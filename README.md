# Docker
Docker is an open-source platform that automates the deployment, scaling, and management of applications in lightweight containers. These containers package an application with all its dependencies, libraries, and configurations, ensuring that it runs consistently across different environments.

- These `container` is acts as light weight servers.
- `Creation` and `Deletion` of containers is quite easy.
- We can create as many containers for our application.

## What Problems Does Docker Solve?

1. **Consistency Across Environments**:
   - **Challenge**: Applications often behave differently in development, testing, and production environments due to variations in system configurations and dependencies.
   - **Solution**: Docker containers encapsulate everything needed to run an application, ensuring consistent behavior regardless of the environment.

2. **Dependency Management**:
   - **Challenge**: Managing different versions of libraries and dependencies can be complex and lead to conflicts.
   - **Solution**: Docker allows you to package all dependencies within a container, eliminating version conflicts and dependency issues.

3. **Isolation**:
   - **Challenge**: Running multiple applications on the same host can lead to conflicts and resource contention.
   - **Solution**: Docker containers provide isolated environments, allowing multiple applications to run on the same host without interfering with each other.

4. **Scalability and Efficiency**:
   - **Challenge**: Scaling applications and managing resources efficiently can be complex.
   - **Solution**: Docker containers are lightweight and can be quickly scaled up or down, allowing efficient resource utilization and simplified scaling.

5. **Portability**:
   - **Challenge**: Moving applications between different environments or cloud providers can be difficult.
   - **Solution**: Docker containers can run anywhere, from a developer's laptop to on-premises servers to cloud environments, making applications highly portable.

6. **Faster Development and Deployment**:
   - **Challenge**: Traditional development and deployment processes can be slow and error-prone.
   - **Solution**: Docker enables faster development cycles by allowing developers to quickly build, test, and deploy applications in consistent environments.

### Key Docker Components:
- **Docker Engine**: The core component that runs and manages containers.
- **Docker Hub**: A repository for Docker images where you can find and share containerized applications.
- **Docker Compose**: A tool for defining and running multi-container Docker applications.

### Example Use Case:
Imagine you're developing a web application that relies on a specific version of Node.js and a database. With Docker, you can create a container that includes Node.js, your application code, and all required dependencies. This container can then be run on any system with Docker installed, ensuring that your application behaves the same everywhere.

---

## Docker Installation
- Visit docker's official site for installation of `CLI` and `Docker Desktop`

- [Click To Download](https://docs.docker.com/desktop/setup/install/linux/fedora/)

---

## Creating Images in Docker's Container
To create a image of any specific OS in your container run the command given below:
```shell
docker run -it ubuntu
``` 
The Above commands will: 
1. Do check for OS Image, if it is present then in that it will use that one.
2. Otherwise it will download the image from `hub.docker.com`.

---
## Basic commands of Dockers
1. To Check ***running Container***
```shell
docker container ls
``` 
2. To Check ***all Containers*** including paused ones:
```shell
docker container ls -a
``` 
3. To Start ***closed Container***:
```shell
docker start containerName
``` 
4. To close ***a open Container***:
```shell
docker close containerName
``` 
3. To Start a ***closed Container*** in terminal:
```shell
docker exec -it containerID /bin/bash
``` 

---
## Dockers Image and Containers
In Docker, **images** and **containers** are fundamental concepts that work together to create and run applications in a consistent, isolated environment. Let me explain both:

### Docker Image:
- **Definition**: A Docker image is a lightweight, stand-alone, and executable software package that includes everything needed to run a piece of software: code, runtime, libraries, environment variables, and configuration files.
- **Read-Only**: Images are read-only templates used to create containers. They are built using a Dockerfile, which contains a set of instructions on how to build the image.
- **Layers**: Docker images are composed of layers, each representing an instruction in the Dockerfile. Layers are cached and reused to make image creation faster and more efficient.
- **Registry**: Images can be stored and shared through registries like Docker Hub or private repositories.

### Example of a Dockerfile (used to create an image):
```Dockerfile
# Use an official Node.js runtime as a parent image
FROM node:14

# Set the working directory
WORKDIR /app

# Copy the local files to the working directory
COPY . /app

# Install dependencies
RUN npm install

# Expose the application port
EXPOSE 3000

# Define the command to run the application
CMD ["npm", "start"]
```

### Docker Container:
- **Definition**: A Docker container is a runtime instance of a Docker image. It includes everything that was defined in the image and an additional writable layer where changes can be made.
- **Isolation**: Containers provide isolated environments for running applications. Each container has its own filesystem, CPU, memory, process space, and network interfaces.
- **Stateful**: Unlike images, containers are stateful. You can start, stop, move, and delete them. Containers are ephemeral, meaning they can be created and destroyed without affecting the underlying image.
- **Interaction**: Containers can be managed using Docker commands or Docker Compose for multi-container applications.

### Example of Running a Container:
```shell
# Pull the image from Docker Hub
docker pull node:14

# Run a container from the image
docker run -d -p 3000:3000 --name my-node-app node:14
```

### Summary:
- **Images**: Templates or blueprints for creating containers. They are static and read-only.
- **Containers**: Instances of images that run applications. They are dynamic, stateful, and provide an isolated environment.

---
### Command to mapping local port with docker port
```bash
docker run -it -p 8080:3000 --name my-new-node-container node /bin/bash
```
### Command to to pass enviroment variable
```bash
docker run -it -p 8080:3000 -e key=vale -e key=value --name my-new-node-container node /bin/bash
```

---

## Dockerization of Node.js application
Dockerizing a Node.js application involves creating a Docker image for your application, which can then be run in a container. Here's a step-by-step guide to help you get started:

### Step 1: Create a Node.js Application
If you don't already have a Node.js application, you can create a simple one. For example, create a `server.js` file with the following content:

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello, Docker!');
});

app.listen(port, () => {
  console.log(`App running at http://localhost:${port}`);
});
```

### Step 2: Initialize a `package.json` File
Run the following command to create a `package.json` file:

```bash
npm init -y
```

Then install Express.js:

```bash
npm install express
```

### Step 3: Create a Dockerfile
Create a file named `Dockerfile` in the root directory of your project with the following content:

```Dockerfile
# Use the official Node.js image as a base image
FROM node:14

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code to the working directory
COPY . .

# Expose the application port
EXPOSE 3000

# Define the command to run the application
CMD ["node", "server.js"]
```

### Step 4: Create a `.dockerignore` File
Create a `.dockerignore` file to specify which files and directories to ignore when building the image. This helps reduce the image size:

```
node_modules
npm-debug.log
```

### Step 5: Build the Docker Image
Open your terminal and navigate to the project directory. Run the following command to build the Docker image:

```bash
docker build -t my-node-app .
```

### Step 6: Run the Docker Container
Run a container from the Docker image and map the ports:

```bash
docker run -d -p 8080:3000 --name my-node-app-container my-node-app
```

### Step 7: Access the Application
Open your web browser and navigate to `http://localhost:8080`. You should see the message "Hello, Docker!".

### Summary
By following these steps, you've successfully dockerized your Node.js application. Your application is now running inside a Docker container, which ensures consistency across different environments.

---
## Docker Compose
Docker Compose is a tool that simplifies the process of defining and running multi-container Docker applications. With Docker Compose, you can use a single YAML file to configure your application's services, networks, and volumes. This makes it easier to manage and deploy complex applications.

### Getting Started with Docker Compose:

### Step 1: Install Docker Compose
Ensure you have Docker Compose installed. If not, follow the [official installation guide](https://docs.docker.com/compose/install/) for your operating system.

### Step 2: Create a `docker-compose.yml` File
In the root directory of your project, create a `docker-compose.yml` file to define your services.

### Example `docker-compose.yml` File:
Let's create a simple setup for a Node.js application and a MongoDB database.

```yaml
version: '3.8'

services:
  web:
    image: node:14
    working_dir: /app
    volumes:
      - .:/app
    ports:
      - "8080:3000"
    command: sh -c "npm install && npm start"

  db:
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

### Explanation:
- **`version`**: Specifies the Compose file format version.
- **`services`**: Defines the individual services (containers) that make up your application.
  - **`web`**: The Node.js application service.
    - **`image`**: Specifies the Docker image to use.
    - **`working_dir`**: Sets the working directory inside the container.
    - **`volumes`**: Mounts the current directory to the `/app` directory inside the container.
    - **`ports`**: Maps port 8080 on the host to port 3000 inside the container.
    - **`command`**: Runs the specified command to start the application.
  - **`db`**: The MongoDB service.
    - **`image`**: Specifies the MongoDB image to use.
    - **`ports`**: Maps port 27017 on the host to port 27017 inside the container.
    - **`volumes`**: Uses a named volume to persist MongoDB data.
- **`volumes`**: Defines named volumes to persist data.

### Step 3: Run Docker Compose
Navigate to the directory containing your `docker-compose.yml` file and run the following command to start your services:

```bash
docker-compose up
```

This will build and start the defined services. You can access your Node.js application at `http://localhost:8080`.

### Managing Docker Compose:
- **Start Services**: `docker-compose up`
- **Stop Services**: `docker-compose down`
- **Build or Rebuild Services**: `docker-compose build`
- **View Logs**: `docker-compose logs`
- **Run a One-off Command**: `docker-compose run <service_name> <command>`

### Example:
To stop the services, you can use:
```bash
docker-compose down
```

Docker Compose is powerful and simplifies managing multi-container Docker applications, making it easier to develop, test, and deploy your applications.

---
## Docker Network
Docker networks enable containers to communicate with each other and with the host system. They provide isolation and security while facilitating connectivity. Here are some key concepts and commands related to Docker networking:

### Types of Docker Networks:

1. **Bridge Network**:
   - Default network type.
   - Containers on the same bridge network can communicate with each other.
   - Suitable for standalone applications.

2. **Host Network**:
   - Shares the host’s networking namespace.
   - Containers use the host’s IP address and port space.
   - Suitable for performance-sensitive applications.

3. **Overlay Network**:
   - Used for multi-host Docker setups, like Docker Swarm.
   - Enables communication between containers on different Docker hosts.

4. **None Network**:
   - No network interfaces are attached to the container.
   - Suitable for cases where network isolation is required.

5. **Custom User-Defined Bridge Network**:
   - Similar to the default bridge network but provides better network isolation and control over container names and IP addresses.

### Common Docker Network Commands:

#### 1. **List Networks**:
   ```bash
   docker network ls
   ```

#### 2. **Inspect a Network**:
   ```bash
   docker network inspect <network_name>
   ```
   Example:
   ```bash
   docker network inspect bridge
   ```

#### 3. **Create a Custom Bridge Network**:
   ```bash
   docker network create <network_name>
   ```
   Example:
   ```bash
   docker network create my-bridge-network
   ```

#### 4. **Run a Container in a Custom Network**:
   ```bash
   docker run -d --name <container_name> --network <network_name> <image_name>
   ```
   Example:
   ```bash
   docker run -d --name my-container --network my-bridge-network nginx
   ```

#### 5. **Connect a Running Container to a Network**:
   ```bash
   docker network connect <network_name> <container_name>
   ```
   Example:
   ```bash
   docker network connect my-bridge-network my-container
   ```

#### 6. **Disconnect a Container from a Network**:
   ```bash
   docker network disconnect <network_name> <container_name>
   ```
   Example:
   ```bash
   docker network disconnect my-bridge-network my-container
   ```

### Example Workflow:
1. **Create a Custom Network**:
   ```bash
   docker network create my-bridge-network
   ```

2. **Run Containers in the Custom Network**:
   ```bash
   docker run -d --name container1 --network my-bridge-network nginx
   docker run -d --name container2 --network my-bridge-network nginx
   ```

3. **Verify Network Connectivity**:
   ```bash
   docker exec -it container1 ping container2
   ```

Using Docker networks allows you to configure and manage container communications in a flexible and secure manner.

---

## Docker Volumes
In Docker, **volumes** are used to persist and share data between containers and the host machine. They provide a way to store data independently of the container's lifecycle, ensuring that data isn't lost when containers are stopped, removed, or recreated.

### Types of Docker Volumes:

1. **Named Volumes**:
   - Created and managed by Docker.
   - Stored in a location within Docker's storage area.
   - Can be reused across multiple containers.
   - Example: `my-volume`

2. **Bind Mounts**:
   - Bind a specific path on the host to a specific path in the container.
   - Gives more control over the location on the host machine.
   - Suitable for scenarios where you need access to the host's filesystem.
   - Example: `/path/on/host:/path/in/container`

### Managing Docker Volumes:

#### 1. **Create a Named Volume**:
   ```bash
   docker volume create my-volume
   ```

#### 2. **List Volumes**:
   ```bash
   docker volume ls
   ```

#### 3. **Inspect a Volume**:
   ```bash
   docker volume inspect my-volume
   ```

#### 4. **Remove a Volume**:
   ```bash
   docker volume rm my-volume
   ```

### Using Volumes in Containers:

#### 1. **Using Named Volumes**:
When you create a container, you can specify a named volume using the `-v` flag:

```bash
docker run -d -v my-volume:/data --name my-container nginx
```
In this example, `my-volume` is mounted to the `/data` directory inside the container.

#### 2. **Using Bind Mounts**:
You can also use bind mounts to map a directory from the host to the container:

```bash
docker run -d -v /path/on/host:/path/in/container --name my-container nginx
```
In this example, `/path/on/host` is mapped to `/path/in/container`.

### Example with Docker Compose:
You can also define volumes in a `docker-compose.yml` file:

```yaml
version: '3.8'

services:
  web:
    image: nginx
    volumes:
      - my-volume:/data

volumes:
  my-volume:
```

### Summary:
- **Volumes**: Persist data independently of container lifecycle.
- **Named Volumes**: Managed by Docker, reusable across containers.
- **Bind Mounts**: Specific path on the host bound to a container path.

Volumes are crucial for scenarios where data persistence is necessary, such as databases, user uploads, and configuration files.

---

## Efficient caching layers
It refer to strategies used to optimize the caching mechanism in various systems, such as web servers, databases, and containerized applications. The goal is to reduce latency, improve performance, and minimize resource usage by storing frequently accessed data in a cache.

### Key Concepts of Efficient Caching Layers:

1. **Hierarchical Caching**:
   - **Multi-layered caching** involves using multiple levels of cache (e.g., in-memory cache, disk cache, and content delivery network (CDN)). This hierarchical structure helps reduce latency by storing data closer to the user.

2. **Cache Invalidation**:
   - **Minimize cache invalidation** by organizing instructions or data in a way that reduces the need to refresh the cache. For example, in Docker, place frequently changing instructions at the end of the Dockerfile to maximize layer reuse.

3. **Specific Commands**:
   - **Be explicit with commands** to prevent unnecessary cache invalidation. Specify exact versions for package installations and avoid using commands that can lead to cache misses.

4. **Layer Reuse**:
   - **Leverage layer reuse** in containerized applications. When building Docker images, Docker caches the newly created layers. If the instruction hasn't changed since the last build, Docker will reuse the existing layer, reducing build time and minimizing bandwidth usage.

5. **Content Delivery Networks (CDNs)**:
   - **Use CDNs** to cache content closer to end-users, reducing the load on origin servers and improving response times.

### Example: Efficient Docker Layer Caching
When building Docker images, you can optimize layer caching by organizing your Dockerfile efficiently. For instance:

```Dockerfile
# Use a specific version of the base image
FROM node:14

# Copy package.json and package-lock.json first (frequently changed)
COPY package*.json ./

# Install dependencies (less frequently changed)
RUN npm install

# Copy the rest of the application code (most frequently changed)
COPY . .

# Expose the application port
EXPOSE 3000

# Define the command to run the application
CMD ["node", "server.js"]
```

By copying the `package.json` and `package-lock.json` files first and installing dependencies before copying the rest of the application code, you maximize the reuse of cached layers.

### Summary
Efficient caching layers involve strategies to optimize data storage and retrieval, reduce latency, and improve performance by leveraging hierarchical caching, minimizing cache invalidation, and reusing layers where possible.

---

## Multi stage build
Multi-stage builds are a powerful feature in Docker that allow you to create smaller and more efficient Docker images. By breaking the build process into multiple stages, you can selectively copy only the necessary artifacts from one stage to another, reducing the final image size and improving security.

### Benefits of Multi-Stage Builds:
- **Smaller Image Size**: By discarding unnecessary files and dependencies from intermediate stages, you create a leaner final image.
- **Security**: Minimizes the attack surface by including only the essential components in the final image.
- **Efficiency**: Streamlines the build process by reusing intermediate stages and caching layers.

### Example: Multi-Stage Build for a Node.js Application

Here’s an example of a multi-stage build for a Node.js application:

#### Dockerfile:

```Dockerfile
# Stage 1: Build Stage
FROM node:14 as build

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the application (if applicable, e.g., for front-end apps)
RUN npm run build

# Stage 2: Production Stage
FROM node:14-alpine

# Set the working directory
WORKDIR /app

# Copy only the necessary files from the build stage
COPY --from=build /app/package*.json ./
COPY --from=build /app/node_modules ./node_modules
COPY --from=build /app/dist ./dist

# Expose the application port
EXPOSE 3000

# Define the command to run the application
CMD ["node", "dist/server.js"]
```

### Explanation:
1. **Build Stage**:
   - The first stage uses the `node:14` image to install dependencies and build the application.
   - This stage includes all development dependencies and the source code.
   - The build output (e.g., `/app/dist` for a front-end application) is generated here.

2. **Production Stage**:
   - The second stage uses a smaller, lighter image (`node:14-alpine`) to create the final production image.
   - Only the necessary files from the build stage are copied to this stage.
   - The resulting image is much smaller because it contains only the built application and its runtime dependencies.

### Building and Running the Image:
1. **Build the Image**:
   ```bash
   docker build -t my-node-app .
   ```

2. **Run the Container**:
   ```bash
   docker run -d -p 8080:3000 --name my-node-app-container my-node-app
   ```

By using multi-stage builds, you can create optimized Docker images that are smaller, more secure, and faster to build. This is particularly useful for complex applications with many dependencies and build steps.
