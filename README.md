# Slack Status CLI

A Python CLI tool to update your Slack presence, status message/emoji, and Do Not Disturb (DND) settings across multiple workspaces with a single command.

## Features

- **Multi-workspace support**: Update status across all your Slack workspaces simultaneously
- **Flexible token management**: Load tokens from environment variables or config files
- **Complete status control**: Set presence (auto/away), custom status messages, emojis, and DND
- **Dry run mode**: Preview changes before applying them
- **Comprehensive error handling**: Continues processing other workspaces if one fails
- **Multiple config formats**: Supports JSON and YAML configuration files

## Installation

1. Clone this repository:
   ```bash
   git clone <repository-url>
   cd slack-status-cli
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Make the script executable:
   ```bash
   chmod +x slack-status
   ```

## Setup

### Getting Slack Tokens

You need a user OAuth token for each Slack workspace. To get these tokens:

1. Go to [Slack API Apps](https://api.slack.com/apps)
2. Create a new app for each workspace
3. Add the following OAuth scopes under "User Token Scopes":
   - `users:write` - Update your presence and status
   - `users.profile:write` - Update your profile status
   - `dnd:write` - Update your DND settings
4. Install the app to your workspace
5. Copy the "User OAuth Token" (starts with `xoxp-`)

### Token Configuration

You can provide tokens in two ways:

#### Option 1: Environment Variable
```bash
export SLACK_TOKENS="xoxp-token1,xoxp-token2,xoxp-token3"
```

#### Option 2: Configuration File
Create a JSON or YAML file with your tokens:

**config.json:**
```json
{
  "tokens": [
    "xoxp-your-personal-workspace-token",
    "xoxp-your-work-workspace-token"
  ]
}
```

**config.yaml:**
```yaml
tokens:
  - xoxp-your-personal-workspace-token
  - xoxp-your-work-workspace-token
```

## Usage

### Basic Examples

```bash
# Deep Work mode - sets status, emoji, and DND for 3 hours
./slack-status --status-text "Deep Work" --status-emoji ":brain:" --dnd-on 180

# Clear status completely and disable DND (return to fully active)
./slack-status --dnd-off

# Set status message and emoji
./slack-status --status-text "In a meeting" --status-emoji ":calendar:"

# Set presence to away and enable DND for 1 hour
./slack-status --presence away --dnd-on 60

# Use a config file
./slack-status --config config.json --status-text "Working from home"

# Preview changes without applying them
./slack-status --dry-run --status-text "Testing" --verbose
```

### Command Line Options

```
--status-text TEXT         Status message text
--status-emoji EMOJI       Status emoji (e.g., :coffee:)  
--status-expiration MINUTES  Status expiration in minutes (0 = never, default: 0)
--presence {auto,away}     Set presence (default: auto)
--dnd-on MINUTES          Enable DND for specified minutes
--dnd-off                 Disable DND  
--config PATH             Path to config file (JSON or YAML)
--dry-run                 Preview actions without executing them
--verbose                 Enable verbose logging
--help                    Show help message
```

### Advanced Examples

```bash
# Set temporary status that expires in 8 hours (480 minutes)
./slack-status --status-text "Working from home" --status-emoji ":house:" --status-expiration 480

# Go away and enable DND until end of day
./slack-status --presence away --dnd-on 480

# Use YAML config with verbose output
./slack-status --config ~/.slack-status.yaml --status-text "Deep work" --verbose

# Test configuration without making changes
./slack-status --config config.json --dry-run --verbose
```

## Default Behavior

- **Presence**: Set to `auto` (active/online) if not specified
- **Status**: Cleared if `--status-text` and `--status-emoji` are not provided  
- **DND**: Left unchanged if neither `--dnd-on` nor `--dnd-off` is specified
- **Status Expiration**: Never expires (0) if not specified

## Error Handling

The tool will:
- Continue processing remaining workspaces if one fails
- Log specific error messages for troubleshooting
- Return exit code 1 if any workspace fails to update
- Handle rate limiting and common API errors gracefully

## Troubleshooting

### Common Issues

1. **"No Slack tokens found"**
   - Ensure `SLACK_TOKENS` environment variable is set, or
   - Provide a valid config file with `--config`

2. **"missing_scope" errors**
   - Your Slack app needs the required OAuth scopes (see Setup section)
   - Reinstall your Slack app with the correct permissions

3. **"invalid_auth" errors**  
   - Check that your tokens are valid and haven't expired
   - Ensure tokens start with `xoxp-` (user tokens, not bot tokens)

4. **Rate limiting**
   - The tool handles rate limits automatically with backoff
   - Use `--verbose` to see detailed API responses

### Debug Mode

Use `--verbose --dry-run` to troubleshoot without making changes:

```bash
./slack-status --config config.json --status-text "Test" --verbose --dry-run
```

## License

MIT License - see LICENSE file for details.
