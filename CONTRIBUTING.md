# Contributing to Johnson and Sebha NPC Organization

Thank you for your interest in contributing! We welcome contributions from everyone. This document provides guidelines and instructions for contributing to our project.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [How to Contribute](#how-to-contribute)
- [Commit Message Guidelines](#commit-message-guidelines)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Testing](#testing)
- [Documentation](#documentation)
- [Questions?](#questions)

## 🤝 Code of Conduct

### Our Pledge

In the interest of fostering an open and welcoming environment, we as contributors and maintainers pledge to making participation in our project and our community a harassment-free experience for everyone.

### Our Standards

Examples of behavior that contributes to creating a positive environment include:

- Using welcoming and inclusive language
- Being respectful of differing opinions, viewpoints, and experiences
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards other community members

Examples of unacceptable behavior include:

- The use of sexualized language or imagery and unwelcome sexual attention
- Trolling, insulting/derogatory comments, and personal or political attacks
- Public or private harassment
- Publishing others' private information without explicit permission
- Other conduct which could reasonably be considered inappropriate

## 🚀 Getting Started

### Prerequisites

- Node.js 16.x or higher
- npm or yarn
- Git
- GitHub account
- Text editor (VS Code recommended)

### Fork & Clone

1. Fork the repository by clicking the "Fork" button
2. Clone your fork:
```bash
git clone https://github.com/YOUR-USERNAME/johnsonandsebhanpc-org.git
cd johnsonandsebhanpc-org
```

3. Add upstream remote:
```bash
git remote add upstream https://github.com/sikh3nt/johnsonandsebhanpc-org.git
```

## 💻 Development Setup

1. **Install dependencies**
```bash
npm install
```

2. **Create environment file**
```bash
cp .env.example .env.local
```

3. **Start development server**
```bash
npm run dev
```

4. **Run tests**
```bash
npm test
```

## 🎯 How to Contribute

### Reporting Bugs

Before submitting a bug report, please check the [issue list](https://github.com/sikh3nt/johnsonandsebhanpc-org/issues) to see if it has already been reported.

When creating a bug report, include:

- **Clear title and description**
- **Steps to reproduce** the issue
- **Expected behavior**
- **Actual behavior**
- **Screenshots/videos** (if applicable)
- **Environment** (OS, browser, Node version, etc.)
- **Error messages and logs**

**Example:**
```
Title: NPC creation form fails with TypeError

Steps to Reproduce:
1. Navigate to /npcs/create
2. Fill in the form fields
3. Click "Create NPC" button

Expected: NPC should be created successfully
Actual: Error message appears in console

Error: TypeError: Cannot read property 'id' of undefined
```

### Suggesting Enhancements

Enhancement suggestions are tracked as GitHub issues with the `enhancement` label.

When suggesting an enhancement, provide:

- **Clear title and description** of the enhancement
- **Motivation** - Why do you believe this would be useful?
- **Possible implementation** - How do you envision it working?
- **Related issues** - Reference any related issues

### Submitting Pull Requests

1. **Create a feature branch**
```bash
git checkout -b feature/your-feature-name
```

2. **Make your changes**
   - Keep commits atomic and logical
   - Write clear commit messages
   - Test your changes thoroughly

3. **Keep your branch updated**
```bash
git fetch upstream
git rebase upstream/main
```

4. **Push to your fork**
```bash
git push origin feature/your-feature-name
```

5. **Create a Pull Request**
   - Use a clear title
   - Reference related issues
   - Provide detailed description
   - Include screenshots/videos if applicable

## 📝 Commit Message Guidelines

We follow the Conventional Commits specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation only changes
- **style**: Changes that don't affect code meaning (formatting, missing semicolons, etc.)
- **refactor**: Code change that neither fixes a bug nor adds a feature
- **perf**: Code change that improves performance
- **test**: Adding missing tests or correcting existing tests
- **chore**: Changes to build process, dependencies, or other non-code changes

### Scope

Optional but recommended. Specify the area of the codebase affected:
- auth
- npc
- dashboard
- api
- db
- etc.

### Subject

- Use imperative mood ("add" not "added" or "adds")
- Don't capitalize first letter
- No period (.) at the end
- Limit to 50 characters

### Body

- Explain the **what** and **why**, not the how
- Wrap at 72 characters
- Separate from subject with a blank line

### Footer

Optional. Reference issues and breaking changes:
```
Closes #123
Breaking-Change: description of breaking change
```

### Examples

```
feat(npc): add npc creation modal

Add a new modal component for creating NPCs with form validation
and error handling.

Closes #42
```

```
fix(auth): prevent token refresh on logout

Remove automatic token refresh mechanism when user logs out.
This prevents unnecessary API calls and improves logout experience.
```

## 🔄 Pull Request Process

1. **Ensure tests pass**
```bash
npm test
```

2. **Lint your code**
```bash
npm run lint
npm run lint:fix  # auto-fix issues
```

3. **Format code**
```bash
npm run format
```

4. **Update documentation** if needed

5. **Fill out the PR template** completely

6. **Wait for review** - Maintainers will review your PR

7. **Address feedback** - Make requested changes and push updates

8. **Merge** - Once approved, your PR will be merged!

## 📐 Coding Standards

### TypeScript

- Use strict mode: `"strict": true` in `tsconfig.json`
- Avoid `any` types - use proper typing
- Use interfaces for object types
- Use enums for constant values
- Export types that are part of the public API

### React

- Use functional components with hooks
- Write meaningful component names
- Keep components focused and single-responsibility
- Use custom hooks for shared logic
- Proper prop typing with TypeScript
- Memoize components when needed (React.memo)

### Code Style

- Use 2-space indentation
- Use single quotes for strings
- Use semicolons
- Use const by default, let when needed, avoid var
- Use arrow functions for callbacks
- Use object shorthand

### Example

```typescript
// ✅ Good
const handleClick = (id: string): void => {
  fetchUser(id);
};

// ❌ Avoid
var handleClick = function(id) {
  fetchUser(id);
}
```

## 🧪 Testing

All code changes should include tests:

```bash
# Run tests
npm test

# Run tests in watch mode
npm run test:watch

# Generate coverage report
npm run test:coverage
```

### Testing Guidelines

- Test user interactions, not implementation details
- Use descriptive test names
- Group related tests with `describe`
- Test both happy paths and error cases
- Aim for >80% code coverage

### Example

```typescript
describe('LoginForm', () => {
  it('should submit form with valid credentials', async () => {
    render(<LoginForm />);
    const emailInput = screen.getByLabelText(/email/i);
    const passwordInput = screen.getByLabelText(/password/i);
    const submitButton = screen.getByRole('button', { name: /login/i });

    await userEvent.type(emailInput, 'user@example.com');
    await userEvent.type(passwordInput, 'password123');
    await userEvent.click(submitButton);

    expect(mockLogin).toHaveBeenCalledWith({
      email: 'user@example.com',
      password: 'password123',
    });
  });

  it('should display error message on failed login', async () => {
    mockLogin.mockRejectedValueOnce(new Error('Invalid credentials'));
    
    render(<LoginForm />);
    // ... fill form and submit
    
    expect(await screen.findByText(/invalid credentials/i)).toBeInTheDocument();
  });
});
```

## 📚 Documentation

When adding new features:

1. **Update README.md** if adding major features
2. **Add JSDoc comments** to functions and components
3. **Update relevant docs** in the `/docs` folder
4. **Add inline comments** for complex logic

### JSDoc Example

```typescript
/**
 * Fetches an NPC by ID
 * @param id - The NPC ID to fetch
 * @returns Promise containing the NPC data
 * @throws Error if NPC not found
 * @example
 * const npc = await fetchNPC('123');
 */
export async function fetchNPC(id: string): Promise<NPC> {
  // ...
}
```

## ❓ Questions?

- **Issues**: Open a GitHub issue
- **Discussions**: Use GitHub Discussions
- **Email**: Contact maintainers directly
- **Chat**: Join our Discord/Slack (if applicable)

## 🙏 Thank You!

We appreciate your contributions and efforts to make this project better. Every contribution, no matter how small, is valuable!

---

**Last Updated**: June 10, 2026

Happy coding! 🚀
