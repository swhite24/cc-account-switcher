# Multi-Account Switcher for Claude Code

A simple tool to manage and switch between multiple Claude Code accounts (both OAuth and Console API key accounts) on macOS, Linux, and WSL.

## Features

- **Multi-account management**: Add, remove, and list Claude Code accounts (OAuth and Console API key)
- **Quick switching**: Switch between accounts with simple commands
- **Mixed account types**: Supports both OAuth accounts and Console API key accounts
- **Cross-platform**: Works on macOS, Linux, and WSL
- **Secure storage**: Uses system keychain (macOS) or protected files (Linux/WSL)
- **Settings preservation**: Only switches authentication - your themes, settings, and preferences remain unchanged

## Installation

Download the script directly:

```bash
curl -O https://raw.githubusercontent.com/ming86/cc-account-switcher/main/ccswitch.sh
chmod +x ccswitch.sh
```

## Usage

### Basic Commands

```bash
# Add current account to managed accounts
./ccswitch.sh --add-account

# List all managed accounts
./ccswitch.sh --list

# Switch to next account in sequence
./ccswitch.sh --switch

# Switch to specific account by number or email
./ccswitch.sh --switch-to 2
./ccswitch.sh --switch-to user2@example.com

# Remove an account
./ccswitch.sh --remove-account user2@example.com

# Show help
./ccswitch.sh --help
```

### First Time Setup

1. **Log into Claude Code** with your first account (OAuth or Console API key)
2. Run `./ccswitch.sh --add-account` to add it to managed accounts
3. **Log out** and log into Claude Code with your second account
4. Run `./ccswitch.sh --add-account` again
5. Now you can switch between accounts with `./ccswitch.sh --switch`
6. **Important**: After each switch, restart Claude Code to use the new authentication

> **Account Types:** The switcher automatically detects whether you're using an OAuth account or a Console API key account. You can mix both types - for example, have an OAuth personal account and a Console API key work account.

> **What gets switched:** Only your authentication credentials change. Your themes, settings, preferences, and chat history remain exactly the same.

## Requirements

- Bash 4.4+
- `jq` (JSON processor)

### Installing Dependencies

**macOS:**

```bash
brew install jq
```

**Ubuntu/Debian:**

```bash
sudo apt install jq
```

## How It Works

The switcher stores account authentication data separately:

- **macOS**: Credentials in Keychain (OAuth tokens or Console API keys), OAuth/Console config in `~/.claude-switch-backup/`
- **Linux/WSL**: Both credentials and OAuth/Console config in `~/.claude-switch-backup/` with restricted permissions

The tool automatically detects your account type:
- **OAuth accounts**: Identified by standard OAuth credentials
- **Console API key accounts**: Identified by the presence of `customApiKeyResponses` in the config

When switching accounts, it:

1. Backs up the current account's authentication data (OAuth or Console API key)
2. Restores the target account's authentication data
3. Updates Claude Code's authentication files with the correct credential type

## Troubleshooting

### If a switch fails

- Check that you have accounts added: `./ccswitch.sh --list`
- Verify Claude Code is closed before switching
- Try switching back to your original account

### If you can't add an account

- Make sure you're logged into Claude Code first
- Check that you have `jq` installed
- Verify you have write permissions to your home directory

### If Claude Code doesn't recognize the new account

- Make sure you restarted Claude Code after switching
- Check the current account: `./ccswitch.sh --list` (look for "(active)")

## Cleanup/Uninstall

To stop using this tool and remove all data:

1. Note your current active account: `./ccswitch.sh --list`
2. Remove the backup directory: `rm -rf ~/.claude-switch-backup`
3. Delete the script: `rm ccswitch.sh`

Your current Claude Code login will remain active.

## Security Notes

- Credentials stored in macOS Keychain or files with 600 permissions
- Authentication files are stored with restricted permissions (600)
- The tool requires Claude Code to be closed during account switches

## License

MIT License - see LICENSE file for details
