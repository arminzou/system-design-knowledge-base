# Conventional Commit Instructions

## Basic Format
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Type (Required)
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Formatting changes (no code change)
- `refactor`: Code restructuring (no feature/fix)
- `perf`: Performance improvements
- `test`: Test-related changes
- `build`: Build system changes
- `ci`: CI configuration changes
- `chore`: Maintenance tasks

## Scope (Optional)
- Add parentheses after type to indicate affected area: `feat(api):`

## Breaking Changes
- Add `!` after type/scope: `feat!:` or `feat(api)!:`
- Or add `BREAKING CHANGE:` in footer

## Writing Guidelines
- Use imperative present tense ("add" not "added")
- Keep description under 72 characters
- Don't end description with period
- Don't capitalize first letter

## Examples

```
feat: add user authentication
```

```
fix(parser): handle multi-space arrays
```

```
feat(api)!: remove deprecated endpoints

BREAKING CHANGE: The v1 API endpoints are no longer available.
```

```
docs: update README installation steps

Added platform-specific instructions for Windows, Linux, and macOS.

Fixes: #123
```

## Best Practices
- Make one change per commit
- Be specific about what changed
- Write as if explaining to future developers