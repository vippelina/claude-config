---
name: start-memory-service
description: Start the MCP Memory Service HTTP dashboard server
homepage: https://github.com/doobidoo/mcp-memory-service
metadata: {"moltbot":{"emoji":"🧠","requires":{"bins":["python3"]}}}
---

# Start Memory Service Skill

Automatically finds and starts the MCP Memory Service HTTP dashboard server.

## What it does

1. Locates the mcp-memory-service directory on your computer
2. Activates the Python virtual environment
3. Starts the HTTP server with dashboard at http://localhost:8000

## Usage

Simply invoke this skill:
```bash
/start-memory-service
```

Or use it from Claude:
- "Start the memory service"
- "Run the memory dashboard"
- "Launch mcp memory server"

## How it works

The skill searches common locations for the mcp-memory-service directory:
- ~/Documents/mybrain/mcp-memory-service
- ~/mcp-memory-service
- ~/Documents/mcp-memory-service
- ~/projects/mcp-memory-service

Then runs:
```bash
cd <found-directory>
source .venv/bin/activate
python3 scripts/server/run_http_server.py
```

## What you'll get

Once started:
- ✅ Dashboard at: http://localhost:8000
- ✅ REST API at: http://localhost:8000/api/
- ✅ Health check: http://localhost:8000/api/health
- ✅ Real-time updates via Server-Sent Events
- ✅ Automatic backup scheduler
- ✅ Memory consolidation scheduler

## To stop the server

```bash
pkill -f "run_http_server.py"
```

## Requirements

- Python 3.10+
- Virtual environment set up in `.venv` directory
- Dependencies installed (`pip install -e .`)

## Implementation

```bash
#!/bin/bash

# Function to find mcp-memory-service directory
find_memory_service() {
    local search_paths=(
        "$HOME/Documents/mybrain/mcp-memory-service"
        "$HOME/mcp-memory-service"
        "$HOME/Documents/mcp-memory-service"
        "$HOME/projects/mcp-memory-service"
        "$HOME/dev/mcp-memory-service"
        "$HOME/code/mcp-memory-service"
    )

    for path in "${search_paths[@]}"; do
        if [ -d "$path" ]; then
            echo "$path"
            return 0
        fi
    done

    # If not found in common locations, search more broadly
    echo "Searching for mcp-memory-service directory..." >&2
    local found=$(find "$HOME" -maxdepth 4 -type d -name "mcp-memory-service" 2>/dev/null | head -1)
    if [ -n "$found" ]; then
        echo "$found"
        return 0
    fi

    return 1
}

# Main execution
MCP_DIR=$(find_memory_service)

if [ -z "$MCP_DIR" ]; then
    echo "❌ Error: Could not find mcp-memory-service directory"
    echo "Please ensure the repository is cloned to a standard location"
    exit 1
fi

echo "📂 Found mcp-memory-service at: $MCP_DIR"

# Check if venv exists
if [ ! -d "$MCP_DIR/.venv" ]; then
    echo "❌ Error: Virtual environment not found at $MCP_DIR/.venv"
    echo "Please run: cd $MCP_DIR && python3 -m venv .venv && source .venv/bin/activate && pip install -e ."
    exit 1
fi

echo "🔧 Activating virtual environment..."
cd "$MCP_DIR"

# Start the server in the background
echo "🚀 Starting MCP Memory Service HTTP server..."
source .venv/bin/activate && python3 scripts/server/run_http_server.py &

# Wait a moment for server to start
sleep 3

# Check if server is running
if curl -s http://localhost:8000/api/health > /dev/null 2>&1; then
    echo "✅ Server started successfully!"
    echo "📊 Dashboard: http://localhost:8000"
    echo "🔧 API: http://localhost:8000/api/"
    echo ""
    echo "To stop the server:"
    echo "  pkill -f 'run_http_server.py'"
else
    echo "⚠️  Server may still be starting... Check http://localhost:8000 in a moment"
fi
```

## Notes

- The server runs in the background
- Check the server logs if it fails to start
- Ensure port 8000 is available
- The skill automatically finds the directory, so it works from anywhere
