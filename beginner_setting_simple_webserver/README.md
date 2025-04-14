# Instructions

## Install Docker
Make sure Docker is installed on your system.
## Create the project directory
Create a new folder and an `index.html` file inside it that will be served by Nginx.
## Write the Dockerfile
A Dockerfile is a script that defines the environment of the container. It tells Docker what base image to use, what files to include, and what ports to expose:
```
FROM nginx:alpine
COPY ./index.html /usr/share/nginx/html
EXPOSE 80
```
## Build the Docker image
Navigate to your project folder and build the image using:
```
docker build -t my-nginx-app .
```
## Run the container
Start the container and map port 80 of the container to port 8080 on your machine:

```
docker run -d -p 8080:80 my-nginx-app
```
## Access the web server
Open your browser and navigate to http://localhost:8080 to see your created page.