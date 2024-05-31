## Documentation: Creating a Custom Docker Image for a Simple Node.js Application and Running a Container

### Prerequisites
- Docker installed on your machine.
- Basic knowledge of Node.js and Docker.

### Step 1: Set Up Your Node.js Application
1. Create a new directory for your project and navigate into it:
    ```sh
    mkdir my-node-app
    cd my-node-app
    ```
2. Initialize a new Node.js project:
    ```sh
    npm init -y
    ```
3. Create a simple Node.js application. Create a file named `app.js` and add the following code:
    ```javascript
    const http = require('http');

    const hostname = '0.0.0.0';
    const port = 3000;

    const server = http.createServer((req, res) => {
        res.statusCode = 200;
        res.setHeader('Content-Type', 'text/plain');
        res.end('Hello, World!\n');
    });

    server.listen(port, hostname, () => {
        console.log(`Server running at http://${hostname}:${port}/`);
    });
    ```
4. Add the `express` dependency (optional but commonly used for Node.js apps):
    ```sh
    npm install express
    ```

### Step 2: Create a Dockerfile
1. In the root of your project directory, create a file named `Dockerfile` and add the following content:
    ```Dockerfile
    # Use an official Node.js runtime as the base image
    FROM node:14

    # Set the working directory inside the container
    WORKDIR /usr/src/app

    # Copy package.json and package-lock.json to the working directory
    COPY package*.json ./

    # Install the dependencies
    RUN npm install

    # Copy the rest of the application code to the working directory
    COPY . .

    # Expose the port the app runs on
    EXPOSE 3000

    # Define the command to run the application
    CMD ["node", "app.js"]
    ```

### Step 3: Build the Docker Image
1. Build your Docker image using the Dockerfile:
    ```sh
    docker build -t my-node-app .
    ```
    This command builds the Docker image and tags it as `my-node-app`.

### Step 4: Run a Container from the Built Image
1. Run a container from your Docker image:
    ```sh
    docker run -p 3000:3000 my-node-app
    ```
    This command runs a container from the `my-node-app` image, mapping port 3000 of your host to port 3000 of the container.

### Step 5: Access the Application
1. Open your web browser and navigate to `http://localhost:3000`. You should see `Hello, World!`.

### Conclusion
You have successfully created a custom Docker image for a simple Node.js application and run a container from the built image. This process involves setting up a Node.js application, creating a Dockerfile, building the Docker image, and running the container.

### Additional Tips
- To stop the running container, press `Ctrl+C` in the terminal where the container is running.
- To run the container in detached mode, add the `-d` flag:
    ```sh
    docker run -d -p 3000:3000 my-node-app
    ```
- To view running containers, use:
    ```sh
    docker ps
    ```
- To stop a specific container, use:
    ```sh
    docker stop <container_id>
    ```
- To remove a specific container, use:
    ```sh
    docker rm <container_id>
    ```