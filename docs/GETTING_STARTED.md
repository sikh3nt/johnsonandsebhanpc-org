# Getting Started Guide

Welcome to the Johnson and Sebha NPC Organization project! This guide will help you get the project up and running on your local machine.

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** v16.0.0 or higher ([Download](https://nodejs.org/))
- **npm** v8.0.0 or higher (comes with Node.js)
- **Git** ([Download](https://git-scm.com/))
- **A code editor** (VS Code recommended)

### Verify Installation

```bash
node --version  # Should be v16 or higher
npm --version   # Should be v8 or higher
git --version   # Should be installed
```

## 🚀 Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/sikh3nt/johnsonandsebhanpc-org.git
cd johnsonandsebhanpc-org
```

### 2. Install Dependencies

```bash
npm install
```

This will install all required packages listed in `package.json`.

**Troubleshooting**: If you encounter issues, try:
```bash
npm clean-install  # Clear cache and reinstall
```

### 3. Setup Environment Variables

Copy the example environment file and configure it:

```bash
cp .env.example .env.local
```

Edit `.env.local` and update the values:

```env
# Required for development
NODE_ENV=development
REACT_APP_API_URL=http://localhost:5000

# Optional - Add as needed
DATABASE_URL=postgresql://user:password@localhost:5432/db_name
JWT_SECRET=your_secret_key_here
```

### 4. Start the Development Server

```bash
npm run dev
```

The application should now be running at `http://localhost:3000`

## 📂 Project Structure Overview

```
src/
├── components/      # React components
├── pages/          # Page components
├── hooks/          # Custom React hooks
├── services/       # API calls
├── types/          # TypeScript definitions
├── utils/          # Helper functions
└── styles/         # Global styles

docs/              # Documentation files
public/            # Static assets
tests/             # Test files
```

## 🔧 Available Commands

### Development
```bash
# Start dev server with hot reload
npm run dev

# Type checking
npm run type-check

# Linting
npm run lint
npm run lint:fix
```

### Building
```bash
# Build for production
npm run build

# Preview production build
npm run preview
```

### Testing
```bash
# Run tests
npm test

# Run tests in watch mode
npm run test:watch

# Generate coverage report
npm run test:coverage
```

### Code Quality
```bash
# Format code
npm run format

# Fix linting issues
npm run lint:fix
```

## 🌐 Accessing the Application

Once the development server is running:

1. **Open browser**: Navigate to `http://localhost:3000`
2. **Hot reload**: Changes to code will automatically refresh the browser
3. **Console**: Check browser console (F12) for any errors

## 📱 Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## 🔌 API Integration

The application connects to an API server. By default:

- **API Base URL**: `http://localhost:5000`
- **Timeout**: 30 seconds

To change the API URL, modify `.env.local`:
```env
REACT_APP_API_URL=https://your-api-url.com
```

## 🚨 Troubleshooting

### Port Already in Use

If port 3000 is already in use:

```bash
# Windows
netstat -ano | findstr :3000
taskkill /PID <PID> /F

# macOS/Linux
lsof -i :3000
kill -9 <PID>
```

Or specify a different port:
```bash
PORT=3001 npm run dev
```

### Node Modules Issues

```bash
# Clear npm cache
npm cache clean --force

# Remove node_modules
rm -rf node_modules

# Reinstall
npm install
```

### Module Not Found Errors

Ensure all imports use correct paths:
```typescript
// ✅ Correct
import { Button } from '@/components/Button';

// ❌ Incorrect
import { Button } from './components/Button';
```

## 📚 Next Steps

1. **Read the [Architecture Overview](ARCHITECTURE.md)** to understand the project structure
2. **Check [API Documentation](API.md)** for available endpoints
3. **Review [Contributing Guidelines](CONTRIBUTING.md)** before making changes
4. **Start with [Issues](https://github.com/sikh3nt/johnsonandsebhanpc-org/issues)** labeled `good first issue`

## 🤝 Getting Help

- **Documentation**: Check the `/docs` folder
- **Issues**: Open a [GitHub Issue](https://github.com/sikh3nt/johnsonandsebhanpc-org/issues)
- **Discussions**: Use [GitHub Discussions](https://github.com/sikh3nt/johnsonandsebhanpc-org/discussions)

## ✨ Tips for Success

1. **Use VS Code extensions**:
   - ESLint
   - Prettier - Code formatter
   - Tailwind CSS IntelliSense

2. **Enable auto-save** in your editor

3. **Use the browser DevTools** for debugging

4. **Check the console** for helpful error messages

5. **Keep dependencies updated** periodically

## 📖 Useful Resources

- [React Documentation](https://react.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Vite Documentation](https://vitejs.dev)
- [Tailwind CSS](https://tailwindcss.com)

---

**Stuck?** Don't hesitate to ask for help in the issues or discussions!

Happy coding! 🎉
