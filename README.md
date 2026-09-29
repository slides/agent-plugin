# Slides.com Agent Plugin

Connect your AI assistant to Slides.com to find, create, edit, share, and visually check presentations. The package combines the Slides.com MCP connection with a shared authoring skill.

## Connect

You need a Slides.com Lite, Pro, or active Team account. Sign in to Slides.com when your assistant prompts you. Choose **Full access** to create, edit, and share presentations, or **Read-only** to inspect them. Disconnect at [Connected apps](https://slides.com/users/oauth).

For ChatGPT, Claude, Cursor, and other MCP clients, follow the [connection instructions](https://slides.com/docs/connect-ai-tools). The remote server is `https://mcp.slides.com/`; it uses browser-based OAuth and does not require a client secret or API key in this package.

### Claude Code

Install from this repository as a marketplace:

```sh
claude plugin marketplace add slides/agent-plugin
claude plugin install slides@slides
```

Open `/mcp` and authenticate the Slides.com connection. If you previously added Slides.com manually, remove the duplicate connection before using the plugin.

### Local development

In the Slides.com web checkout, load `plugins/slides` as the plugin directory. In this exported repository, use the repository root. Claude Code supports `claude --plugin-dir <path>`; Cursor uses `.cursor-plugin/plugin.json`, and Codex uses `.codex-plugin/plugin.json`.

Marketplace discovery requires each platform's approval. A manifest in this repository does not mean the plugin has a public directory listing.

## Try it

- “Help me create a five-slide presentation from a brief and give me a link I can share.”
- “Draft an email to my team summarizing my latest presentation.”
- “Turn the latest earnings report from Spotify into a five-slide overview.”

The authoring skill fetches the current Slides.com authoring guide through `get_authoring_guide` before writing slide HTML. It uses slide screenshots to verify rendering and `view_deck` to show the result. Hosts without interactive previews receive a link to open the deck in Slides.com.

## Package

- `.mcp.json`: shared production MCP connection.
- `skills/slides-authoring/`: shared authoring and visual verification workflow.
- `assets/`: shared branding.
- `.codex-plugin/`, `.claude-plugin/`, `.cursor-plugin/`: host-specific manifests.
- `.claude-plugin/marketplace.json`: installation from the exported repository root.

The server and presentation operations live in the Slides.com web application. This package contains no server code or credentials. It is maintained in `plugins/slides` in the Slides.com web monorepo and published to [slides/agent-plugin](https://github.com/slides/agent-plugin).

## License

The plugin's skill, configuration, and documentation are licensed under the [MIT License](LICENSE), copyright 2026 Slides, Inc.

The license covers this plugin package only. Access to the Slides.com application and hosted MCP service remains subject to the [Slides.com Terms of Service](https://slides.com/terms) and the account requirements above.

[Help](https://slides.com/docs) · [Contact support](mailto:support@slides.com) · [Privacy](https://slides.com/privacy) · [Terms](https://slides.com/terms)
