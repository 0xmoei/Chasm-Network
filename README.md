# Chasm-Network

We run a node on Chasm Network and collect points

## System Requirements (Minimum-Recommended)
| Ram | cpu     | disk                      |
| :-------- | :------- | :-------------------------------- |
| `1 GB`      | `1 Core` | `5-20 GB SSD` |

## Step 1: Mint NFT to Get Scout ID and API Key 
Visit https://scout.chasm.net/private-mint

You need 0.3 $MNT on Mantle network in your wallet, you can obtain from [squid](https://app.squidrouter.com/)

![Screenshot_100](https://github.com/user-attachments/assets/76c84ff6-656f-4c61-9420-fb345e0a6040)


You will obtained `WEBHOOK_API_KEY` and `SCOUT_UID` from here.

## Step 2: Get Groq API Key
Sign up for an account at [Groq](https://console.groq.com/keys) to `get GROQ_API_KEY`

![Screenshot_99](https://github.com/user-attachments/assets/d7eb8406-cea3-4cec-a66c-006872aca31d)

## Step 3: Optional: Get OpenRouter & OpenAI API keys
This step is optional
- [OpenRouter](https://openrouter.ai/) for `OPENROUTER_API_KEY`

- [OpenAI](https://platform.openai.com/api-keys) for `OPENAI_API_KEY`

- You can buy phone-numbers for OpenAI [here](https://smspva.com/?ref=724518) by crypto

## Step 4: Install Dependecies
```console
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

# Install Docker
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Step 5: Setup
### 5-1: Enter root
```console
cd ~
```

### 5-2: Create and config .env file
```console
nano .env
```

Copy the following config in your `.env` file and then update following variables in front of `=`
- `SCOUT_NAME`: Your favorite scout name
- `SCOUT_UID`: From Step 1: Mint NFT
- `WEBHOOK_API_KEY`: From Step 1: Mint NFT
- `WEBHOOK_URL`: Replace your http://<ip>:<PORT> | Update with your vps's IP and port | e.g. http://x.x.x.x:3001/
- `GROQ_API_KEY`: From Step 2: Get Groq API Key
- `OPENROUTER_API_KEY`: Step 3 (optional)
- `OPENAI_API_KEY`: Step 3 (optional)
```console
PORT=3001
LOGGER_LEVEL=debug

# Chasm
ORCHESTRATOR_URL=https://orchestrator.chasm.net
SCOUT_NAME=myscout
SCOUT_UID=
WEBHOOK_API_KEY=
WEBHOOK_URL=

# Chosen Provider (groq, openai)
PROVIDERS=groq
MODEL=gemma2-9b-it
GROQ_API_KEY=

# Optional
OPENROUTER_API_KEY=
OPENAI_API_KEY=
```
To save .env file and exit: `Ctrl + X + Y` , `Enter`

### 5-3: Run the scout
```console
# Open Port
ufw allow 3001

# Pull the code from DockerHub
docker pull johnsonchasm/chasm-scout

# Start the docker file
docker run -d --restart=always --env-file ./.env -p 3001:3001 --name scout johnsonchasm/chasm-scout
```

## 6: Verify

### 6-1: Test Server Response:
```console
# Should get "OK" response
curl localhost:3001
```

### 6-2: Test LLM
```console
source ./.env
curl -X POST \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer $WEBHOOK_API_KEY" \
     -d '{"body":"{\"model\":\"gemma-7b-it\",\"messages\":[{\"role\":\"system\",\"content\":\"You are a helpful assistant.\"}]}"}' \
     $WEBHOOK_URL
```

### 6-3: Logs
```console
docker logs scout
```

### Check leaderboard
https://scout.chasm.net/leaderboard

### Optional: Kill and stop docker
```console
docker stop scout && docker rm scout
```

### Optional: Restart Docker
```console
docker restart scout
```
