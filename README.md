# Claude Code Configuration

This repository contains my personal [Claude Code](https://claude.ai/download) configuration, including settings, custom agents, and hooks for enhanced functionality.

## 📦 What's Included

### Settings (`settings.json`)
- Model preferences (Sonnet)
- Hook configurations for session lifecycle events
- Pre-tool use permission handlers

### Agents (`agents/`)
- **todoist-agent**: Specialized agent for Todoist task management with ADHD-optimized workflow
  - Uses "Now/Next/This Week/Later" prioritization system
  - Integrates with MCP Todoist server tools
  - Automatic task prioritization and progress tracking

### Hooks (`hooks/`)
Advanced automation system for Claude Code sessions:

#### Core Hooks (`hooks/core/`)
- **session-start.js**: Initialization, memory retrieval, context setup
- **session-end.js**: Cleanup, memory storage, session summary
- **mid-conversation.js**: Dynamic context updates during conversation
- **permission-request.js**: MCP tool permission handling
- **memory-retrieval.js**: Smart memory search and retrieval
- **topic-change.js**: Context shift detection

#### Utilities (`hooks/utilities/`)
Supporting modules for hook functionality:
- Adaptive pattern detection
- Auto-capture patterns for memory storage
- Context formatting and shift detection
- Conversation analysis
- Git repository analysis
- MCP client integration
- Memory client with scoring algorithms
- Performance management
- Project detection
- Session tracking
- Tiered conversation monitoring
- User override detection
- Version checking

## 🚀 Installation

### Prerequisites
- [Claude Code CLI](https://claude.ai/download) installed
- Node.js (for hook scripts)
- MCP servers configured (optional, for full functionality)

### Setup

1. **Clone this repository:**
   ```bash
   git clone https://github.com/vippelina/claude-config.git
   ```

2. **Backup your existing configuration:**
   ```bash
   mv ~/.claude ~/.claude.backup
   ```

3. **Copy configuration files:**
   ```bash
   mkdir -p ~/.claude
   cp -r claude-config/settings.json ~/.claude/
   cp -r claude-config/agents ~/.claude/
   cp -r claude-config/hooks ~/.claude/
   ```

4. **Update file paths in settings.json:**
   Edit `~/.claude/settings.json` and replace `/Users/vibeke.tengroth/.claude/` with your actual home directory path.

5. **Make hooks executable:**
   ```bash
   chmod +x ~/.claude/hooks/core/*.js
   chmod +x ~/.claude/hooks/statusline.sh
   ```

## 🔧 Configuration

### Hook Configuration
Hooks are configured in `settings.json` under the `hooks` key:
- **SessionStart**: Runs when a new Claude session begins
- **SessionEnd**: Runs when a session ends
- **UserPromptSubmit**: Runs on each user message
- **PreToolUse**: Runs before MCP tools are executed

### MCP Integration
The hooks integrate with MCP servers for:
- Memory storage and retrieval
- Todoist task management
- Project context detection

## 📝 What's NOT Included

The following directories and files are excluded via `.gitignore`:
- `cache/` - Temporary cache files
- `debug/` - Debug logs
- `history.jsonl` - Conversation history
- `ide/` - IDE-specific settings
- `plugins/` - Plugin data
- `projects/` - Project-specific data
- `session-env/` - Session environment variables
- `shell-snapshots/` - Terminal state snapshots
- `stats-cache.json` - Statistics cache
- `statsig/` - Analytics data
- `todos/` - Local todo storage

## 🎯 Key Features

### Intelligent Memory System
- Automatic context retrieval based on conversation topics
- Project-aware memory scoring
- Adaptive pattern detection for auto-capture
- Git repository analysis for context

### ADHD-Optimized Workflow
- Single-focus task management
- Prioritization to reduce decision paralysis
- Progress tracking and celebration
- Weekly summaries

### Smart Permission Handling
- Pre-approval system for MCP tools
- Automatic permission requests with context

## 🤝 Contributing

Feel free to fork this repository and adapt it to your own needs. If you have improvements or suggestions, issues and pull requests are welcome!

## 📄 License

This configuration is provided as-is for personal use. Adapt freely to your own workflow.

## 🔗 Resources

- [Claude Code Documentation](https://claude.ai/claude-code)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
- [Todoist API](https://developer.todoist.com/)
