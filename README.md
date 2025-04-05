# Jumbo 🚀

**The smarter, faster fetch alternative** - A lightweight HTTP client with built-in retries, caching, and GraphQL support for Node.js and browsers.

[![npm](https://img.shields.io/npm/v/jumbo-client)](https://www.npmjs.com/package/jumbo-client)
[![Bundle Size](https://img.shields.io/bundlephobia/minzip/jumbo-client)](https://bundlephobia.com/package/jumbo-client)
[![License](https://img.shields.io/npm/l/jumbo-client)](LICENSE)

## Features ✨

- ⚡ **Blazing fast** - 40% smaller than Axios
- 🔄 **Auto-retry** with exponential backoff
- 💾 **Smart caching** (memory or Redis)
- 🎯 **GraphQL support** out of the box
- 🛡 **TypeScript ready** with full type safety
- 🌐 **Isomorphic** works in Node.js and browsers
- 🔌 **Pluggable** middleware system

## Installation

```bash
npm install jumbo-client
# or
yarn add jumbo-client
# or
pnpm add jumbo-client
```

## Quick Start

### Basic Usage

```typescript
import Jumbo from 'jumbo-client';

const api = new Jumbo({
  baseURL: 'https://api.example.com',
  retries: 3,    // Auto-retry failed requests
  timeout: 5000  // Timeout after 5 seconds
});

// GET request
const user = await api.get('/users/1');

// POST request
await api.post('/users', { name: 'Alice' });
```

### Advanced Features

**GraphQL Support:**
```typescript
const data = await api.graphql(`
  query GetUser($id: ID!) {
    user(id: $id) { 
      id
      name
      email
    }
  }
`, { id: 123 });
```

**Caching Responses:**
```typescript
const api = new Jumbo({
  cache: {
    adapter: 'memory', // or 'redis'
    ttl: 300 // 5 minutes
  }
});

// First call hits the network, subsequent calls use cache
const products = await api.get('/products', { 
  cacheKey: 'all-products' // Optional custom cache key
});
```

**Custom Middleware:**
```typescript
api.use(async (request, next) => {
  // Add auth token to every request
  request.headers.Authorization = `Bearer ${token}`;
  return next(request);
});
```

## Benchmarks 📊

| Operation       | Jumbo  | Axios  | Fetch  |
|----------------|-------|--------|--------|
| Simple GET     | 12ms  | 18ms   | 15ms   |
| Retry (3x)     | ✅    | ❌     | ❌     |
| Cache Hit      | 0.5ms | ❌     | ❌     |
| Bundle Size    | 6.2kb | 13.4kb | Built-in |

## Why Jumbo? 🤔

1. **Simpler API** - No need for multiple libraries (Axios + Apollo + retry libs)
2. **Smarter Defaults** - Built-in solutions for common needs
3. **Future Ready** - Designed for serverless and edge environments
4. **Type Safe** - Full TypeScript support out of the box

## API Reference

### `new Jumbo(options)`

| Option       | Type               | Default | Description                          |
|-------------|--------------------|---------|--------------------------------------|
| baseURL     | string             | -       | Base URL for all requests            |
| retries     | number             | 0       | Number of retry attempts             |
| timeout     | number             | 30000   | Request timeout in ms                |
| cache       | CacheOptions       | -       | Cache configuration                  |
| headers     | Record<string, string> | {}    | Default headers                     |

### Instance Methods

- `.get(url, config?)`
- `.post(url, body, config?)`
- `.put(url, body, config?)`
- `.patch(url, body, config?)`
- `.delete(url, config?)`
- `.graphql(query, variables?)`
- `.use(middleware)`

## License

MIT © [Lord-Samuel](https://github.com/Lord-Samuel)

---
