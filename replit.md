# Telegram Shell Bot

## Overview
This is a fully functional Telegram bot that allows you to execute shell commands remotely via Telegram. It provides an interactive terminal-like experience with support for file uploads/downloads, a text editor, and real-time command output.

**Current Status:** ✅ Fully configured and running in Replit environment

## Project Architecture

### Technology Stack
- **Runtime:** Node.js 20.19.3
- **Main Framework:** Botgram (Telegram bot framework)
- **Key Dependencies:**
  - `botgram` - Telegram bot API wrapper
  - `node-pty` - Terminal emulation (updated to v1.0.0 for Node.js 20 compatibility)
  - `terminal.js` - Terminal rendering and escape sequence interpretation
  - `escape-html` - HTML sanitization for Telegram messages
  - `mime` - MIME type detection for file operations

### Project Structure
```
├── server.js           # Main entry point, bot initialization and command handlers
├── lib/
│   ├── command.js      # Command execution and terminal management
│   ├── editor.js       # Text file editor functionality
│   ├── renderer.js     # Terminal output rendering
│   ├── terminal.js     # Terminal emulation logic
│   ├── utils.js        # Utility functions (env, shell detection, etc.)
│   └── wizard.js       # Configuration wizard (not used in Replit)
├── package.json        # Dependencies and project metadata
└── config.json         # ❌ Not used (uses environment variables instead)
```

## Configuration

### Environment Variables (Replit Secrets)
The bot uses the following environment variables for configuration:
- `TELEGRAM_BOT_TOKEN` - Bot authentication token from BotFather
- `TELEGRAM_OWNER_ID` - Numeric Telegram user ID of the bot owner

### Configuration Method
Modified `server.js` to prioritize environment variables over config.json file:
- If both environment variables are present, they are used (Replit setup)
- Otherwise, falls back to config.json (original setup)
- This allows the bot to run securely in Replit without committing credentials

## Recent Changes (2025-10-28)

### Replit Environment Setup
1. **Environment Variable Support:** Modified `server.js` lines 18-40 to read configuration from environment variables (`TELEGRAM_BOT_TOKEN`, `TELEGRAM_OWNER_ID`) with validation
2. **Dependency Updates:** Updated `node-pty` from `^0.9.0` to `^1.0.0` for Node.js 20 compatibility
3. **System Dependencies:** Added Python 3 and GCC via Nix for native module compilation
4. **Workflow Configuration:** Set up "Telegram Bot" workflow to run `node server.js` with console output
5. **New Feature:** Added `/json` command to display JSON data of replied messages for debugging and inspection

### Technical Details
- **Build Requirements:** Python 3 and GCC are required to compile node-pty native modules
- **No Frontend:** This is a backend-only bot with no web interface
- **Port Usage:** No ports are used (Telegram bot uses polling/webhooks to Telegram servers)

## Bot Features

### Commands Available
- `/run <command>` - Execute a shell command
- `/cancel` - Send SIGINT (Ctrl+C) to running command
- `/kill` - Send SIGTERM to running process
- `/enter <text>` - Send input to running command
- `/upload <file>` - Download a file from the server
- `/file <file>` - View/edit a text file
- `/cd <dir>` - Change working directory
- `/status` - View current bot status and settings
- `/shell` - View/change the shell
- `/env` - View/set environment variables
- `/resize` - Change terminal size
- `/json` - Display JSON data of any message (reply to a message with this command)
- `/token` - Generate access token for other users
- `/grant` / `/revoke` - Manage user access

### Security Features
- Owner-only access by default
- Token-based access for additional users
- Per-chat context isolation
- Environment variable sanitization

## User Preferences
None specified yet.

## How to Use

### Starting the Bot
The bot starts automatically via the "Telegram Bot" workflow. You should see "Bot ready." in the console logs.

### Accessing the Bot
1. Open Telegram and search for your bot (using the username you set with BotFather)
2. Send `/start` to begin
3. Use `/help` to see all available commands
4. Use `/run <command>` to execute shell commands

### Granting Access to Others
If you want to allow another Telegram user to use your bot:
1. Send `/token` to your bot
2. Share the generated link with the user
3. They click the link or forward the message
4. Use `/revoke <id>` to remove access later

## Troubleshooting

### Bot Not Responding
- Check that the workflow "Telegram Bot" is running
- Verify logs show "Bot ready."
- Confirm your Telegram user ID matches `TELEGRAM_OWNER_ID`

### Command Execution Issues
- Commands run in the Replit environment with limited privileges
- Some commands requiring root access will fail
- File paths are relative to `/home/runner/workspace`

### Module Compilation Errors
If you encounter native module errors:
- Ensure Python 3 and GCC are installed (already configured)
- Delete `node_modules` and run `npm install` again

## Notes
- The bot runs in a Linux environment (NixOS on Replit)
- Working directory defaults to `/home/runner/workspace`
- The bot maintains separate contexts for each chat/user
- Terminal size defaults to 40x20 characters (adjustable with `/resize`)
