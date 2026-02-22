# status and health
wsl
openclaw sessions

# Check your usage over 24h
# Set your ANTHROPIC_API_KEY environment variable first
curl https://api.anthropic.com/v1/usage -H "Authorization: Bearer $ANTHROPIC_API_KEY" | jq '.usage.cache'



# reload

openclaw gateway restart
# Stop then start
openclaw gateway stop
openclaw gateway start
# Or check status first
openclaw gateway status
openclaw gateway restart


# shell
openclaw tui
openclaw tui --url ws://127.0.0.1:18789 --token <token>
openclaw tui --session main --deliver

## from inside tui
session_status

openclaw doctor --fix


