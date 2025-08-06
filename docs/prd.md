# Goal

Write and document a Python script that can update my Slack presence, status message/emoji and Do Not Disturb (DND) settings across multiple workspaces in one command.

## Environment and constraints:
	1.	Use the official Slack Python SDK (slack_sdk), which provides wrappers around the Web API methods.
	2.	My user OAuth tokens for each workspace should not be hard‑coded.  The script must load them from a configuration file or environment variables.  A simple way is to accept a JSON or YAML config file mapping each workspace name to its user token (e.g., { "personal": "xoxp-...", "metaprop": "xoxp-..." }) and read it at startup.  If no config is provided, the script can look for environment variables like SLACK_TOKEN_PERSONAL, SLACK_TOKEN_METAPROP, etc.
	3.	By default, the script should set presence to “auto” (active/online) and clear any status message or DND settings if no options are provided.

##Slack API usage:
– For presence, call users.setPresence with presence="auto" for online or presence="away" for away ￼.
– For DND, use dnd.setSnooze with a num_minutes value to enable snooze and dnd.endDnd to turn it off ￼.
– For status, call users.profile.set with a JSON profile payload containing status_text, status_emoji and status_expiration (0 for no expiration).
Make sure to pass the appropriate user token in the Authorization header or when instantiating slack_sdk.WebClient.

##Script behaviour:
– Accept command‑line options or arguments for:
• --status-text (string) – optional custom status message.
• --status-emoji (string) – optional emoji (e.g., :coffee:).
• --presence (choices: auto, away) – optional presence override.
• --dnd (integer) – optional DND duration in minutes.  If supplied, enable DND for that many minutes; if omitted, leave DND unchanged.
• --workspaces (comma‑separated list) – optional subset of workspace names to target; if omitted, operate on all configured workspaces.
• --config (file path) – optional path to the JSON/YAML config with tokens.
– The script should iterate through the selected workspaces, instantiate a WebClient with each token, and make the relevant API calls.
– Include basic error handling: if an API call returns an error (e.g., invalid_scope or rate limit), log it and continue to the next workspace.
– Add a --dry-run flag that prints what would be done without calling the API (useful for testing).

##Defaults:
– If --status-text and --status-emoji are not provided, clear any existing status.
– If --presence is not provided, set presence to auto (active).
– If --dnd is not provided, leave DND unchanged.


## Resources to Read
	1.	Presence API usage:
– Educative “Manipulate Presence” lesson – explains that users.setPresence accepts auto or away, and shows how to call the endpoint with a user token ￼.
Link: https://www.educative.io/courses/automations-with-the-slack-api-in-javascript/manipulate-presence
	2.	Do Not Disturb (DND) examples:
– Owen Rumney’s Home Assistant article – demonstrates calling dnd.setSnooze with num_minutes to enable DND and dnd.endDnd to disable it, and shows updating presence via users.setPresence ￼.
Link: https://www.owenrumney.co.uk/home-assistant-status-setter/
	3.	Status message update example:
– SlackStatusChanger Python script – a simple example of using the Slack SDK’s users_profile_set method to set status_text, status_emoji and status_expiration in a user’s profile.
Link: https://raw.githubusercontent.com/doitintl/SlackStatusChanger/main/main.py
	4.	Multi‑workspace configuration pattern:
– slack‑status (greenstatic) README – explains creating a Slack app, obtaining a user token via slack-status init, and shows a sample config file listing multiple workspaces with tokens and groups ￼ ￼.
Link: https://raw.githubusercontent.com/greenstatic/slack-status/master/README.md
	5.	Alternative CLI for reference:
– slack‑status‑cli README – lists the scopes required for a Slack app, and demonstrates commands such as st away --dnd <duration> for setting away status with DND ￼ ￼.
Link: https://raw.githubusercontent.com/yankeexe/slack-status-cli/master/README.md
    6. Slack API presence and status
    Link: https://api.slack.com/apis/presence-and-status

# Output
- The script should be self‑contained in one .py file, include clear function definitions (e.g., set_presence(), set_status(), set_dnd()), and provide a helpful usage message when run with --help.
- The README.md should reflect how to setup and use the application.

