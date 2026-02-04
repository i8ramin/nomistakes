# nomistakes

> Error prevention and best practices enforcement for AI agent-assisted coding

An [agentskills.io](https://agentskills.io) skill that teaches AI agents to write bulletproof code by preventing common mistakes before they happen.

## What It Does

**nomistakes** is like a coding coach that lives inside AI agents (Claude Code, OpenCode, Cursor, etc.), helping them:

- ✅ Validate inputs at boundaries
- ✅ Handle errors explicitly (no silent failures)
- ✅ Prevent null pointer crashes
- ✅ Manage async operations safely
- ✅ Use TypeScript strictly
- ✅ Review AI-generated code for common bugs
- ✅ Migrate to modern tooling (Biome)

## Installation

### For Claude Code

```bash
# Copy to your skills directory
cp -r nomistakes ~/.claude/skills/
```

### For OpenCode

```bash
# Using the skills CLI
npx skills add <your-username>/nomistakes
```

### For Other Compatible Agents

Any agent supporting the agentskills.io standard can use this skill. Check your agent's documentation for skill installation instructions.

## Features

### 10 Core Error Prevention Principles

1. **Input Validation** - Guard all function boundaries
2. **Error Handling** - Fail fast, fail loudly
3. **Null Safety** - Optional chaining, explicit handling
4. **Async Operations** - Timeouts, cancellation, proper error propagation
5. **Type Safety** - Runtime checks + TypeScript
6. **Boundary Checks** - Array bounds, division by zero
7. **Resource Management** - Cleanup, file handles, connections
8. **Immutability** - Prevent accidental mutations
9. **Configuration Validation** - Validate env vars
10. **Defensive Programming** - Assertions, fail-safes

### Latest Tools & Best Practices (2026)

- **Biome Migration Guide** - Automated migration from ESLint + Prettier (50-100x faster)
- **AI Code Review Checklist** - Catch bugs in AI-generated code (Copilot, Cursor, Claude)
- **TypeScript 5.x Features** - Latest type safety patterns
- **Property-Based Testing** - Auto-generate edge cases
- **Effect-TS Patterns** - Type-safe error handling

### Comprehensive Documentation

- **Quick Reference** - SKILL.md with checklists and examples
- **API Reference** - Pattern lookup by concern
- **Real-World Examples** - Production-ready implementations
- **Deep Dives** - Error handling, TypeScript safety, testing strategies

## Documentation Structure

```
nomistakes/
├── SKILL.md                           # Core principles (agents load this)
├── docs/
│   ├── README.md                      # Documentation index
│   ├── api/reference.md               # Quick pattern lookup
│   ├── examples/real-world-patterns.md # Complete implementations
│   ├── ai-review-checklist.md         # AI code review guide
│   ├── adversarial-review.md          # VDD hostile review pattern
│   └── BIOME_MIGRATION.md             # ESLint → Biome guide
├── references/
│   ├── error-handling-patterns.md     # Error types, Result pattern, retry
│   ├── typescript-safety.md           # Branded types, exhaustive checks
│   ├── testing-strategies.md          # Property-based, mutation testing
│   └── ai-code-review.md              # Common AI bugs + detection
└── scripts/
    ├── validate-skill.js              # Validate agentskills.io spec
    └── migrate-to-biome.js            # Automated Biome migration
```

## Usage Example

**Without nomistakes:**
```typescript
async function createUser(email, password) {
  const user = await db.insert({ email, password });
  return user;
}
```

**With nomistakes:**
```typescript
async function createUser(
  email: string, 
  password: string
): Promise<Result<User, ValidationError>> {
  // Input validation
  if (!email || !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
    return { error: new ValidationError('Invalid email format') };
  }
  
  if (!password || password.length < 8) {
    return { error: new ValidationError('Password must be 8+ chars') };
  }
  
  // Error handling
  try {
    const user = await db.insert({ 
      email: email.toLowerCase().trim(), 
      password: await hash(password) 
    });
    return { data: user };
  } catch (e) {
    if (e.code === 'UNIQUE_VIOLATION') {
      return { error: new ValidationError('Email already exists') };
    }
    logger.error('User creation failed', { email, error: e });
    throw new ApplicationError('Failed to create user', { cause: e });
  }
}
```

## Who Benefits

- **AI Agents** - Write better code automatically
- **Developers** - Get fewer bugs in agent-generated code  
- **Teams** - Consistent code quality across AI-assisted development

## Specification

This skill follows the [agentskills.io specification](https://agentskills.io/specification) for maximum compatibility across AI coding agents.

## License

MIT - see [LICENSE](LICENSE) file for details.

## Author

Ramin Akhavan

## Contributing

Issues and pull requests welcome! See existing patterns in `references/` for contribution guidelines.

---

**Version**: 1.0.0  
**Compatible with**: Claude Code, OpenCode, Cursor, Goose, Amp, and other agentskills.io-compatible agents
