<h1 align="center">
  <br>
  <a href="https://github.com/thedotmack/claude-mem">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="docs/claude-mem-logo-for-dark-mode.webp">
      <source media="(prefers-color-scheme: light)" srcset="docs/claude-mem-logo-for-light-mode.webp">
      <img src="docs/claude-mem-logo-for-light-mode.webp" alt="Claude-Mem" width="400">
    </picture>
  </a>
  <br>
</h1>

<h4 align="center">Persistent memory compression system built for <a href="https://claude.com/claude-code" target="_blank">Claude Code</a>.</h4>

<p align="center">
  <a href="LICENSE">
    <img src="https://img.shields.io/badge/License-AGPL%203.0-blue.svg" alt="License">
  </a>
  <a href="package.json">
    <img src="https://img.shields.io/badge/version-4.3.0-green.svg" alt="Version">
  </a>
  <a href="package.json">
    <img src="https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen.svg" alt="Node">
  </a>
  <a href="https://github.com/hesreallyhim/awesome-claude-code">
    <img src="https://awesome.re/mentioned-badge.svg" alt="Mentioned in Awesome Claude Code">
  </a>
</p>

<p align="center">
  <a href="#quick-start">Quick Start</a> •
  <a href="#how-it-works">How It Works</a> •
  <a href="#mcp-search-tools">Search Tools</a> •
  <a href="#documentation">Documentation</a> •
  <a href="#configuration">Configuration</a> •
  <a href="#troubleshooting">Troubleshooting</a> •
  <a href="#license">License</a>
</p>

<p align="center">
  Claude-Mem seamlessly preserves context across sessions by automatically capturing tool usage observations, generating semantic summaries, and making them available to future sessions. This enables Claude to maintain continuity of knowledge about projects even after sessions end or reconnect.
</p>

---

## Quick Start

Start a new Claude Code session in the terminal and enter the following commands:

```
> /plugin marketplace add thedotmack/claude-mem

> /plugin install claude-mem
```

Restart Claude Code. Context from previous sessions will automatically appear in new sessions.

**Key Features:**
- 🧠 **Persistent Memory** - Context survives across sessions
- 📊 **Progressive Disclosure** - Layered memory retrieval with token cost visibility
- 🔍 **7 Search Tools** - Query your project history via MCP
- 🤖 **Automatic Operation** - No manual intervention required
- 🔗 **Citations** - Reference past decisions with `claude-mem://` URIs

---

## Documentation

📚 **[View Full Documentation](docs/)** - Browse markdown docs on GitHub

💻 **Local Preview**: Run Mintlify docs locally:
```bash
cd docs
npx mintlify dev
```

### Getting Started
- **[Installation Guide](docs/installation.mdx)** - Quick start & advanced installation
- **[Usage Guide](docs/usage/getting-started.mdx)** - How Claude-Mem works automatically
- **[MCP Search Tools](docs/usage/search-tools.mdx)** - Query your project history

### Architecture
- **[Overview](docs/architecture/overview.mdx)** - System components & data flow
- **[Hooks](docs/architecture/hooks.mdx)** - 5 lifecycle hooks explained
- **[Worker Service](docs/architecture/worker-service.mdx)** - HTTP API & PM2 management
- **[Database](docs/architecture/database.mdx)** - SQLite schema & FTS5 search
- **[MCP Search](docs/architecture/mcp-search.mdx)** - 7 search tools & examples

### Configuration & Development
- **[Configuration](docs/configuration.mdx)** - Environment variables & settings
- **[Development](docs/development.mdx)** - Building, testing, contributing
- **[Troubleshooting](docs/troubleshooting.mdx)** - Common issues & solutions

---

## How It Works

```
┌─────────────────────────────────────────────────────────────┐
│ Session Start → Inject context from last 10 sessions       │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ User Prompts → Create session, save user prompts           │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ Tool Executions → Capture observations (Read, Write, etc.)  │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ Worker Processes → Extract learnings via Claude Agent SDK   │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ Session Ends → Generate summary, ready for next session     │
└─────────────────────────────────────────────────────────────┘
```

**Core Components:**
1. **5 Lifecycle Hooks** - SessionStart, UserPromptSubmit, PostToolUse, Stop, SessionEnd
2. **Worker Service** - HTTP API on port 37777 managed by PM2
3. **SQLite Database** - Stores sessions, observations, summaries with FTS5 search
4. **7 MCP Search Tools** - Query historical context with citations

See [Architecture Overview](docs/architecture/overview.mdx) for details.

---

## MCP Search Tools

Claude-Mem provides 7 specialized search tools:

1. **search_observations** - Full-text search across observations
2. **search_sessions** - Full-text search across session summaries
3. **search_user_prompts** - Search raw user requests
4. **find_by_concept** - Find by concept tags
5. **find_by_file** - Find by file references
6. **find_by_type** - Find by type (decision, bugfix, feature, etc.)
7. **get_recent_context** - Get recent session context

**Example Queries:**
```
search_observations with query="authentication" and type="decision"
find_by_file with filePath="worker-service.ts"
search_user_prompts with query="add dark mode"
get_recent_context with limit=5
```

See [MCP Search Tools Guide](docs/usage/search-tools.mdx) for detailed examples.

---

## What's New in v4.3.0

**Progressive Disclosure Context:**
- Enhanced context hook displays observation timeline with token cost visibility
- Table format shows ID, timestamp, type indicators (🔴 critical, 🟤 decision, 🔵 informational), title, and token counts
- Progressive disclosure instructions guide Claude on when to fetch full observation details vs. reading code
- Layered memory retrieval: Index → Details → Perfect Recall (code/transcripts)

**Improvements:**
- Added Agent Skills documentation and version bump management skill
- Removed hardcoded paths for project and Claude Code executable (fixes #23)
- Enhanced session summary handling and timeline rendering

See [CHANGELOG.md](CHANGELOG.md) for complete version history.

---

## System Requirements

- **Node.js**: 18.0.0 or higher
- **Claude Code**: Latest version with plugin support
- **PM2**: Process manager (bundled - no global install required)
- **SQLite 3**: For persistent storage (bundled)

---

## Key Benefits

### Progressive Disclosure Context
- **Layered memory retrieval** mirrors human memory patterns
- **Layer 1 (Index)**: See what observations exist with token costs at session start
- **Layer 2 (Details)**: Fetch full narratives on-demand via MCP search
- **Layer 3 (Perfect Recall)**: Access source code and original transcripts
- **Smart decision-making**: Token counts help Claude choose between fetching details or reading code
- **Type indicators**: Visual cues (🔴 critical, 🟤 decision, 🔵 informational) highlight observation importance

### Automatic Memory
- Context automatically injected when Claude starts
- No manual commands or configuration needed
- Works transparently in the background

### Full History Search
- Search across all sessions and observations
- FTS5 full-text search for fast queries
- Citations link back to specific observations

### Structured Observations
- AI-powered extraction of learnings
- Categorized by type (decision, bugfix, feature, etc.)
- Tagged with concepts and file references

### Multi-Prompt Sessions
- Sessions span multiple user prompts
- Context preserved across `/clear` commands
- Track entire conversation threads

---

## Configuration

Claude-mem uses a centralized settings system. Configure using:

1. **Agent Skill** (recommended) - Ask Claude "show me my claude-mem settings"
2. **Slash Commands** - `/claude-mem:context-on`, `/claude-mem:memory-off`, etc.
3. **CLI Tool** - `node plugin/scripts/settings-cli.js`
4. **Direct Edit** - Edit `~/.claude-mem/settings.json`

**Available Settings:**
- `model` - AI model (default: claude-sonnet-4-5)
- `workerPort` - Worker port (default: 37777)
- `enableMemoryStorage` - Store observations (default: true)
- `enableContextInjection` - Inject context at session start (default: true)
- `contextDepth` - Number of sessions to load (default: 5)

See [Configuration Guide](docs/configuration.mdx) for details.

---

## Development

```bash
# Clone and build
git clone https://github.com/thedotmack/claude-mem.git
cd claude-mem
npm install
npm run build

# Run tests
npm test

# Start worker
npm run worker:start

# View logs
npm run worker:logs
```

See [Development Guide](docs/development.mdx) for detailed instructions.

---

## Troubleshooting

**Common Issues:**
- Worker not starting → `npm run worker:restart`
- No context appearing → `npm run test:context`
- Database issues → `sqlite3 ~/.claude-mem/claude-mem.db "PRAGMA integrity_check;"`
- Search not working → Check FTS5 tables exist

See [Troubleshooting Guide](docs/troubleshooting.mdx) for complete solutions.

---

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes with tests
4. Update documentation
5. Submit a Pull Request

See [Development Guide](docs/development.mdx) for contribution workflow.

---

## License

This project is licensed under the **GNU Affero General Public License v3.0** (AGPL-3.0).

Copyright (C) 2025 Alex Newman (@thedotmack). All rights reserved.

See the [LICENSE](LICENSE) file for full details.

**What This Means:**
- You can use, modify, and distribute this software freely
- If you modify and deploy on a network server, you must make your source code available
- Derivative works must also be licensed under AGPL-3.0
- There is NO WARRANTY for this software

---

## Support

- **Documentation**: [docs/](docs/)
- **Issues**: [GitHub Issues](https://github.com/thedotmack/claude-mem/issues)
- **Repository**: [github.com/thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)
- **Author**: Alex Newman ([@thedotmack](https://github.com/thedotmack))

---

**Built with Claude Agent SDK** | **Powered by Claude Code** | **Made with TypeScript**
