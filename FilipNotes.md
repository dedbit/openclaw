
Running from git bash terminal
```bash
cd /d/dev/12c
./docker-setup.sh
```


```Powershell
docker build -t openclaw:local -f Dockerfile .

bash docker-setup.sh
bash docker-setup.sh 
```

```Powershell
# restart
shutdown -r -t 0
```


```Powershell
wsl -- bash -c "cd /mnt/d/dev/12c/OpenClaw && ./docker-setup.sh"

# option 2
wsl
cd /mnt/d/dev/12c/OpenClaw
./docker-setup.sh

# upgrade ubuntu image
wsl --set-version Ubuntu-24.04 2

wsl -d Ubuntu-24.04
cd /mnt/d/dev/12c/OpenClaw
./docker-setup.sh
```

# configurations
Set chown on .openclaw 
```Powershell
wsl -d Ubuntu-24.04
sudo chown -R 1000:1000 ~/.openclaw
sudo chown -R 1000:1000 /home/ubuntu/.openclaw
sudo chown -R 1000:1000 /mnt/d/dev/12c/OpenClaw/.openclaw
exit
```

# Errors
WhatsApp login failed: Error: EACCES: permission denied, mkdir '/home/node/.openclaw/credentials'

Error: EACCES: permission denied, open '/home/node/.openclaw/openclaw.json.7.bb24d182-6cef-4bfb-bbe3-f611ff230480.tmp'


```
wsl -d Ubuntu-24.04
cd /mnt/d/dev/12c/OpenClaw
./docker-setup.sh
```

```Powershell
# Clean Docker build cache
docker builder prune -a -f

# Remove any partial openclaw images
docker rmi openclaw:local 2>$null

# Restart Docker Desktop
Restart-Service -Name "com.docker.service" -Force

# Wait 30 seconds for Docker to start, then retry in WSL
wsl -d Ubuntu-24.04
cd /mnt/d/dev/12c/OpenClaw
./docker-setup.sh


```


# chatgpt token

http://localhost:1455/auth/callback?code=ac_dSAnh7fl7eGrQr2PvF9MQVe4xiXEy_Po1Erm0_m3QsE.ZGr2-uM3aaorsJxOc_0nlk8scDUiikYJTMMK3NQKUz8&scope=openid+profile+email+offline_access&state=743e9fffbf89bcd51ef74ecb87a67de0



docker compose run --rm openclaw-cli channels login  # For WhatsApp QR
docker compose run --rm openclaw-cli channels add --channel telegram --token <your-token>

# Code Tunnel
```Powershell
code tunnel --name lindboe-pc-openclaw --accept-server-license-terms
```


code --remote tunnel+lindboe-pc "D:\dev\12c\OpenClaw"

# restart Start claw
docker compose down
docker compose up -d openclaw-gateway


docker compose exec openclaw-gateway cat /home/node/.openclaw/devices/paired.json

# shutdown openclaw container



# test

docker compose run --rm openclaw-cli agent --message "Hello, test message" --thinking low --local


## test prompts:

Go to https://dr.dk and give me the top 3 news headlines



# google api key

https://aistudio.google.com/app/api-keys
https://console.cloud.google.com/vertex-ai/studio/settings/api-keys?project=project-3252737a-af8d-41aa-858


# Configuration
```Powershell
openclaw setup
openclaw onboard --auth-choice gemini-api-key
# Interactive prompts for API key
openclaw models set google/gemini-2.5-flash



# Run docker-setup.sh first
./docker-setup.sh

# Then use CLI in container
docker compose run --rm openclaw-cli onboard --auth-choice gemini-api-key
docker compose run --rm openclaw-cli config set agents.defaults.model.primary google/gemini-2.5-flash
```


## common patterns
# Run any CLI command inside the container
docker compose run --rm openclaw-cli <command>

# Run commands from host
docker compose run --rm openclaw-cli dashboard --no-open
docker compose run --rm openclaw-cli dashboard --no-openard --no-open



# Examples from the setup script and docs:
docker compose run --rm openclaw-cli onboard
docker compose run --rm openclaw-cli channels login
docker compose run --rm openclaw-cli channels add --channel telegram --token <token>
docker compose run --rm openclaw-cli dashboard --no-open
docker compose run --rm openclaw-cli devices list

# Interactive shell into the running gateway
docker compose exec openclaw-gateway sh
node dist/index.js dashboard --no-open
node dist/index.js devices list
node dist/index.js channels login


\\wsl$\docker-desktop-data\data\docker\volumes\openclaw_openclaw-workspace\_data


# Health check
docker compose exec openclaw-gateway node dist/index.js health --token "$OPENCLAW_GATEWAY_TOKEN"

# Tail logs
docker compose logs -f openclaw-gateway


# Whats app conf
# First,  add basic WhatsApp config
docker compose run --rm openclaw-cli config set "channels.whatsapp.dmPolicy" "allowlist"
docker compose run --rm openclaw-cli config set "channels.whatsapp.allowFrom" '["*"]'

# Then try login (for the default account)  
docker compose run --rm openclaw-cli channels login --verbose

# restart
docker compose restart openclaw-gateway

docker compose down
docker compose up -d openclaw-gateway
docker compose logs -f openclaw-gateway



# files in container
\\wsl$\docker-desktop-data\data\docker\volumes\openclaw_openclaw-workspace\_data


# .openclaw path
\\wsl.localhost\Ubuntu-24.04\home\ubuntu\.openclaw\openclaw.json
