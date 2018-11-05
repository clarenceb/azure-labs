# Demo 1 - Running a container locally on Docker

## Running container with Docker

Get the sample code:

```
git clone https://github.com/clarenceb/docker-django-webapp-linux
cd docker-django-webapp-linux
```

Pull the base image (optional):

```
docker pull python:3.4
```

Build the container (using the `Dockerfile`):

```
docker build -t webapp-linux .
```

Check cached local Docker images:

```
docker image ls
```

Run the container:

```
docker run -d -p 8080:8000 --name webapp webapp-linux
```

Check running containers:

```
docker container ls
```

Test the web app locally:

Browse to http://localhost:8080

Tail the logs:

```
docker logs -f webapp
```

Re-load the browser tab.

Check logs in terminal.

Exit with `CTRL+C`

Exec into container:

```
docker exec -ti webapp bash
```

Run some commands in container:

```
cat /etc/os-release
ps -ef
top # 'q' to exit top
```

Exit container with `exit`.

Cleanup:

```
docker rm -f webapp
```

Tag image:

```
docker tag webapp-linux:latest azurecontainerdemos/webapp-linux:v1.0.1
```

Push image to Docker Hub:

```
docker push azurecontainerdemos/webapp-linux:v1.0.1
```

Fallback for pushed image - use `azurecontainerdemos/webapp-linux:v1.0.0` which is already pushed.

