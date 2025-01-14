# Prisma Transactions

This document provides a guide for handling transactions in Prisma, ensuring efficient and consistent database operations. Prisma supports transactions to group multiple database operations into a single unit of work, maintaining data integrity.

---

## Table of Contents

1. [What are Transactions?](#what-are-transactions)
2. [Why Use Transactions?](#why-use-transactions)
3. [Setting Up Prisma](#setting-up-prisma)
4. [Using Transactions in Prisma](#using-transactions-in-prisma)
   - [Atomic Transactions](#atomic-transactions)
   - [Interactive Transactions](#interactive-transactions)
5. [Error Handling](#error-handling)
6. [Best Practices](#best-practices)
7. [Examples](#examples)
8. [References](#references)

---

## What are Transactions?

Transactions are a mechanism to execute multiple database operations as a single, atomic unit. If any operation within the transaction fails, the entire transaction is rolled back, ensuring data consistency.

---

## Why Use Transactions?

Transactions are useful for:

- Ensuring consistency in critical operations.
- Handling multi-step operations where partial completion could leave data in an invalid state.
- Safeguarding against system failures or errors during complex operations.

---

## Setting Up Prisma

Before using transactions, ensure:

1. Prisma is installed and configured in your project.
2. Your `prisma/schema.prisma` file is set up with the required database models.
3. Prisma Client is generated using:
   ```bash
   npx prisma generate
   ```

---

## Using Transactions in Prisma

Prisma offers two types of transactions:

### Atomic Transactions

Atomic transactions group multiple operations together and ensure all of them succeed or none of them are applied.

Example:
```javascript
const result = await prisma.$transaction([
  prisma.user.create({ data: { name: 'Alice' } }),
  prisma.post.create({ data: { title: 'Hello World', authorId: 1 } })
]);
```

### Interactive Transactions

Interactive transactions allow more complex workflows where multiple steps can depend on intermediate results.

Example:
```javascript
await prisma.$transaction(async (prisma) => {
  const user = await prisma.user.create({
    data: { name: 'Bob' },
  });

  await prisma.post.create({
    data: { title: 'First Post', authorId: user.id },
  });
});
```

---

## Error Handling

Always handle errors to ensure proper rollback in case of failures. Prisma automatically rolls back transactions if any operation fails.

Example:
```javascript
try {
  await prisma.$transaction(async (prisma) => {
    // Perform database operations
  });
} catch (error) {
  console.error('Transaction failed:', error);
}
```

---

## Best Practices

1. **Keep Transactions Short**:
   Minimize the time transactions remain open to avoid potential database locks.

2. **Handle Errors Gracefully**:
   Implement robust error-handling mechanisms.

3. **Use Interactive Transactions for Complex Logic**:
   When operations depend on intermediate results, interactive transactions are preferred.

4. **Avoid External API Calls Inside Transactions**:
   External calls can increase transaction time and lead to potential timeouts.

---

## Examples

### Example 1: Atomic Transaction
```javascript
const result = await prisma.$transaction([
  prisma.order.create({ data: { total: 100 } }),
  prisma.payment.create({ data: { amount: 100, orderId: 1 } })
]);
```

### Example 2: Interactive Transaction
```javascript
await prisma.$transaction(async (prisma) => {
  const order = await prisma.order.create({
    data: { total: 150 },
  });

  await prisma.payment.create({
    data: { amount: 150, orderId: order.id },
  });
});
```

---

## References

- [Prisma Transactions Documentation](https://www.prisma.io/docs/concepts/components/prisma-client/transactions)
- [Prisma Client API Reference](https://www.prisma.io/docs/reference/api-reference/prisma-client-reference)

---

Happy Coding!
