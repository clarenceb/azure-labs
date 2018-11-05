# Demo 1 - Running a container locally on Docker

## Running containers with Docker

Get the sample code:

```
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
cd azure-voting-app-redis
```

Pull the redis image for backend:

```
docker pull redis
```

Build the frontend container:

```
docker build -t azure-vote-front azure-vote/
```

Check cached local Docker images:

```
docker image ls
```

Run the backend container:

```
docker run -d -p 6379:6379 --name azure-vote-back redis:latest
```

Run the frontend container:

```
docker run -d -p 8080:80 --env REDIS=azure-vote-back --name azure-vote-front azure-vote-front:latest
```

Check running containers:

```
docker container ls
```

Test the web app locally:

Browse to http://localhost:8080

Cleanup:

```
docker rm -f azure-vote-back
docker rm -f azure-vote-front
```

## Optional: Docker-Compose

```
docker-compose up
```

Show `docker-compose.yml`

Test the web app locally:

Browse to http://localhost:8080

`CTRL+C` to exit docker-compose.
