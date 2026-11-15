# Project 1: Static website in a custom Docker image

## Goal
Package a small website into my own Docker image and run it with the Apache web server.

## Files
- site/index.html : the web page
- Dockerfile : the recipe that builds the image
- .dockerignore : files Docker leaves out of the build

## The Dockerfile
- FROM httpd:2.4 : start from the official Apache image
- COPY ./site/ /usr/local/apache2/htdocs/ : put my website inside the image
- EXPOSE 80 : documents the port Apache listens on (it does not open it)

## How to run it
    docker build -t my-site:1.0 .
    docker run -d --name my-site -p 8085:80 my-site:1.0
Then open http://localhost:8085

## What I learned
- An image is a read-only template. A container is a running copy of it.
- With the website inside the image, the container no longer reads my folder.
- A new version is a new image with a new tag (1.0 and 1.1 can run side by side).
- docker logs, docker exec, docker history and docker inspect show what is really happening.
