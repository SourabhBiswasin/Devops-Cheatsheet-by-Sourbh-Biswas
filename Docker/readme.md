Docker command cheat sheet links

https://quickref.me/docker.html

https://www.geeksforgeeks.org/docker-cheat-sheet/?ref=lbp

https://dockerlabs.collabnix.com/docker/cheatsheet/

# TIPS

1) To exit from a running container in a terminal without stopping the container use:-
```bash
CTRL+P + CTRL+Q
```
Note: - exit command inside container terminal will stop the container.

2) To use container terminal or to go inside container use:-
```bash
docker exec -it <container_name> bash
```

# Docker volume mapping
 Docker volume mapping is used to connect a local or other container volume to a desired container volume. And during this process the data in the desired container will create duplicate data into mapped volume as well. This data duplication process is bidirection means if we create any data in local or other container mappped volume, it will also be duplicate to desired mapped running container volume as well.

Example Code
```bash
docker run -d --name=<give_container_name> -v D:/<volume location of local host or of another container>:/<volume location_of_container_or_foldername_of_desired_container> <image_name> bash
```
-v refer to volume mapping
-d refer to run container is detch mode

Note: - Volume mapping can be done on while creating a new container only. WE can't do it on already available containers.

## Remove docker volume mapping

First stop the container and remove the volume using command

```bash
docker rm volume <volume_name>
```
or 

```bash
docker rm volume prune
```
# Docker port mapping

```bash
docker run -d -p 8009<enter_port_as_per_our_need>:80<default_port_of_container_or_image> <image_name>
```
-p refer to port mapping
-d refer to run container is detch mode

## Rename docker image

```bash
docker tag <old_image_name> <new_image_name>
```

# [Docker compose documentation](https://docs.docker.com/reference/compose-file/services/)
 - Docker compose is used to manage multiple container such as creating containers, volume mapping, port mapping and networks.

## [Examples of docker compose files](https://docs.github.com/en/actions/use-cases-and-examples)

# Docker file create: -  
Sample template

```bash
# Use an official Ubuntu base image
FROM ubuntu:20.04
# Note for FROM command:- Enter the image name in FROM

COPY <enter_location_path_here>
# Note for COPY command:- Add . . to copy from root folder/directory Example: - target/Sourabh-Calculator.jar

# RUN: Executes commands at build time to install software, download dependencies, or configure the environment. The result is saved in the image.
RUN apt-get update && apt-get install -y curl
# Note for COPY command:- give any linux command in the RUN. We can write multiple RUN commands in single docker file

# Print the version of curl
RUN curl --version

# CMD: Specifies the default command to be executed when a container starts. It can be overridden when running a container.
# Set default command to display the curl version
CMD ["curl", "--version"]

# ENTRYPOINT: Defines the main executable for the container, which can't be easily overridden. However, additional arguments can be passed when the container starts. When combined with CMD, CMD provides the default arguments for ENTRYPOINT.
# Set entrypoint to curl command
ENTRYPOINT ["curl"]
```

# Docker Multi-Stage Dockerfile

A multi-stage Dockerfile includes multiple stages:

- Build Stage: Build or compile the application.
- Runtime Stage: Copy the output (e.g., binaries, compiled code) to the minimal runtime environment.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Example: Python Multi-Stage Build

```bash
# Stage 1: Build Stage
FROM python:3.10 AS builder

# Set the working directory
WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application source code
COPY . .

# Stage 2: Final Stage
FROM python:3.10-slim

# Set the working directory
WORKDIR /app

# Copy only the necessary files from the builder stage
COPY --from=builder /app /app

# Expose the port and set the entry point
EXPOSE 5000
CMD ["python", "app.py"]
```
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Example: Java Multi-Stage Build

```bash
# Stage 1: Build the Java JAR file
FROM maven:3.8.4-openjdk-11 AS builder
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn package -DskipTests

# Stage 2: Run the Java application
FROM openjdk:11-jre-slim
WORKDIR /app
COPY --from=builder /app/target/My-Calculator.jar My-Calculator.jar
EXPOSE 8080
CMD ["java", "-jar", "My-Calculator.jar"]
```

Note: - 
Use lightweight base images (e.g., alpine or slim) for the final stage.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Example of a multi-stage Dockerfile for a React.js application.

```bash
# Stage 1: Build the React application
FROM node:18 AS builder

# Set the working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package.json package-lock.json ./
RUN npm install

# Copy the source code and build the app
COPY . .
RUN npm run build

# Stage 2: Serve the React app using Nginx
FROM nginx:alpine

# Copy the built files from the builder stage to Nginx's html directory
COPY --from=builder /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
```

Explanation of Stages
Stage 1 - Builder:

Uses the official Node.js image to build the React app.
Installs dependencies (npm install) and creates an optimized production build (npm run build).
The output is stored in the /app/build directory.
Stage 2 - Serve:

Uses the lightweight Nginx image to serve the static React files.
Copies the build files from the builder stage into Nginx's serving directory (/usr/share/nginx/html).

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Example for multi-stage Dockerfile for a Next.js application.

```bash
# Stage 1: Build the Next.js application
FROM node:18 AS builder

# Set the working directory
WORKDIR /app

# Install dependencies
COPY package.json package-lock.json ./
RUN npm install

# Copy the source code
COPY . .

# Build the Next.js application
RUN npm run build

# Stage 2: Run the application
FROM node:18-slim

# Set the working directory
WORKDIR /app

# Install only production dependencies
COPY package.json package-lock.json ./
RUN npm install --omit=dev

# Copy the build output and necessary files from the builder stage
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json

# Expose the Next.js default port
EXPOSE 3000

# Start the Next.js server
CMD ["npm", "start"]
```
Explanation of Stages

Stage 1 - Build Stage (builder):
- Uses the full node:18 image to build the Next.js project.
- Copies the project files, installs dependencies, and runs the build command:

```bash
npm run build
```

- The output of the build (.next folder) is prepared for production use.

Stage 2 - Runtime Stage:

- Uses the lightweight node:18-slim image to run the application in production.
- Installs only production dependencies using:
```bash
npm install --omit=dev
```
- Copies the build output (.next), public assets, and node_modules from the builder stage.
- The CMD starts the Next.js production server.

Your Next.js project should look like this:

nextjs-app/
│-- Dockerfile
│-- package.json
│-- package-lock.json
│-- next.config.js
│-- public/
│   ├── favicon.ico
│-- pages/
│   ├── index.js
│-- components/
    ├── Header.js

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Example of a multi-stage Dockerfile for a Django application.

```bash
# Stage 1: Build Stage
FROM python:3.11 AS builder

# Set the working directory
WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the Django project
COPY . .

# Collect static files
RUN python manage.py collectstatic --noinput

# Stage 2: Runtime Stage
FROM python:3.11-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE 1  # Prevent Python from writing .pyc files
ENV PYTHONUNBUFFERED 1         # Ensure stdout/stderr are unbuffered

# Set the working directory
WORKDIR /app

# Install only runtime dependencies
COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin

# Copy the Django project
COPY --from=builder /app /app

# Expose the application port
EXPOSE 8000

# Command to run the Django app
CMD ["gunicorn", "myproject.wsgi:application", "--bind", "0.0.0.0:8000"]

```

Explanation
Stage 1 - Build Stage (builder):

- Base Image: Uses the full python:3.11 image to install dependencies and build the project.
- Dependencies:
   - Installs dependencies listed in requirements.txt.
- Static Files:
  - Runs python manage.py collectstatic to collect static files into a single location (e.g., /app/static).
- Output:
   - Everything required to run the project is prepared, including installed libraries and static files.

Stage 2 - Runtime Stage:

- Base Image: Uses the lightweight python:3.11-slim image for a smaller runtime environment.
- Dependencies:
   - Copies only the necessary dependencies and binaries from the builder stage.
- Code:
   - Copies the Django project files and collected static files from the builder stage.
- Gunicorn:
   - Runs gunicorn as the application server to serve the Django project.


Dependencies File (requirements.txt)

Ensure you have a requirements.txt file with necessary dependencies, for example:

```bash
Django==4.2
gunicorn==20.1.0
psycopg2-binary==2.9.5  # For PostgreSQL
```
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Example of multi-stage Dockerfiles for React Native projects.

```bash
# Stage 1: Build Stage
FROM node:18 AS builder

# Set the working directory
WORKDIR /app

# Install dependencies
COPY package.json package-lock.json ./
RUN npm install

# Copy project files
COPY . .

# Build the React Native app for Android
RUN npm run android:build

# Stage 2: Final Stage (Optional for saving build artifacts)
FROM alpine:latest

# Copy the built APK from the build stage
COPY --from=builder /app/android/app/build/outputs/apk/release/app-release.apk /output/app-release.apk

# Provide APK output location
CMD ["sh", "-c", "echo 'APK built successfully and stored in /output/app-release.apk'"]

```
Explanation:
- Builder Stage:
  - Uses the node:18 image to install dependencies and build the Android APK.
  - npm run android:build runs a build script defined in your package.json to produce the APK.
- Final Stage:
  - Uses a lightweight alpine image to export the APK.
  - Ensures the final image is clean and does not contain unnecessary build files.

The built APK file will be saved in the output directory on your host.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Example of multi-stage Dockerfiles for flutter projects.

```bash
# Stage 1: Build Stage
FROM cirrusci/flutter:stable AS builder

# Set the working directory
WORKDIR /app

# Copy project files
COPY . .

# Get Flutter dependencies
RUN flutter pub get

# Build the Flutter APK
RUN flutter build apk --release

# Stage 2: Final Stage
FROM alpine:latest

# Copy the built APK from the builder stage
COPY --from=builder /app/build/app/outputs/flutter-apk/app-release.apk /output/app-release.apk

# Provide APK output location
CMD ["sh", "-c", "echo 'Flutter APK built successfully and stored in /output/app-release.apk'"]
```

Explanation:
- Builder Stage:
  - Uses the official cirrusci/flutter:stable image to install Flutter SDK and build the APK.
  - Runs flutter pub get to fetch dependencies.
  - Runs flutter build apk --release to produce the release APK.
- Final Stage:
  - Uses a lightweight alpine image to export the built APK file.

The built APK will be saved in the output directory on your host.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------




