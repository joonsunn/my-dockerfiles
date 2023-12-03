# Docker starter guide

## Docker individual containers

### Step 1: make dockerfile

```dockerfile
FROM node:20

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["npm","run", "start"]
```

### Step 2: make .dockerignore

```
node_modules
Dockerfile
.git
.gitignore
.dockerignore
```

### Step 3: create docker image

```bash
docker build -t "image name" .
```

Trailing ```.``` is actually ```destination directory```, but for simplicity's sake just build it in current directory.

### Step 4: create docker container from image

```bash
docker run -p 3000:3000 --name "image name" "container name"
```

If ```--name``` flag not run to give container name, docker will give random name

### Step 5: stop a container

```bash
docker stop "container name"
```

### Step 6: start a container that was already built previously

<pre>docker <b>start</b> "container name"</pre>

### Step 7: update container based on latest image

No shortcut. Need to delete old container and build from latest image. This means need to run

```bash
docker rm "container name"
```

```bash
docker build -t "image name" .
```

```bash
docker run -p 3000:3000 --name "image name" "container name"
```

## Docker compose

### Step 1: Create docker-compose.yml at root directory of app

```yaml
version: '3'
services:
  react-app:
    build: .
    ports:
      - "3000:3000"
```

### Step 2: Create docker compose container

Navigate to root directory of app. Then run:

```bash
docker compose up
```

Alternatively can do

```bash
docker compose build
```

### Step 3: Stop currently running Docker Compose container

```bash
docker compose stop "service name"
```

```service name``` is optional.

### Step 4: Start a previously created Docker Compose container

```bash
docker compose start "container name"
```

```container name``` is optional.

### Misc

Basically, ```docker compose``` has same commands as ```docker```, such as ```rm``` to remove container, and ```rmi``` to remove image.

Based on cheatsheets:

1. <https://gist.github.com/jonlabelle/bd667a97666ecda7bbc4f1cc9446d43a>
2. <https://dockerlabs.collabnix.com/docker/cheatsheet/>