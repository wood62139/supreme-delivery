# Development Guide

## Prerequisites

- **Node.js**: 18.x or higher
- **npm**: 9.x or higher
- **Docker**: Latest version
- **Docker Compose**: 2.x or higher
- **Git**: Latest version
- **VS Code** (recommended)

## Initial Setup

### 1. Clone the Repository

```bash
git clone https://github.com/wood62139/supreme-delivery.git
cd supreme-delivery
```

### 2. Install Dependencies

```bash
# Frontend
cd frontend && npm install

# Backend
cd ../backend && npm install
```

### 3. Environment Configuration

```bash
# Copy environment template
cp .env.example .env.local

# Edit configuration (update API keys, database credentials, etc.)
nano .env.local
```

### 4. Start Development Environment

#### Option A: Using Docker Compose (Recommended)

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

#### Option B: Manual Setup

```bash
# Terminal 1: PostgreSQL
docker run --name postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 postgres:15

# Terminal 2: MongoDB
docker run --name mongodb -p 27017:27017 mongo:6.0

# Terminal 3: Redis
docker run --name redis -p 6379:6379 redis:7

# Terminal 4: RabbitMQ
docker run --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12-management

# Terminal 5: Backend
cd backend && npm run dev

# Terminal 6: Frontend
cd frontend && npm run dev
```

### 5. Access Applications

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000
- **API Documentation**: http://localhost:5000/api/docs
- **RabbitMQ Dashboard**: http://localhost:15672

## Project Structure Navigation

### Frontend Development

```bash
cd frontend

# Development server with hot reload
npm run dev

# Type checking
npm run type-check

# Linting
npm run lint

# Format code
npm run format

# Run tests
npm run test

# View test coverage
npm run test:coverage

# Storybook for component development
npm run storybook
```

**Key Directories:**
- `src/components/` - React components
- `src/pages/` - Next.js page routes
- `src/hooks/` - Custom React hooks
- `src/services/` - API client services
- `src/store/` - State management
- `src/types/` - TypeScript type definitions
- `src/styles/` - Global styles and Tailwind config

### Backend Development

```bash
cd backend

# Development server with auto-restart
npm run dev

# Build TypeScript
npm run build

# Type checking
npm run type-check

# Linting
npm run lint

# Format code
npm run format

# Run tests
npm run test

# Watch mode tests
npm run test:watch

# Run integration tests
npm run test:integration

# Database migrations
npm run db:migrate

# Seed database
npm run db:seed

# Reset database
npm run db:reset
```

**Key Directories:**
- `src/controllers/` - Route handlers
- `src/services/` - Business logic
- `src/models/` - Database models
- `src/middleware/` - Express middleware
- `src/routes/` - API route definitions
- `src/utils/` - Utility functions
- `src/types/` - TypeScript interfaces

## Git Workflow

### Branch Naming Convention

```
feature/description       - New features
bugfix/description        - Bug fixes
docs/description          - Documentation
refactor/description      - Code refactoring
chore/description         - Build/CI tasks
```

### Commit Message Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

**Examples:**
```
feat(tracking): add real-time GPS updates

fix(auth): resolve JWT token expiration issue

docs(api): update endpoint documentation
```

### Creating a Feature Branch

```bash
# Update main branch
git pull origin main

# Create feature branch
git checkout -b feature/amazing-feature

# Make changes and commit
git commit -m "feat(feature): add amazing feature"

# Push to remote
git push origin feature/amazing-feature

# Create Pull Request on GitHub
```

## Code Style & Standards

### TypeScript Best Practices

- Use strict mode: `"strict": true` in tsconfig.json
- Explicit type annotations for function parameters
- Avoid `any` type
- Use `const` by default, `let` if needed
- Use interfaces for object shapes

### Frontend Standards

- Functional components with hooks
- Custom hooks for reusable logic
- Component co-location (tests with components)
- Proper error boundaries
- Accessibility (a11y) considerations

### Backend Standards

- Async/await over callbacks
- Service layer for business logic
- Controller layer for request handling
- Middleware for cross-cutting concerns
- Input validation in middleware
- Proper error handling with custom error classes

### Testing Requirements

- **Frontend**: Minimum 80% code coverage
- **Backend**: Minimum 75% code coverage
- Test file naming: `*.test.ts` or `*.spec.ts`
- Place tests near source files

## Common Development Tasks

### Adding a New API Endpoint

1. **Create Database Model** (`backend/src/models/`)
2. **Create Service** (`backend/src/services/`)
3. **Create Controller** (`backend/src/controllers/`)
4. **Add Routes** (`backend/src/routes/`)
5. **Add Tests** (`backend/tests/`)
6. **Create Frontend Service** (`frontend/src/services/`)
7. **Create Frontend Hook** (`frontend/src/hooks/`)
8. **Create Frontend Component** (`frontend/src/components/`)

### Adding a New Page

1. Create page component in `frontend/src/pages/`
2. Add route in page directory structure
3. Create custom hooks if needed
4. Add tests for the page
5. Update navigation/sidebar

### Working with Database Migrations

```bash
# Create new migration
npx prisma migrate dev --name add_new_table

# Apply migrations
npm run db:migrate

# Revert to previous state
npx prisma migrate resolve --rolled-back

# View migration status
npx prisma migrate status
```

### Running Tests

```bash
# All tests
npm test

# Watch mode
npm test:watch

# Specific test file
npm test -- auth.test.ts

# Coverage report
npm test:coverage

# Integration tests only
npm run test:integration
```

## Debugging

### Frontend Debugging

```bash
# Debug in Chrome DevTools
npm run dev

# Open Chrome DevTools (F12)
# Source tab → webpack → src/
```

### Backend Debugging

```bash
# Using Node Inspector
node --inspect-brk dist/index.js

# VS Code Debug Configuration
# .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${workspaceFolder}/backend/dist/index.js"
    }
  ]
}
```

### Database Debugging

```bash
# PostgreSQL
psql -U postgres -d supreme_delivery

# MongoDB
mongosh localhost:27017

# Redis
redis-cli
```

## Performance Optimization

### Frontend
- Code splitting with dynamic imports
- Image optimization with Next.js Image
- Bundle analysis: `npm run build --analyze`
- Lighthouse audits in Chrome DevTools

### Backend
- Database query optimization
- Connection pooling
- Caching strategies
- API response compression
- Load testing with k6 or Artillery

## Common Issues & Solutions

### Database Connection Issues

```bash
# Check PostgreSQL status
docker ps | grep postgres

# View PostgreSQL logs
docker logs supreme-delivery-postgres

# Reset database
npm run db:reset
```

### Port Already in Use

```bash
# Find process using port
lsof -i :3000  # for frontend
lsof -i :5000  # for backend

# Kill process
kill -9 <PID>
```

### Module Not Found

```bash
# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install

# Clear Next.js cache
rm -rf frontend/.next
```

## Useful Extensions (VS Code)

- ES7+ React/Redux/React-Native snippets
- TypeScript Vue Plugin
- Prettier - Code formatter
- ESLint
- REST Client
- MongoDB for VS Code
- PostgreSQL Explorer

## Additional Resources

- [Next.js Documentation](https://nextjs.org/docs)
- [React Documentation](https://react.dev)
- [Express.js Guide](https://expressjs.com)
- [Prisma ORM](https://www.prisma.io/docs)
- [TypeScript Handbook](https://www.typescriptlang.org/docs)
- [Docker Documentation](https://docs.docker.com)

## Getting Help

- Check existing GitHub issues
- Review architecture documentation
- Ask in GitHub Discussions
- Create detailed issue reports with reproduction steps

---

**Happy Coding! 🚀**