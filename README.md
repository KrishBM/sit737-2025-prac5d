# SIT737 - Cloud Native Application Development
## Task 5.2D: Dockerization - Publishing Microservice to the Cloud

This repository demonstrates the process of containerizing a Node.js microservice using Docker and publishing it to Google Cloud Artifact Registry as part of the SIT737 Cloud Native Application Development unit.

## Project Overview

This project builds upon previous work (from Week 2) to containerize a simple microservice and publish it to Google Cloud. The process involves creating a Docker image locally, configuring Google Cloud services, and pushing the image to Google Artifact Registry for cloud deployment.

## Prerequisites

To replicate this project, you'll need:

- [Git](https://github.com)
- [Node.js](https://nodejs.org/en/download/)
- [Docker](https://www.docker.com/products/docker-desktop)
- [Google Cloud SDK](https://cloud.google.com/sdk/docs/install)
- Google Cloud Platform account

## Implementation Steps

### 1. Repository Setup
```bash
# Create and clone the repository
git clone https://github.com/KrishBM/sit737-2025-prac5d.git
cd sit737-2025-prac5d
```

### 2. Node.js Project Initialization
```bash
# Initialize the Node.js project
npm init -y
```

### 3. Application Code
The application uses the same code from the Week 2 assignment. The main file is `index.js` which contains a simple Express.js server.

### 4. Docker Configuration
Created a basic Dockerfile to containerize the application:

```dockerfile
FROM node:16

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD [ "node", "index.js" ]
```

### 5. Local Docker Build and Test

```bash
# Build the Docker image
docker build -t week5img .

# Run the Docker container locally
docker run -p 3000:3000 week5img
```

### 6. Google Cloud Configuration

#### 6.1 Enable Artifact Registry
Enable the "Artifact Registry" service in Google Cloud Console and create a repository named "sit737-5d".

#### 6.2 Authentication Setup
```bash
# Authenticate with Google Cloud
gcloud auth login

# Verify project configuration
gcloud config list
gcloud config get-value project

# Configure Docker for Google Artifact Registry authentication
gcloud auth configure-docker australia-southeast2-docker.pkg.dev
```

### 7. Publishing to Google Cloud Artifact Registry

```bash
# Tag the Docker image for Google Artifact Registry
docker tag week5img australia-southeast2-docker.pkg.dev/sit737-25t1-moradiya-7db181d/sit737-5d/week5img:latest

# Verify image tagging
docker images

# Push the image to Google Artifact Registry
docker push australia-southeast2-docker.pkg.dev/sit737-25t1-moradiya-7db181d/sit737-5d/week5img:latest
```

### 8. Running the Cloud-Hosted Container

```bash
# Pull and run the container from Google Artifact Registry
docker run -p 8080:3000 australia-southeast2-docker.pkg.dev/sit737-25t1-moradiya-7db181d/sit737-5d/week5img:latest
```

Access the microservice at http://localhost:8080

### 9. Resource Cleanup

```bash
# Delete the Docker image from Artifact Registry
gcloud artifacts docker images delete australia-southeast2-docker.pkg.dev/sit737-25t1-moradiya-7db181d/sit737-5d/week5img:latest --delete-tags --quiet

# Delete the Artifact Registry repository
gcloud artifacts repositories delete sit737-5d --location=australia-southeast2 --quiet

# Verify deletion
gcloud artifacts repositories list --location=australia-southeast2
```

## Key Learnings

This project demonstrates:
1. Creating and configuring Docker images for Node.js applications
2. Working with Google Cloud Platform's Artifact Registry
3. Authenticating between local Docker environment and cloud services
4. Pushing and pulling Docker images to/from a cloud repository
5. Port mapping between container and host
6. Proper resource management and cleanup in cloud environments

## Complete Command Reference

```bash
# Google Cloud Authentication
gcloud auth login
gcloud config list
gcloud config get-value project
gcloud auth configure-docker australia-southeast2-docker.pkg.dev

# Docker Commands
docker tag week5img krishbm/week5img:latest
docker tag week5img australia-southeast2-docker.pkg.dev/sit737-25t1-moradiya-7db181d/sit737-5d/week5img:latest
docker images
docker push australia-southeast2-docker.pkg.dev/sit737-25t1-moradiya-7db181d/sit737-5d/week5img:latest
docker run -p 8080:3000 australia-southeast2-docker.pkg.dev/sit737-25t1-moradiya-7db181d/sit737-5d/week5img:latest

# Clean Up
gcloud artifacts docker images delete australia-southeast2-docker.pkg.dev/sit737-25t1-moradiya-7db181d/sit737-5d/week5img:latest --delete-tags --quiet
gcloud artifacts repositories delete sit737-5d --location=australia-southeast2 --quiet
gcloud artifacts repositories list --location=australia-southeast2
```

## Repository

Public GitHub repository URL: [https://github.com/KrishBM/sit737-2025-prac5d](https://github.com/KrishBM/sit737-2025-prac5d)

## Author

KrishBM
