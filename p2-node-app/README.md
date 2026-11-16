# Project 2: Node.js app with MongoDB

## Goal
Package a Node.js profile app in its own Docker image and run it with a MongoDB database on a Docker network, with the data kept in a volume.

## How it fits together
browser -> my-app container (port 3000) -> mongodb container (reached by the name "mongodb" over the network my-net) -> volume mongo-data

## The Dockerfile
- FROM node:20 : base image with Node.js
- WORKDIR /app : folder inside the image
- COPY package*.json ./ : copy only the dependency list first
- RUN npm install : install libraries while building (cached while package.json is unchanged)
- COPY . . : copy the rest of the code
- EXPOSE 3000 : documents the port
- CMD ["node", "server.js"] : starts the app when a container runs

## Run it
    docker build -t my-app:1.0 .
    docker network create my-net
    docker run -d --name mongodb --network my-net -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password -v mongo-data:/data/db mongo:7
    docker run -d --name my-app --network my-net -p 3000:3000 my-app:1.0
Then open http://localhost:3000

## What I learned
- Inside a container, localhost means the container itself, so the app uses the address mongodb, which Docker resolves on the shared network.
- The database has no published port. Only containers on the network can reach it.
- A volume keeps the data when containers are deleted. I deleted both containers, recreated them, and my saved profile was still there.
- Copying package.json before the code makes rebuilds fast because the npm install layer is cached.

## Known issues and next steps
- node:20 is out of support. Next version: my-app:1.1 on a current Node LTS with an alpine base.
- The database username and password are written in server.js. Better: read them from environment variables.
