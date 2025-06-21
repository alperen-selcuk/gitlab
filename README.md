# gitlab

this repo include installation of self hosted gitlab server.

## docker-compose

you can run gitlab server with this compose file.

```
version: '3.7'

services:
  gitlab:
    image: gitlab/gitlab-ce:latest
    container_name: gitlab
    hostname: yourdomain
    restart: always
    shm_size: '256m'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'https://yourdomain'
    ports:
      - "80:80"
      - "443:443"
      - "2222:22"
    volumes:
      - './config:/etc/gitlab'
      - './logs:/var/log/gitlab'
      - './data:/var/opt/gitlab'
```

## gitlab-runner

you can run gitlab runner on linux, docker or kubernetes

#### linux

gitlab-runner register \
  --non-interactive \
  --url "https://gitlab.com/" \
  --token "$RUNNER_TOKEN" \
  --executor "docker" \
  --docker-image alpine:latest

#### docker

```
docker run -d --name gitlab-runner --restart always \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
```


### kubernetes

```
helm install  gitlab-runner --namespace gitlab-runner -f gitlab-runner-values.yaml gitlab/gitlab-runner --create-namespace
```
