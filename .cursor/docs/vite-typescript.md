# Vite + TypeScript Documentation

## Project Setup

### Initial Configuration
```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
```

### TypeScript Configuration
```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

## Vite Configuration
```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src')
    }
  },
  server: {
    port: 3000,
    open: true
  },
  build: {
    outDir: 'dist',
    sourcemap: true,
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom']
        }
      }
    }
  }
})
```

## TypeScript Best Practices

### Type Definitions
```typescript
// Use interfaces for object shapes
interface User {
  id: number
  name: string
  email: string
  role: 'admin' | 'user'
}

// Use type aliases for unions and complex types
type Status = 'loading' | 'success' | 'error'
type ApiResponse<T> = {
  data: T
  status: Status
  message?: string
}

// Use const assertions for literal types
const ROLES = ['admin', 'user', 'guest'] as const
type Role = typeof ROLES[number]
```

### Component Patterns
```typescript
// Props interface
interface ButtonProps {
  variant?: 'primary' | 'secondary'
  size?: 'small' | 'medium' | 'large'
  disabled?: boolean
  onClick?: () => void
  children: React.ReactNode
}

// Component with proper typing
const Button: React.FC<ButtonProps> = ({
  variant = 'primary',
  size = 'medium',
  disabled = false,
  onClick,
  children
}) => {
  return (
    <button
      className={`btn btn-${variant} btn-${size}`}
      disabled={disabled}
      onClick={onClick}
    >
      {children}
    </button>
  )
}
```

### Hooks with TypeScript
```typescript
// Custom hook with proper typing
function useApi<T>(url: string) {
  const [data, setData] = useState<T | null>(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState<string | null>(null)

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(setData)
      .catch(err => setError(err.message))
      .finally(() => setLoading(false))
  }, [url])

  return { data, loading, error }
}
```

## Development Workflow

### Scripts
```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "type-check": "tsc --noEmit"
  }
}
```

### Testing Setup
```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    setupFiles: ['./src/test/setup.ts']
  }
})
```

## Best Practices
- Use strict TypeScript configuration
- Prefer interfaces over types for object shapes
- Use const assertions for literal types
- Implement proper error boundaries
- Use React.memo for performance optimization
- Keep components small and focused
- Use absolute imports with path mapping
- Implement proper loading and error states

## References
- [Vite Documentation](https://vitejs.dev/guide/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
- [Vite Testing Guide](https://vitest.dev/guide/)

