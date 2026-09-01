# Slides Agent Plugin

This package connects agent hosts to Slides.com through the remote Slides MCP server and teaches them how to read, view, create, edit, organize, and visually verify presentations.

The MCP connection and `slides-authoring` skill are shared. Platform-specific packaging is limited to `.codex-plugin/` and `.claude-plugin/`.

## Components

- `.mcp.json` connects to `https://mcp.slides.com/`.
- `skills/slides-authoring/SKILL.md` contains the shared presentation-authoring workflow.
- `.codex-plugin/plugin.json` packages the integration for Codex.
- `.claude-plugin/plugin.json` packages the integration for Claude.

The remote MCP server is implemented and deployed from the Slides web application. This package contains configuration and authoring guidance only.

The public source for this package is [slides/agent-plugin](https://github.com/slides/agent-plugin). It is maintained in the Slides web monorepo and published from `plugins/slides`.
