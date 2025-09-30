# AuthFramework JavaScript SDK

The official JavaScript/TypeScript SDK for AuthFramework authentication and authorization service.

[![npm version](https://badge.fury.io/js/%40authframework%2Fjs-sdk.svg)](https://badge.fury.io/js/%40authframework%2Fjs-sdk)
[![TypeScript](https://img.shields.io/badge/%3C%2F%3E-TypeScript-%230074c1.svg)](http://www.typescriptlang.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Tests](https://github.com/authframework/authframework-js/workflows/CI/badge.svg)](https://github.com/authframework/authframework-js/actions)
[![Coverage](https://codecov.io/gh/authframework/authframework-js/branch/main/graph/badge.svg)](https://codecov.io/gh/authframework/authframework-js)

## Features

- 🔐 **Complete Authentication**: Login, logout, registration, password reset
- 🛡️ **Authorization**: Role-based access control (RBAC) and permissions  
- 🔑 **Token Management**: JWT token validation and refresh
- 👤 **User Management**: Profile management and user operations
- 🔧 **Admin Operations**: User administration and system management
- 🌐 **Framework Agnostic**: Works with React, Vue, Angular, Node.js, and vanilla JS
- ⚡ **Modern APIs**: Promise-based with async/await support
- 📦 **Tree Shakeable**: ESM and CommonJS builds with optimized bundles
- 🧪 **TypeScript First**: Full type definitions included
- 🚀 **Zero Dependencies**: Lightweight with no external dependencies
- 📱 **Universal**: Works in browsers, Node.js, React Native, and Electron

## Installation

```bash
npm install @authframework/js-sdk
```

```bash
yarn add @authframework/js-sdk
```

```bash
pnpm add @authframework/js-sdk
```

## Quick Start

### Browser/Frontend Usage

```typescript
import { AuthFrameworkClient } from '@authframework/js-sdk';

const client = new AuthFrameworkClient({
  baseUrl: 'https://your-auth-server.com',
  apiKey: 'your-api-key' // Optional
});

// Login
try {
  const response = await client.auth.login('username', 'password');
  console.log('Access token:', response.accessToken);
  
  // Get user profile
  const profile = await client.auth.getProfile();
  console.log('User:', profile.username);
  
  // Validate token
  const validation = await client.tokens.validate();
  console.log('Token valid:', validation.valid);
} catch (error) {
  console.error('Auth error:', error.message);
}
```

### Node.js Usage

```javascript
const { AuthFrameworkClient } = require('@authframework/js-sdk');

const client = new AuthFrameworkClient({
  baseUrl: 'https://your-auth-server.com',
  apiKey: process.env.AUTHFRAMEWORK_API_KEY
});

async function main() {
  try {
    // Admin operations
    const stats = await client.admin.getSystemStats();
    console.log('System stats:', stats);
    
    // Create user
    const newUser = await client.admin.createUser({
      username: 'newuser',
      email: 'newuser@example.com',
      password: 'securepassword'
    });
    console.log('Created user:', newUser.id);
  } catch (error) {
    console.error('Error:', error.message);
  }
}

main();
```

### React Hook Example

```typescript
import { useState, useEffect } from 'react';
import { AuthFrameworkClient } from '@authframework/js-sdk';

const client = new AuthFrameworkClient({
  baseUrl: 'https://your-auth-server.com'
});

function useAuth() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    checkAuthStatus();
  }, []);

  const checkAuthStatus = async () => {
    try {
      const token = localStorage.getItem('authToken');
      if (token) {
        client.setAccessToken(token);
        const profile = await client.auth.getProfile();
        setUser(profile);
      }
    } catch (error) {
      localStorage.removeItem('authToken');
    } finally {
      setLoading(false);
    }
  };

  const login = async (username: string, password: string) => {
    const response = await client.auth.login(username, password);
    localStorage.setItem('authToken', response.accessToken);
    setUser(response.user);
    return response;
  };

  const logout = async () => {
    await client.auth.logout();
    localStorage.removeItem('authToken');
    setUser(null);
  };

  return { user, loading, login, logout };
}

// Usage in component
function App() {
  const { user, loading, login, logout } = useAuth();

  if (loading) return <div>Loading...</div>;

  return (
    <div>
      {user ? (
        <div>
          <p>Welcome, {user.username}!</p>
          <button onClick={logout}>Logout</button>
        </div>
      ) : (
        <LoginForm onLogin={login} />
      )}
    </div>
  );
}
```

### Vue Composition API Example

```typescript
import { ref, onMounted } from 'vue';
import { AuthFrameworkClient } from '@authframework/js-sdk';

const client = new AuthFrameworkClient({
  baseUrl: 'https://your-auth-server.com'
});

export function useAuth() {
  const user = ref(null);
  const loading = ref(true);

  onMounted(async () => {
    await checkAuthStatus();
  });

  const checkAuthStatus = async () => {
    try {
      const token = localStorage.getItem('authToken');
      if (token) {
        client.setAccessToken(token);
        const profile = await client.auth.getProfile();
        user.value = profile;
      }
    } catch (error) {
      localStorage.removeItem('authToken');
    } finally {
      loading.value = false;
    }
  };

  const login = async (username: string, password: string) => {
    const response = await client.auth.login(username, password);
    localStorage.setItem('authToken', response.accessToken);
    user.value = response.user;
    return response;
  };

  return { user, loading, login };
}
```

## API Reference

### AuthFrameworkClient

```typescript
const client = new AuthFrameworkClient({
  baseUrl: string;           // Required: Your AuthFramework server URL
  apiKey?: string;          // Optional: API key for server-to-server auth
  timeout?: number;         // Optional: Request timeout in ms (default: 30000)
  retries?: number;         // Optional: Max retry attempts (default: 3)
});
```

### Authentication Methods

```typescript
// Login
await client.auth.login(username: string, password: string, rememberMe?: boolean);

// Logout
await client.auth.logout();

// Register
await client.auth.register(username: string, email: string, password: string, userData?: object);

// Get profile
await client.auth.getProfile();

// Change password
await client.auth.changePassword(currentPassword: string, newPassword: string);

// Reset password request
await client.auth.resetPasswordRequest(email: string);

// Reset password confirm
await client.auth.resetPasswordConfirm(token: string, newPassword: string);
```

### Token Management

```typescript
// Validate token
await client.tokens.validate(token?: string);

// Refresh token
await client.tokens.refresh(refreshToken: string);

// Set access token
client.setAccessToken(token: string);

// Clear access token
client.clearAccessToken();
```

### Admin Operations

```typescript
// Get system stats
await client.admin.getSystemStats();

// Create user
await client.admin.createUser(userData: object);

// Get users
await client.admin.getUsers(filters?: object);

// Update user
await client.admin.updateUser(userId: string, userData: object);

// Delete user
await client.admin.deleteUser(userId: string);
```

## Error Handling

The SDK provides structured error handling:

```typescript
import { AuthFrameworkError } from '@authframework/js-sdk';

try {
  await client.auth.login('username', 'password');
} catch (error) {
  if (error instanceof AuthFrameworkError) {
    console.log('Error code:', error.code);
    console.log('Error message:', error.message);
    console.log('Status code:', error.statusCode);
    console.log('Details:', error.details);
  }
}
```

## Configuration

### Environment Variables

```bash
# For Node.js applications
AUTHFRAMEWORK_BASE_URL=https://your-auth-server.com
AUTHFRAMEWORK_API_KEY=your-api-key
AUTHFRAMEWORK_TIMEOUT=30000
```

### TypeScript Configuration

Add to your `tsconfig.json`:

```json
{
  "compilerOptions": {
    "moduleResolution": "node",
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true
  }
}
```

## Framework Integrations

- **React**: Hooks and context providers
- **Vue**: Composition API composables
- **Angular**: Services and guards
- **Express**: Middleware for route protection
- **Next.js**: API route helpers and middleware
- **Nuxt**: Plugins and middleware

See the [examples](./examples/) directory for detailed integration guides.

## Development

### Prerequisites

- Node.js 16+
- npm, yarn, or pnpm

### Setup

```bash
# Clone the repository
git clone https://github.com/authframework/authframework-js.git
cd authframework-js

# Install dependencies
npm install

# Build the library
npm run build

# Run tests
npm test

# Run tests with coverage
npm run test:coverage

# Lint code
npm run lint

# Type check
npm run typecheck
```

### Project Structure

```
src/
├── auth/           # Authentication methods
├── admin/          # Admin operations
├── tokens/         # Token management
├── types/          # TypeScript definitions
├── errors/         # Error classes
├── utils/          # Utility functions
└── index.ts        # Main export file
```

## Contributing

Please see our [Contributing Guide](CONTRIBUTING.md) for details on how to contribute to this project.

## Support

- **Documentation**: [https://authframework-js.readthedocs.io/](https://authframework-js.readthedocs.io/)
- **Issues**: [GitHub Issues](https://github.com/authframework/authframework-js/issues)
- **Discussions**: [GitHub Discussions](https://github.com/authframework/authframework-js/discussions)
- **Security**: Report security issues to security@authframework.dev

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for a list of changes and version history.