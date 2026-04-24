# Claude Code Configuration

This directory contains configuration files for Claude Code.

## MCP Servers

By default, the following MCP servers are **disabled** to reduce token usage:
- `playwright` - Browser automation
- `context7` - Additional context tools

### Re-enabling MCP Servers

If you need these MCP servers, you can re-enable them:

**Option 1: Delete the settings file**
```bash
rm .claude/settings.json
```

**Option 2: Edit settings.json**
Change `"disabled": true` to `"disabled": false`:
```json
{
  "mcpServers": {
    "playwright": {
      "disabled": false  // Changed from true
    },
    "context7": {
      "disabled": false  // Changed from true
    }
  }
}
```

**Option 3: Enable specific servers**
Keep some disabled and enable others:
```json
{
  "mcpServers": {
    "playwright": {
      "disabled": false  // Enabled
    },
    "context7": {
      "disabled": true   // Still disabled
    }
  }
}
```

### Other Available MCP Servers

You can add other MCP servers by installing them and configuring in `settings.json`. See [Claude Code MCP Documentation](https://docs.anthropic.com/en/docs/build-with-claude/mcp) for available servers.

---

## Files in This Directory

- `settings.json` - Project-level settings (MCP server configuration)
- `settings.local.json` - Local settings (permissions, not committed to git)
- `skills/` - Custom Claude Code skills for this project
- `agents/` - Agent definitions and workflows

---

**Note:** `settings.local.json` is gitignored and contains machine-specific settings. `settings.json` is committed and shared across all developers.
