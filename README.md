# My Skills

A collection of Claude Code skills for enhanced development workflows.

## Skills

### code-review-router

Intelligently routes code reviews between Gemini CLI and Codex CLI based on your changes.

**Usage:**
```
/code-review-router
```

**Prerequisites:**
- Git repository with uncommitted changes
- At least one CLI installed: `gemini` or `codex`

**Routing Logic:**

| Change Type | Routes To | Why |
|-------------|-----------|-----|
| Security-sensitive code | Codex | Deeper security analysis |
| Large changesets (20+ files, 500+ lines) | Codex | Thorough review needed |
| Database/API/service changes | Codex | Architectural risk |
| Pure frontend (React, Vue, CSS) | Gemini | Faster feedback |
| Python/Django | Gemini | Strong Python support |
| Simple refactors | Gemini | Speed over depth |

**Features:**
- Automatic complexity scoring
- Language and framework detection
- Security pattern recognition
- Fallback to alternative CLI on failure
- 120-second timeout protection

## Installation

Place skill files in your Claude Code skills directory or reference this folder in your Claude Code configuration.

## License

MIT
