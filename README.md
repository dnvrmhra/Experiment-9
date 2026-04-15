# Experiment 9: Dockerization and CI/CD Pipeline for React Application

---------------------------------------------------------------------

## Tech Stack

* Frontend: React (v18)
* Containerization: Docker
* Web Server: Nginx (Alpine)
* CI/CD: GitHub Actions
* Registry: Docker Hub

----------------------------------------------------------------------

## Project Structure

```
react-app/
│
├── .github/
│   └── workflows/
│       └── ci-cd.yml
│
├── public/
├── src/
├── Dockerfile
├── .dockerignore
├── package.json
└── README.md
```

----------------------

## Dockerization

### Multi-Stage Docker Build

The application uses a multi-stage Docker build process:

1. Build Stage

   * Uses Node.js to install dependencies and build the React application

2. Production Stage

   * Uses Nginx to serve the optimized static build

### Dockerfile

```
# Stage 1: Build React app
FROM node:18 AS build

WORKDIR /app
COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# Stage 2: Serve using Nginx
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## Running Locally with Docker

### Build Image

```
docker build -t dnvrmhra/react-app .
```

### Run Container

```
docker run -p 8080:80 dnvrmhra/react-app
```

Access the application at:

```
http://localhost:8080
```

---

## CI/CD Pipeline

### Workflow Description

The CI/CD pipeline is configured using GitHub Actions and is triggered on every push to the main branch.

### Pipeline Steps

1. Checkout source code
2. Login to Docker Hub using secrets
3. Build Docker image
4. Tag image with latest and commit SHA
5. Push image to Docker Hub

### Workflow File Location

```
.github/workflows/ci-cd.yml
```

---

## GitHub Secrets Configuration

The following secrets are required:

* DOCKER_USERNAME
* DOCKER_PASSWORD

These are used for authenticating with Docker Hub during the pipeline execution.

---

## Expected Output

* Docker image successfully built and pushed
* Image available on Docker Hub
* Tags include:

  * latest
  * commit SHA
* GitHub Actions workflow completes successfully

---

## Conclusion

This project successfully demonstrates end-to-end containerization and CI/CD automation for a React application. It ensures efficient deployment, version control of images, and streamlined development workflow using industry-standard tools.

---

## Name: Daanveer Mehra
## UID: 24BAI70059
## Section: 24AML-2(A)
