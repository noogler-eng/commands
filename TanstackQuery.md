# TanStack Query Implementation Guide

A comprehensive guide for implementing TanStack Query (formerly React Query) in your React applications.

## Features

- Powerful server state management
- Automatic background data refetching
- Window focus refetching
- Request deduplication
- Paginated/infinite queries
- Parallel queries
- Mutation handling
- Error handling with retries
- Loading states
- Prefetching capabilities

## Installation

```bash
npm install @tanstack/react-query
# or
yarn add @tanstack/react-query
```

## Basic Setup

First, wrap your application with `QueryClientProvider`:

```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'

// Create a client
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 1000 * 60 * 5, // 5 minutes
      cacheTime: 1000 * 60 * 30, // 30 minutes
      retry: 1,
      refetchOnWindowFocus: false,
    },
  },
})

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <YourApp />
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  )
}
```

## Basic Query Example

```jsx
import { useQuery } from '@tanstack/react-query'

function TodoList() {
  const { data, isLoading, error } = useQuery({
    queryKey: ['todos'],
    queryFn: async () => {
      const response = await fetch('https://api.example.com/todos')
      if (!response.ok) {
        throw new Error('Network response was not ok')
      }
      return response.json()
    },
  })

  if (isLoading) return <div>Loading...</div>
  if (error) return <div>Error: {error.message}</div>

  return (
    <ul>
      {data.map(todo => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  )
}
```

## Mutation Example

```jsx
import { useMutation, useQueryClient } from '@tanstack/react-query'

function AddTodo() {
  const queryClient = useQueryClient()
  
  const mutation = useMutation({
    mutationFn: async (newTodo) => {
      const response = await fetch('https://api.example.com/todos', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(newTodo),
      })
      return response.json()
    },
    onSuccess: (data) => {
      // Invalidate and refetch
      queryClient.invalidateQueries({ queryKey: ['todos'] })
      // Or update the cache directly
      queryClient.setQueryData(['todos'], (old) => [...old, data])
    },
  })

  return (
    <form onSubmit={(e) => {
      e.preventDefault()
      mutation.mutate({ title: 'New Todo' })
    }}>
      <button type="submit">Add Todo</button>
    </form>
  )
}
```

## Infinite Queries

```jsx
function InfiniteTodoList() {
  const {
    data,
    fetchNextPage,
    hasNextPage,
    isFetchingNextPage,
  } = useInfiniteQuery({
    queryKey: ['todos'],
    queryFn: async ({ pageParam = 0 }) => {
      const response = await fetch(
        `https://api.example.com/todos?page=${pageParam}`
      )
      return response.json()
    },
    getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
  })

  return (
    <>
      {data.pages.map((group, i) => (
        <React.Fragment key={i}>
          {group.todos.map(todo => (
            <p key={todo.id}>{todo.title}</p>
          ))}
        </React.Fragment>
      ))}
      <button
        onClick={() => fetchNextPage()}
        disabled={!hasNextPage || isFetchingNextPage}
      >
        {isFetchingNextPage
          ? 'Loading more...'
          : hasNextPage
          ? 'Load More'
          : 'Nothing more to load'}
      </button>
    </>
  )
}
```

## Parallel Queries

```jsx
function ParallelQueries() {
  const users = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers,
  })

  const posts = useQuery({
    queryKey: ['posts'],
    queryFn: fetchPosts,
  })

  return (
    <div>
      {users.data?.map(user => (
        <div key={user.id}>{user.name}</div>
      ))}
      {posts.data?.map(post => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  )
}
```

## Dependent Queries

```jsx
function DependentQueries({ userId }) {
  const { data: user } = useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUserById(userId),
  })

  const { data: userPosts } = useQuery({
    queryKey: ['posts', user?.id],
    queryFn: () => fetchUserPosts(user.id),
    enabled: !!user, // Only run this query if user exists
  })

  return (
    <div>
      <h1>{user?.name}</h1>
      {userPosts?.map(post => (
        <p key={post.id}>{post.title}</p>
      ))}
    </div>
  )
}
```

## Prefetching Data

```jsx
function PrefetchExample() {
  const queryClient = useQueryClient()

  // Prefetch the todo query
  const prefetchTodo = async (todoId) => {
    await queryClient.prefetchQuery({
      queryKey: ['todo', todoId],
      queryFn: () => fetchTodoById(todoId),
    })
  }

  return (
    <button onMouseEnter={() => prefetchTodo(5)}>
      Hover to Prefetch
    </button>
  )
}
```

## Optimistic Updates

```jsx
function OptimisticTodo() {
  const queryClient = useQueryClient()

  const mutation = useMutation({
    mutationFn: updateTodo,
    onMutate: async (newTodo) => {
      // Cancel outgoing refetches
      await queryClient.cancelQueries({ queryKey: ['todos'] })

      // Snapshot the previous value
      const previousTodos = queryClient.getQueryData(['todos'])

      // Optimistically update to the new value
      queryClient.setQueryData(['todos'], old => {
        return old.map(todo => 
          todo.id === newTodo.id ? newTodo : todo
        )
      })

      // Return context with snapshot
      return { previousTodos }
    },
    onError: (err, newTodo, context) => {
      // If mutation fails, use context to roll back
      queryClient.setQueryData(['todos'], context.previousTodos)
    },
    onSettled: () => {
      // Always refetch after error or success
      queryClient.invalidateQueries({ queryKey: ['todos'] })
    },
  })
}
```

## Custom Hooks Pattern

```jsx
// hooks/useTodos.js
export function useTodos() {
  return useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
  })
}

export function useAddTodo() {
  const queryClient = useQueryClient()
  
  return useMutation({
    mutationFn: addTodo,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['todos'] })
    },
  })
}

// Component
function TodoComponent() {
  const { data: todos } = useTodos()
  const { mutate: addTodo } = useAddTodo()

  return (
    // Your component JSX
  )
}
```

## Tips and Best Practices

1. **Proper Query Keys**
   - Use array syntax for query keys
   - Include all dependencies in the key
   - Keep keys consistent across your application

2. **Error Handling**
   - Always handle loading and error states
   - Use error boundaries for fallback UI
   - Implement retry logic where appropriate

3. **Cache Management**
   - Set appropriate staleTime and cacheTime
   - Use invalidateQueries strategically
   - Implement optimistic updates for better UX

4. **Performance**
   - Use select option to transform data
   - Implement proper prefetching strategies
   - Handle pagination efficiently

## TypeScript Support

```typescript
interface Todo {
  id: number
  title: string
  completed: boolean
}

function TodoList() {
  const { data } = useQuery<Todo[], Error>({
    queryKey: ['todos'],
    queryFn: fetchTodos,
  })
}
```

## Debugging

1. Install and use ReactQueryDevtools
2. Monitor network requests
3. Use proper error boundaries
4. Implement logging where necessary

## Resources

- [Official Documentation](https://tanstack.com/query/latest)
- [GitHub Repository](https://github.com/TanStack/query)
- [Best Practices Guide](https://tanstack.com/query/latest/docs/react/guides/important-defaults)

## License

This guide is licensed under the MIT License.
