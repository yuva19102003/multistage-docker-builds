---
# Multi-Stage Docker Builds for React.js with Node.js and Nginx

This repository serves as an example of leveraging multi-stage Docker builds to create a streamlined Docker image for deploying a React.js application. By employing multi-stage builds, we can utilize different base images for distinct phases of the build process, resulting in a more efficient and smaller final Docker image.

## Prerequisites

Before proceeding, ensure that you have Docker installed on your local machine. If not, you can find installation instructions for various platforms on the [official Docker website](https://docs.docker.com/get-docker/).

## Build and Run

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/yuva19102003/multistage-docker-builds.git
   cd  multistage-docker-builds
   ```

2. **Build the Docker Image:**
   ```bash
   docker build -t multistage_image -f dockerfile.prod .
   ```
   This command initiates the Docker build process, which involves multiple stages to optimize the resulting image.

3. **Run the Docker Container:**
   ```bash
   docker run -p 8080:80 multistage_image:latest
   ```
   The `-p 8080:80` flag maps port 80 of your host machine to port 80 of the Docker container. Adjust the port mapping as needed.

4. **View the Application:**
   Open your web browser and navigate to [http://localhost](http://localhost) to access your React.js application running inside the Docker container.

## Dockerfile Explanation

The `Dockerfile` included in this repository employs multi-stage builds to achieve a compact and efficient Docker image.

```Dockerfile
# Stage 1: Build React App
FROM node:latest as build
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Production Image
FROM nginx:latest
COPY --from=build /app/build /usr/share/nginx/html

```

- **Stage 1 (Build React App):** Utilizes the Node.js image to set up a build environment. It copies the necessary files, installs dependencies, and builds the React application.

- **Stage 2 (Production Image):** Uses the lightweight Nginx image to create the final production image. It copies the build artifacts from the previous stage into the Nginx image, making it suitable for deployment.

## Customize

Feel free to customize the `Dockerfile` and other configurations to align with the specific requirements of your project. You can adjust the base images, dependencies, and build scripts according to your needs.

---
