# GraphQL Node.js Server

A modern GraphQL server implementation using Node.js, Express, and Apollo Server.

## Features

- Express.js server setup with GraphQL middleware
- Apollo Server integration
- Type definitions and resolvers structure
- Authentication middleware support
- Database integration examples
- Error handling
- Environment configuration

## Prerequisites

- Node.js (v14.0.0 or higher)
- npm or yarn package manager
- MongoDB (optional, if using MongoDB as database)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/graphql-nodejs-server.git
cd graphql-nodejs-server
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file in the root directory:
```
PORT=4000
NODE_ENV=development
DATABASE_URL=mongodb://localhost:27017/your-database
JWT_SECRET=your-secret-key
```

## Project Structure

```
├── src/
│   ├── schema/
│   │   ├── typeDefs/
│   │   │   ├── user.graphql
│   │   │   └── index.js
│   │   ├── resolvers/
│   │   │   ├── user.js
│   │   │   └── index.js
│   ├── models/
│   │   └── User.js
│   ├── middleware/
│   │   ├── auth.js
│   │   └── errorHandler.js
│   ├── config/
│   │   └── database.js
│   └── server.js
├── package.json
└── .env
```

## Basic Server Setup

```javascript
const express = require('express');
const { ApolloServer } = require('apollo-server-express');
const { typeDefs } = require('./schema/typeDefs');
const { resolvers } = require('./schema/resolvers');

const app = express();

const server = new ApolloServer({
  typeDefs,
  resolvers,
  context: ({ req }) => ({ req })
});

async function startServer() {
  await server.start();
  server.applyMiddleware({ app });
  
  app.listen({ port: process.env.PORT || 4000 }, () =>
    console.log(`🚀 Server ready at http://localhost:4000${server.graphqlPath}`)
  );
}

startServer();
```

## Example Schema

```graphql
type User {
  id: ID!
  username: String!
  email: String!
  createdAt: String!
}

type Query {
  getUser(id: ID!): User
  getUsers: [User]!
}

type Mutation {
  createUser(username: String!, email: String!, password: String!): User!
  loginUser(email: String!, password: String!): AuthPayload!
}

type AuthPayload {
  token: String!
  user: User!
}
```

## Example Resolver

```javascript
const resolvers = {
  Query: {
    getUser: async (_, { id }, context) => {
      // Implementation
    },
    getUsers: async () => {
      // Implementation
    }
  },
  Mutation: {
    createUser: async (_, { username, email, password }) => {
      // Implementation
    },
    loginUser: async (_, { email, password }) => {
      // Implementation
    }
  }
};
```

## Authentication Middleware

```javascript
const jwt = require('jsonwebtoken');

const auth = (context) => {
  const authHeader = context.req.headers.authorization;
  if (authHeader) {
    const token = authHeader.split('Bearer ')[1];
    if (token) {
      try {
        const user = jwt.verify(token, process.env.JWT_SECRET);
        return user;
      } catch (err) {
        throw new Error('Invalid/Expired token');
      }
    }
    throw new Error('Authentication token must be Bearer [token]');
  }
  throw new Error('Authorization header must be provided');
};
```

## Available Scripts

- `npm start`: Starts the production server
- `npm run dev`: Starts the development server with nodemon
- `npm test`: Runs the test suite
- `npm run lint`: Runs ESLint

## Testing

The project uses Jest for testing. Run tests with:

```bash
npm test
```

Example test:

```javascript
describe('User Queries', () => {
  it('should fetch a user by id', async () => {
    const result = await testServer.executeOperation({
      query: GET_USER_QUERY,
      variables: { id: 'testUserId' },
    });
    expect(result.errors).toBeUndefined();
    expect(result.data?.getUser).toBeDefined();
  });
});
```

## Error Handling

The server implements custom error handling for common GraphQL errors:

- Authentication errors
- Validation errors
- Database errors
- Network errors

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details

## Support

For support, email support@yourdomain.com or open an issue in the repository.
