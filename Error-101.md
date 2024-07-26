## If you have Error: 101 issue with WebSocket

```console
# Stop and Remove docker
docker stop scout
docker rm scout

# Pull the code from DockerHub
docker pull chasmtech/chasm-scout:0.0.4

# Start the docker file
docker run -d --restart=always --env-file ./.env -p 3001:3001 --name scout chasmtech/chasm-scout:0.0.4
```
