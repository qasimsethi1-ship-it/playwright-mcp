# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run build       # Compile all workspaces
npm run lint        # Lint all workspaces
npm run test        # Run all tests
```

To run tests for a single package:
```bash
cd packages/playwright-mcp && npm test -- <test-file-pattern>
```

## Architecture

This is a **monorepo** (3 packages under `packages/`) implementing a Model Context Protocol (MCP) server for browser automation via Playwright. It is maintained by Microsoft and designed for AI assistants that need browser control without vision models — interaction is accessibility-tree-based, not pixel-based.

### Packages

| Package | Purpose |
|---------|---------|
| `packages/playwright-mcp` | Core MCP server — exposes 30+ automation tools (click, navigate, fill, evaluate JS, upload, tab management, etc.) |
| `packages/playwright-cli-stub` | CLI wrapper that launches the MCP server |
| `packages/extension` | Browser extension component |

### Key Design Decisions

- **Accessibility tree over vision**: Tools operate on the DOM accessibility tree, so no vision/screenshot model is needed.
- **Persistent browser state**: Browser context persists across multiple MCP tool calls within a session.
- **Opt-in capabilities**: Network mocking, storage management (cookies/localStorage), video recording, tracing, PDF generation, and test assertions are all disabled by default and enabled via config.

### Configuration

All server options are defined in the config schema in `packages/playwright-mcp`. Capabilities like `network`, `storage`, `video`, `trace`, and `testing` are opt-in sections in the MCP server config.

---

## Commit Convention

Semantic commit messages: `label(scope): description`

Labels: `fix`, `feat`, `chore`, `docs`, `test`, `devops`

```bash
git checkout -b fix-39562
# ... make changes ...
git add <changed-files>
git commit -m "$(cat <<'EOF'
fix(proxy): handle SOCKS proxy authentication

Fixes: https://github.com/microsoft/playwright/issues/39562
EOF
)"
git push origin fix-39562
gh pr create --repo microsoft/playwright --head username:fix-39562 \
  --title "fix(proxy): handle SOCKS proxy authentication" \
  --body "$(cat <<'EOF'
## Summary
- <describe the change very! briefly>

Fixes https://github.com/microsoft/playwright/issues/39562
EOF
)"
```

Never add Co-Authored-By agents in commit message.
Branch naming for issue fixes: `fix-<issue-number>`

