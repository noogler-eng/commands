# Prisma Connection Pooling Guide

## Table of Contents
- [Understanding Connection Pools](#understanding-connection-pools)
- [Connection Pool Configuration](#connection-pool-configuration)
- [Best Practices](#best-practices)
- [Implementation Examples](#implementation-examples)
- [Monitoring and Optimization](#monitoring-and-optimization)
- [Troubleshooting](#troubleshooting)

## Understanding Connection Pools

### What is Connection Pooling?
Connection pooling is a technique that maintains a cache of database connections that can be reused when future requests to the database are required. Instead of opening a new connection for every database operation, connections are reused from the pool, which:
- Reduces the time spent establishing new connections
- Manages concurrent database connections efficiently
- Improves application performance
- Prevents database overload

### How Prisma Handles Connection Pooling

#### Connection Lifecycle
1. **Pool Initialization**
   - When your application starts, Prisma creates a connection pool
   - The pool size is determined by your configuration
   - Connections are established and added to the pool

2. **Connection Usage**
   - When a query is made, Prisma requests a connection from the pool
   - If available, an idle connection is used
   - If no connections are available, the request waits in a queue

3. **Connection Release**
   - After query completion, the connection returns to the pool
   - The connection remains open for future use
   - Idle connections may be closed based on configuration

## Connection Pool Configuration

### Basic Configuration
```typescript
// schema.prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
  previewFeatures = ["metrics"]
}
```

### Environment Variables
```env
# .env
DATABASE_URL="postgresql://user:password@localhost:5432/mydb?connection_limit=20&pool_timeout=30"
```

### Connection Pool Parameters

#### Primary Parameters
```typescript
// prisma/client.ts
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient({
  datasources: {
    db: {
      url: process.env.DATABASE_URL,
    },
  },
  // Connection pool configuration
  connection: {
    pool: {
      min: 2,         // Minimum connections
      max: 10,        // Maximum connections
      idleTimeout: 60 // Seconds before idle connection is closed
    }
  }
})
```

#### Advanced Configuration Options
```typescript
const prisma = new PrismaClient({
  log: ['query', 'info', 'warn', 'error'],
  datasources: {
    db: {
      url: process.env.DATABASE_URL,
    },
  },
  connection: {
    pool: {
      min: 2,
      max: 10,
      idleTimeout: 60,
      acquireTimeout: 60,    // Seconds to wait for connection
      reapIntervalMillis: 1000, // How often to check for idle connections
      createTimeoutMillis: 30000, // Timeout for creating new connections
      createRetryIntervalMillis: 200, // Retry interval for connection creation
      propagateCreateError: false, // Whether to propagate connection creation errors
    }
  }
})
```

## Best Practices

### 1. Pool Sizing Guidelines
```typescript
// For web applications
const prisma = new PrismaClient({
  connection: {
    pool: {
      min: Math.floor(process.env.POOL_MIN || 2),
      max: Math.floor(process.env.POOL_MAX || 10)
    }
  }
})

// Calculate based on resources
const calculatePoolSize = () => {
  const maxConnections = 100 // Database max connections
  const applicationInstances = 2 // Number of application instances
  return Math.floor(maxConnections / applicationInstances)
}
```

### 2. Connection Management
```typescript
// Proper connection handling
async function main() {
  try {
    // Your database operations
    await prisma.$connect()
    
    // Perform operations
    
  } finally {
    // Always disconnect when done
    await prisma.$disconnect()
  }
}

// Singleton pattern for PrismaClient
import { PrismaClient } from '@prisma/client'

const globalForPrisma = global as unknown as { prisma: PrismaClient }

export const prisma =
  globalForPrisma.prisma ||
  new PrismaClient({
    log: ['query'],
  })

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma
```

## Implementation Examples

### 1. Basic Implementation
```typescript
// src/database.ts
import { PrismaClient } from '@prisma/client'

export const prisma = new PrismaClient({
  connection: {
    pool: {
      min: 2,
      max: 10
    }
  }
})

// Usage in API route
async function handleRequest(req, res) {
  try {
    const users = await prisma.user.findMany()
    res.json(users)
  } catch (error) {
    res.status(500).json({ error: 'Database error' })
  }
}
```

### 2. Advanced Implementation with Metrics
```typescript
// src/database.ts
import { PrismaClient } from '@prisma/client'
import { metrics } from '@opentelemetry/api'

const meter = metrics.getMeter('prisma-pool')
const activeConnections = meter.createUpDownCounter('prisma.pool.connections.active')

const prisma = new PrismaClient({
  log: ['query'],
  connection: {
    pool: {
      min: 2,
      max: 10,
      onCreate: () => {
        activeConnections.add(1)
      },
      onDelete: () => {
        activeConnections.add(-1)
      }
    }
  }
})

export { prisma }
```

## Monitoring and Optimization

### 1. Connection Pool Metrics
```typescript
// Monitor pool status
const poolMetrics = await prisma.$metrics.json()
console.log({
  activeConnections: poolMetrics.pools.active,
  idleConnections: poolMetrics.pools.idle,
  waitingRequests: poolMetrics.pools.waiting
})
```

### 2. Performance Monitoring
```typescript
// Add query timing middleware
prisma.$use(async (params, next) => {
  const start = Date.now()
  const result = await next(params)
  const end = Date.now()
  
  console.log(`Query ${params.model}.${params.action} took ${end - start}ms`)
  return result
})
```

## Troubleshooting

### Common Issues and Solutions

1. **Connection Timeouts**
```typescript
// Increase timeout settings
const prisma = new PrismaClient({
  connection: {
    pool: {
      acquireTimeout: 120, // Increase timeout to 2 minutes
      createTimeoutMillis: 60000 // Increase creation timeout
    }
  }
})
```

2. **Pool Exhaustion**
```typescript
// Implement retry logic
const withRetry = async (operation: () => Promise<any>, retries = 3) => {
  for (let i = 0; i < retries; i++) {
    try {
      return await operation()
    } catch (error) {
      if (error.code === 'P2024' && i < retries - 1) {
        await new Promise(resolve => setTimeout(resolve, 1000 * Math.pow(2, i)))
        continue
      }
      throw error
    }
  }
}
```

3. **Memory Leaks**
```typescript
// Implement connection cleanup
process.on('SIGINT', async () => {
  await prisma.$disconnect()
  process.exit(0)
})

// Regular health checks
setInterval(async () => {
  try {
    await prisma.$queryRaw`SELECT 1`
  } catch (error) {
    console.error('Database health check failed:', error)
    // Implement recovery logic
  }
}, 30000)
```

### Performance Tips

1. **Query Optimization**
```typescript
// Use transactions for multiple operations
await prisma.$transaction(async (tx) => {
  await tx.user.create({ data: userData })
  await tx.profile.create({ data: profileData })
})

// Batch operations
await prisma.user.createMany({
  data: usersData,
  skipDuplicates: true
})
```

2. **Connection Reuse**
```typescript
// Implement connection sharing in middleware
export async function withPrisma(req, res, next) {
  req.prisma = prisma
  try {
    await next()
  } finally {
    // Connection returns to pool automatically
  }
}
```
