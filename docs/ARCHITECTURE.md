# Architecture Overview

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Client Layer                         │
│              (React + TypeScript + Vite)                │
├─────────────────────────────────────────────────────────┤
│                   Network Layer                         │
│            (Axios + TanStack Query/SWR)                 │
├─────────────────────────────────────────────────────────┤
│                   API Gateway                           │
│                 (Express.js + Routes)                   │
├─────────────────────────────────────────────────────────┤
│                  Service Layer                          │
│         (Business Logic + Authentication)               │
├─────────────────────────────────────────────────────────┤
│                   Data Layer                            │
│           (Prisma ORM + PostgreSQL/MongoDB)             │
└─────────────────────────────────────────────────────────┘
```

## Frontend Architecture

### Directory Structure

```
src/
├── components/
│   ├── auth/              # Authentication components
│   ├── dashboard/         # Dashboard components
│   ├── npc/              # NPC-related components
│   ├── org/              # Organization components
│   ├── shared/           # Reusable components
│   └── layout/           # Layout components
├── pages/                # Page-level components
├── hooks/                # Custom React hooks
│   ├── useAuth.ts
│   ├── useNPC.ts
│   └── ...
├── services/             # API service calls
│   ├── auth.service.ts
│   ├── npc.service.ts
│   └── ...
├── store/                # State management (Zustand)
│   ├── authStore.ts
│   ├── npcStore.ts
│   └── ...
├── types/                # TypeScript definitions
│   ├── auth.types.ts
│   ├── npc.types.ts
│   └── ...
├── utils/                # Utility functions
│   ├── constants.ts
│   ├── helpers.ts
│   └── ...
├── styles/               # Global styles
│   └── globals.css
├── App.tsx               # Main App component
└── main.tsx              # Entry point
```

### Component Architecture

Each component follows this pattern:

```typescript
// Component.tsx
import React from 'react';
import { useHook } from '@/hooks/useHook';
import { Button } from '@/components/shared/Button';
import styles from './Component.module.css';

interface ComponentProps {
  title: string;
  onAction: () => void;
}

export const Component: React.FC<ComponentProps> = ({
  title,
  onAction,
}) => {
  const { state, action } = useHook();

  return (
    <div className={styles.container}>
      <h1>{title}</h1>
      <Button onClick={onAction}>Click Me</Button>
    </div>
  );
};

export default Component;
```

### Data Flow

```
User Interaction (UI)
        ↓
Component Event Handler
        ↓
Hook/Store Update
        ↓
API Service Call
        ↓
Backend API
        ↓
Response
        ↓
Store Update
        ↓
Re-render Component
```

## Backend Architecture

### API Structure

```
server/
├── routes/
│   ├── auth.routes.ts
│   ├── npc.routes.ts
│   ├── org.routes.ts
│   └── index.ts
├── controllers/
│   ├── auth.controller.ts
│   ├── npc.controller.ts
│   └── ...
├── services/
│   ├── auth.service.ts
│   ├── npc.service.ts
│   └── ...
├── middleware/
│   ├── auth.middleware.ts
│   ├── validation.middleware.ts
│   └── ...
├── models/
│   ├── User.ts
│   ├── NPC.ts
│   └── ...
├── types/
│   └── index.ts
├── utils/
│   └── ...
└── index.ts
```

### Request-Response Cycle

```
Client Request
    ↓
Route Handler
    ↓
Middleware (Auth, Validation)
    ↓
Controller
    ↓
Service Layer
    ↓
Database
    ↓
Service Response
    ↓
Controller Response
    ↓
Client Response
```

## Database Design

### Entities

#### Users
- id (PK)
- email (UNIQUE)
- password (hashed)
- firstName
- lastName
- role (enum: admin, member, viewer)
- createdAt
- updatedAt

#### NPCs
- id (PK)
- organizationId (FK)
- createdById (FK)
- name
- description
- type (enum: mage, warrior, ranger, etc.)
- level
- stats (JSON)
- attributes (JSON)
- status (enum: active, inactive, archived)
- createdAt
- updatedAt

#### Organizations
- id (PK)
- name
- description
- ownerId (FK)
- settings (JSON)
- createdAt
- updatedAt

#### Relationships
```
User 1:M NPC
User 1:M Organization
Organization 1:M NPC
Organization M:N User (members)
```

## State Management

### Zustand Stores

```typescript
// authStore.ts
import { create } from 'zustand';

interface AuthState {
  user: User | null;
  token: string | null;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
  isLoading: boolean;
}

export const useAuthStore = create<AuthState>((set) => ({
  user: null,
  token: null,
  isLoading: false,
  login: async (email, password) => {
    // Implementation
  },
  logout: () => {
    set({ user: null, token: null });
  },
}));
```

## Authentication Flow

```
1. User enters credentials
        ↓
2. Frontend sends login request
        ↓
3. Backend validates credentials
        ↓
4. Backend generates JWT token
        ↓
5. Token stored in localStorage/cookie
        ↓
6. Token included in subsequent requests
        ↓
7. Backend validates token
        ↓
8. Request processed if valid
```

## Caching Strategy

### Client-Side Caching

- **React Query**: API response caching
- **localStorage**: User preferences
- **sessionStorage**: Temporary data

### Server-Side Caching

- **Redis**: Session storage
- **Database Indexing**: Query optimization
- **ETags**: Conditional requests

## Error Handling

### Frontend
```typescript
try {
  const response = await fetchAPI('/endpoint');
  handleSuccess(response);
} catch (error) {
  if (error instanceof ValidationError) {
    showValidationError(error.details);
  } else if (error instanceof AuthError) {
    redirectToLogin();
  } else {
    showErrorNotification(error.message);
  }
}
```

### Backend
```typescript
try {
  const result = await service.execute();
  return res.json({ success: true, data: result });
} catch (error) {
  if (error instanceof ValidationError) {
    return res.status(400).json({ error: error.message });
  }
  return res.status(500).json({ error: 'Internal Server Error' });
}
```

## Performance Optimization

### Frontend
- Code splitting with dynamic imports
- Lazy loading of components
- Image optimization
- CSS minification
- Bundle size optimization

### Backend
- Query optimization with indexes
- Connection pooling
- Response caching
- Pagination for large datasets
- Compression (gzip)

## Security Architecture

### Authentication
- JWT tokens with expiration
- Refresh token rotation
- Secure password hashing (bcrypt)

### Authorization
- Role-based access control (RBAC)
- Middleware-based permission checks
- Row-level security

### Data Protection
- HTTPS/TLS encryption
- SQL injection prevention
- XSS protection
- CSRF tokens

## Deployment Architecture

```
Developer Workstation
        ↓
GitHub Repository
        ↓
CI/CD Pipeline (GitHub Actions)
        ↓
Tests & Build
        ↓
Production Deployment
        ├── Frontend (Vercel/CDN)
        ├── Backend (Docker/K8s)
        └── Database (Managed Service)
```

## Scalability Considerations

### Horizontal Scaling
- Stateless API design
- Load balancing
- Database replication
- Caching layer

### Vertical Scaling
- Database optimization
- Query caching
- Index optimization
- Connection pooling

## Monitoring & Logging

### Application Monitoring
- Error tracking (Sentry)
- Performance monitoring (Datadog)
- User analytics (Mixpanel)

### Logging
- Structured logging
- Log aggregation
- Error logs
- Audit logs

---

**Last Updated**: June 10, 2026
