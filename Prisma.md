# Prisma Commands Reference Guide

## Installation and Setup

### Initial Setup
```bash
# Install Prisma CLI
npm install prisma --save-dev

# Initialize Prisma in your project
npx prisma init

# Install Prisma Client
npm install @prisma/client
```

## Schema Management

### Schema Commands
```bash
# Generate Prisma Client
npx prisma generate

# Format schema file
npx prisma format

# Validate schema
npx prisma validate
```

## Database Management

### Migration Commands
```bash
# Create a migration
npx prisma migrate dev --name init

# Reset database
npx prisma migrate reset

# Deploy migration to production
npx prisma migrate deploy

# Check migration status
npx prisma migrate status

# Create SQL migration without applying
npx prisma migrate diff

# Apply pending migrations
npx prisma migrate resolve
```

### Database Operations
```bash
# Push schema changes directly to database (dev only)
npx prisma db push

# Pull database schema into Prisma schema
npx prisma db pull

# Launch Prisma Studio
npx prisma studio

# Seed database
npx prisma db seed
```

## Common Schema Examples

### Basic Model Definition
```prisma
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String?
  posts     Post[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String?
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
}
```

### Common Field Types
```prisma
model Example {
  id        Int       @id @default(autoincrement())
  string    String
  text      String    @db.Text
  number    Int
  decimal   Decimal   @db.Decimal(10, 2)
  boolean   Boolean
  datetime  DateTime
  enum      Role      @default(USER)
  json      Json
  bytes     Bytes
}

enum Role {
  USER
  ADMIN
}
```

### Relations
```prisma
// One-to-Many
model User {
  id     Int     @id @default(autoincrement())
  posts  Post[]
}

model Post {
  id       Int    @id @default(autoincrement())
  author   User   @relation(fields: [authorId], references: [id])
  authorId Int
}

// Many-to-Many
model Category {
  id    Int     @id @default(autoincrement())
  posts Post[]  @relation("PostToCategory")
}

model Post {
  id         Int        @id @default(autoincrement())
  categories Category[] @relation("PostToCategory")
}

// One-to-One
model User {
  id       Int      @id @default(autoincrement())
  profile  Profile?
}

model Profile {
  id     Int   @id @default(autoincrement())
  user   User  @relation(fields: [userId], references: [id])
  userId Int   @unique
}
```

## Prisma Client Examples

### Basic CRUD Operations
```typescript
// Create
const user = await prisma.user.create({
  data: {
    email: 'user@example.com',
    name: 'John Doe',
  },
});

// Read
const users = await prisma.user.findMany();
const user = await prisma.user.findUnique({
  where: {
    id: 1,
  },
});

// Update
const updatedUser = await prisma.user.update({
  where: {
    id: 1,
  },
  data: {
    name: 'Jane Doe',
  },
});

// Delete
const deletedUser = await prisma.user.delete({
  where: {
    id: 1,
  },
});
```

### Advanced Queries
```typescript
// Include relations
const userWithPosts = await prisma.user.findUnique({
  where: {
    id: 1,
  },
  include: {
    posts: true,
  },
});

// Select specific fields
const userEmails = await prisma.user.findMany({
  select: {
    email: true,
  },
});

// Filtering
const filteredPosts = await prisma.post.findMany({
  where: {
    published: true,
    title: {
      contains: 'prisma',
    },
  },
});

// Pagination
const paginatedUsers = await prisma.user.findMany({
  skip: 10,
  take: 20,
});

// Ordering
const orderedPosts = await prisma.post.findMany({
  orderBy: {
    createdAt: 'desc',
  },
});
```

## Best Practices

1. Schema Design
   - Use meaningful model and field names
   - Add appropriate indexes for performance
   - Use relations appropriately
   - Document schema with comments

2. Migrations
   - Always review migration files before applying
   - Use meaningful migration names
   - Test migrations on development first
   - Keep migrations small and focused

3. Client Usage
   - Reuse Prisma Client instance
   - Use transactions for related operations
   - Handle errors appropriately
   - Implement proper connection pooling

4. Performance
   - Use select to fetch only needed fields
   - Implement pagination for large datasets
   - Add indexes for frequently queried fields
   - Use batch operations when possible

## Common Issues and Solutions

1. Connection Issues
   ```bash
   # Check database URL
   npx prisma db pull
   
   # Reset database and apply migrations
   npx prisma migrate reset
   ```

2. Schema Conflicts
   ```bash
   # Reset Prisma schema from database
   npx prisma db pull
   
   # Force push schema changes (dev only)
   npx prisma db push --force-reset
   ```

3. Migration Issues
   ```bash
   # Reset migration history
   npx prisma migrate reset
   
   # Skip specific migration
   npx prisma migrate resolve --skip
   ```

## Environment Setup

### Environment Variables
```env
# .env
DATABASE_URL="postgresql://user:password@localhost:5432/mydb?schema=public"
```

### Database Connection URLs
```plaintext
# PostgreSQL
postgresql://user:password@localhost:5432/mydb?schema=public

# MySQL
mysql://user:password@localhost:3306/mydb

# SQLite
file:./dev.db

# MongoDB
mongodb://user:password@localhost:27017/mydb?authSource=admin
```
