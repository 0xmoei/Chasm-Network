# To Add 2nd scout in Chasm Network

## Step 1: Mint another NFT
Visit https://scout.chasm.net/private-mint

## Step 2: Config Scout
Enter root
```cosnole
cd ~
```

Create 2.env
```console
cp .env 2.env
```

Config 2.env
```console
nano 2.env
```

You Must update a few things in new 2.env file
`PORT=` Change 3001 to 3002
`SCOUT_NAME=` New favorite name
`SCOUT_UID=` New one from Step 1: Mint NFT
`WEBHOOK_API_KEY=` New one from Step 1: Mint NFT
`WEBHOOK_URL=` Change port: 3001 to 3002

Add this code to the end of the .env file
```console
NODE_ENV=production
```

## Step 3: Run Scout
```console
# Open Port
sudo ufw allow 3002

# Pull the code from DockerHub
docker pull johnsonchasm/chasm-scout
```

Note: We update arguments of `--env-file`, `-p`, `--name` flags in below command
```console
# Start the docker file
docker run -d --restart=always --env-file ./2.env -p 3002:3002 --name scout2 johnsonchasm/chasm-scout
```

## Step 4: Verify
```console
# Should get "OK" response
curl localhost:3002
```

```console
source ./2.env
curl -X POST \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer $WEBHOOK_API_KEY" \
     -d '{"body":"{\"model\":\"gemma-7b-it\",\"messages\":[{\"role\":\"system\",\"content\":\"You are a helpful assistant.\"}]}"}' \
     $WEBHOOK_URL
```

```console
docker logs scout2
```

If your logs is like following pictures, you are fine. Just wait until your node yellow light turns GREEN

![image](https://github.com/user-attachments/assets/a55d5d0a-74e8-4a3f-ab02-50525c5b109d)

![image](https://github.com/user-attachments/assets/37e972e8-e8f2-4470-9040-ebcc025d4361)





