# MultiCloud, DevOps & AI Challenge - Day 2

## Part 1 - Docker

### Step 1: Install Docker on EC2

Execute the following commands:

```hcl
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo docker run hello-world
sudo systemctl enable docker
docker --version
sudo usermod -a -G docker $(whoami)
newgrp docker
```

![Install Docker](images/install-docker.png)

### Step 2: Create Docker image for CloudMart

### Backend

1. Create folder and download source code:

```hcl
mkdir -p challenge-day2/backend && cd challenge-day2/backend
wget https://tcb-public-events.s3.amazonaws.com/mdac/resources/day2/cloudmart-backend.zip
unzip cloudmart-backend.zip
```

![Mkdir](images/mkdir-backend.png)

2. Create .env file:

```hcl
nano .env
```

Content of .env:

```hcl
PORT=5000
AWS_REGION=us-east-1
BEDROCK_AGENT_ID=<your-bedrock-agent-id>
BEDROCK_AGENT_ALIAS_ID=<your-bedrock-agent-alias-id>
OPENAI_API_KEY=<your-openai-api-key>
OPENAI_ASSISTANT_ID=<your-openai-assistant-id>
```

![env](images/env-backend.png)

3. Create Dockerfile:

```hcl
nano Dockerfile
```

Content of Dockerfile:

```hcl
FROM node:18
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]
```

![DockerFile](images/dockerfile-backend.png)

### Frontend

1. Create folder and download source code:

```hcl
cd ..
mkdir frontend && cd frontend
wget https://tcb-public-events.s3.amazonaws.com/mdac/resources/day2/cloudmart-frontend.zip
unzip cloudmart-frontend.zip
```

![mkdir](images/mkdir-frontend.png)

2. Create Dockerfile:

```hcl
nano Dockerfile
```

Content of Dockerfile:

```hcl
FROM node:16-alpine as build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:16-alpine
WORKDIR /app
RUN npm install -g serve
COPY --from=build /app/dist /app
ENV PORT=5001
ENV NODE_ENV=production
EXPOSE 5001
CMD ["serve", "-s", ".", "-l", "5001"]
```
