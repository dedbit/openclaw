# This file contains instructions on initial setup of openclaw

## Whatsapp
docker compose run --rm openclaw-cli channels login

# Add a specific contact (use their phone number in E.164 format)
docker compose run --rm openclaw-cli config set channels.whatsapp.accounts.default.allowlist "['+4553630260']"

# Config file
.openclaw\openclaw.json

# Start the gateway
docker compose up -d openclaw-gateway

# Or check channel status
docker compose run --rm openclaw-cli channels status

# Check gateway logs
docker compose logs -f openclaw-gateway



## try running from wsl
wsl
# In WSL (not PowerShell)
cd ~
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# Set ownership for Docker node user (UID 1000)
sudo chown -R 1000:1000 ~/.openclaw

# Run setup
./docker-setup.sh

# Verify
docker ps
docker compose run --rm openclaw-cli channels status





# Check status
wsl -d Ubuntu-24.04 bash -c "cd /mnt/d/dev/12c/OpenClaw && docker compose run --rm openclaw-cli channels status"

# Link WhatsApp
wsl -d Ubuntu-24.04 bash -c "cd /mnt/d/dev/12c/OpenClaw && docker compose run --rm openclaw-cli channels login"



docker compose run --rm openclaw-cli channels status


# Enter WSL (this is the right approach!)
wsl -d Ubuntu-24.04 bash
cd /mnt/d/dev/12c/OpenClaw
openclaw channels status

# All commands use Docker (which uses named volumes)
docker compose run --rm openclaw-cli channels status
docker compose run --rm openclaw-cli channels login

# Convenience alias
alias oc='docker compose run --rm openclaw-cli'
oc channels status




# Check if gateway is still running
docker ps --filter 'name=openclaw-gateway'

# Check WhatsApp status (should show "linked" and "connected")
docker compose run --rm openclaw-cli channels status

# View gateway logs
docker compose logs -f openclaw-gateway

docker logs openclaw-openclaw-gateway-1 --tail 100 2>&1 | tail -50

# Watch the logs in real-time
docker logs -f openclaw-openclaw-gateway-1


# add agent
docker compose run --rm openclaw-cli agents add main
