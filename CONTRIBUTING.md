# Contributing to ToodooL

Thank you for considering contributing to ToodooL! This document provides guidelines and instructions for contributing to the project.

## Code of Conduct

- Be respectful and inclusive to all contributors
- Focus on constructive feedback
- Assume good intentions
- Report any inappropriate behavior to the maintainer

## Getting Started

### Step 1: Fork and Clone

1. Fork the repository on GitHub
2. Clone your fork:
   ```bash
   git clone https://github.com/YOUR_USERNAME/Toodool.git
   cd Toodool
   ```
3. Add upstream remote for syncing:
   ```bash
   git remote add upstream https://github.com/theankushgautam/Toodool.git
   ```

### Step 2: Create a Feature Branch

```bash
git checkout -b feature/your-feature-name
```

Use descriptive branch names:
- `feature/add-edit-todos` for new features
- `fix/theme-toggle-bug` for bug fixes
- `docs/update-readme` for documentation
- `refactor/storage-module` for refactoring

### Step 3: Development Setup

```bash
npm install
npm start
```

This starts the dev server at `http://localhost:8080` with hot reloading.

## Development Workflow

### Code Style

- Use **ES6+ features** (classes, arrow functions, const/let)
- Follow the existing code structure and patterns
- Use **camelCase** for variables and methods
- Use **PascalCase** for class names
- **No trailing semicolons** in single statements
- Use double quotes for HTML attributes, single quotes for JavaScript strings

### File Organization

```
src/
├── modules/          # Core application logic
│   ├── Todo.js       # Data model
│   ├── Storage.js    # Storage abstraction
│   └── UI.js         # View controller
├── styles/
│   └── main.css      # All styling
├── assets/           # Images, icons, screenshots
├── index.js          # Entry point
└── template.html     # HTML template
```

### Modifying Existing Code

1. **Understand the architecture** - Review [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)
2. **Keep changes focused** - One feature/fix per PR
3. **Maintain backward compatibility** - Don't break existing functionality
4. **Test your changes** - Manually verify in both dev and production builds

### Adding New Features

Consider:
- Does it fit the minimalistic philosophy?
- Will it add significant complexity?
- Can it be implemented efficiently?
- Should it be a separate feature or configuration?

Get feedback early by opening an issue before implementing major features.

## Commit Guidelines

### Commit Message Format

```
<type>: <description>

[optional body]

[optional references]
```

**Types:**
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation changes
- `refactor`: Code refactoring (no functionality change)
- `style`: Code style changes (formatting, missing semicolons, etc.)
- `test`: Adding or updating tests
- `chore`: Dependency updates, build changes, etc.

**Examples:**

```
feat: add todo editing functionality

Allow users to click on todo titles to edit them. Updates are persisted
to localStorage immediately.
```

```
fix: prevent event listener accumulation on save button

Previously, removeEventListener wouldn't work with arrow functions,
causing listeners to accumulate. Now uses stored handler reference.
```

```
docs: add browser support section to README
```

```
refactor: extract todo rendering logic to separate method
```

### Writing Good Commit Messages

- Use present tense: "Add feature" not "Added feature"
- Use imperative mood: "Move cursor to..." not "Moves cursor to..."
- Limit first line to 72 characters
- Reference issues: "Fixes #123" or "Related to #456"
- Explain *what* and *why*, not just *how*

### Before Committing

```bash
# Format and lint your code
npm run lint:fix

# Build to ensure no errors
npm run build

# Test in dev mode
npm start
```

## Testing Your Changes

### Manual Testing

1. **Start dev server**: `npm start`
2. **Create a todo** and verify it saves
3. **Toggle theme** and verify persistence
4. **Refresh page** and verify todos load
5. **Test in multiple browsers** (Chrome, Firefox, Safari, Edge)

### Testing a Production Build

```bash
npm run build
npm install -g http-server
http-server dist -p 8080
```

Visit `http://localhost:8080` and test all features.

### Checklist Before Submitting PR

- [ ] Code follows project style guidelines
- [ ] All changes are commented where complex
- [ ] No console errors or warnings
- [ ] Feature works in development mode
- [ ] Feature works in production build
- [ ] Existing features still work (no regressions)
- [ ] Tested in multiple browsers
- [ ] README updated if adding features
- [ ] Commit messages follow guidelines

## Submitting a Pull Request

### Before Submitting

1. **Sync with upstream**:
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Push to your fork**:
   ```bash
   git push origin feature/your-feature-name
   ```

### PR Template

Include in your PR description:

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Related Issues
Fixes #123
Related to #456

## Testing
- [ ] Tested in Chrome
- [ ] Tested in Firefox
- [ ] Tested in Safari
- [ ] No console errors
- [ ] Production build verified

## Screenshots (if applicable)
[Add screenshots for UI changes]

## Checklist
- [ ] Code follows style guidelines
- [ ] No new warnings generated
- [ ] Documentation updated
- [ ] All tests pass
```

### After Submitting

- Watch for feedback and review comments
- Be responsive to questions
- Make requested changes promptly
- Rebase and force-push if needed:
  ```bash
  git rebase -i upstream/main
  git push origin feature/your-feature-name --force
  ```

## Reporting Issues

### Bug Reports

Include:
- Clear title describing the bug
- Steps to reproduce
- Expected behavior
- Actual behavior
- Browser and OS
- Console errors
- Screenshots if applicable

**Example:**
```
**Title:** Theme toggle doesn't persist on mobile

**Steps to Reproduce:**
1. Open app on mobile browser
2. Click theme toggle button
3. Refresh page

**Expected:** Dark theme should persist
**Actual:** Page loads with light theme

**Browser:** Safari 16 on iOS 16.3
**Error:** None in console
```

### Feature Requests

Include:
- Clear description of the feature
- Why this feature would be useful
- How you envision it working
- Possible implementation approach
- Mock-ups or examples (if applicable)

**Example:**
```
**Title:** Add priority levels to todos

**Description:** Allow users to mark todos as high/medium/low priority

**Why:** Helps focus on what's important

**Mock-up:** [show where priority selector would appear]
```

## Review Process

- Maintainer will review your PR within a few days
- Feedback will be constructive and specific
- Changes may be requested before merging
- Once approved, your PR will be merged!
- You'll be credited in the changelog

## Questions?

- Check existing [issues](https://github.com/theankushgautam/Toodool/issues)
- Read [ARCHITECTURE.md](docs/ARCHITECTURE.md) for code organization
- Review existing code for patterns
- Open a discussion issue if you have questions

## Recognition

Contributors are recognized in:
- Pull request comments
- CHANGELOG.md
- GitHub contributors graph

Thank you for helping make ToodooL better! 🎉
