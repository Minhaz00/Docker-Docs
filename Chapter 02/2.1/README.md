# Deploying a Monitored NGINX Web Server Using Docker

### Introduction

In this example, you will learn how to use Docker to install and manage a web server using NGINX, set up a monitoring system, and configure alert notifications. By following these instructions, you'll get hands-on experience with Docker's features, such as creating detached and interactive containers, managing container logs, and handling container lifecycle operations.

### Scenario Overview

You have a client who wants a new website that requires close monitoring. They have requested the use of NGINX for the web server and want to receive email notifications when the server goes down. The architecture will consist of three containers:
1. **Web Container**: Runs the NGINX web server.
2. **Mailer Container**: Sends email notifications.
3. **Agent Container**: Monitors the web server and triggers the mailer when the server is down.

### Objectives

- Create and manage detached and interactive containers.
- List running containers.
- View and manage container logs.
- Stop and restart containers.
- Reattach and detach from an interactive container.

### Creating and Starting Containers

#### Step 1: Start NGINX Container

Download, install, and start an NGINX container in detached mode:

```bash
docker run --detach --name web nginx:latest
```

This command:
- Downloads the latest NGINX image from Docker Hub.
- Creates and starts a container named `web` in detached mode.

#### Step 2: Start Mailer Container

Create and start a mailer container in detached mode:

```bash
docker run -d --name mailer mailer_image:latest
```

This command:
- Downloads and starts a container named `mailer` using the specified mailer image.

### Running Interactive Containers

#### Step 3: Start an Interactive Container for Testing

Run an interactive container linked to the `web` container to verify the web server:

```bash
docker run --interactive --tty --link web:web --name web_test busybox:latest /bin/sh
```

This command:
- Creates and starts a container named `web_test` with an interactive shell.
- Links the container to the `web` container, allowing it to access the NGINX server.

Inside the interactive shell, run:

```bash
wget -O - http://web:80/
```

You should see "Welcome to NGINX!" if the web server is running correctly. Exit the shell by typing `exit`.

### Monitoring and Notifications

#### Step 4: Start the Agent Container

Run the agent container in interactive mode:

```bash
docker run -it --name agent --link web:insideweb --link mailer:insidemailer dockerinaction/ch2_agent
```

This container will:
- Monitor the NGINX server.
- Print "System up." if the server is running.
- Trigger the mailer to send an email if the server goes down.

Detach from the interactive container by pressing `Ctrl + P` followed by `Ctrl + Q`.

### Managing Containers

#### Step 5: List Running Containers

Check which containers are running:

```bash
docker ps
```

This command lists details such as container ID, image used, command executed, uptime, and container names.

#### Step 6: Restart Containers

If any container is not running, restart it:

```bash
docker restart web
docker restart mailer
docker restart agent
```

#### Step 7: View Container Logs

Examine logs to ensure everything is running correctly:

```bash
docker logs web
docker logs mailer
docker logs agent
```

- **Web Logs**: Look for "GET / HTTP/1.0" 200 to confirm the agent is testing the web server.
- **Mailer Logs**: Ensure the mailer has started.
- **Agent Logs**: Confirm "System up." messages indicating the server is running.

#### Step 8: Follow Logs

To continuously monitor logs, use the `--follow` flag:

```bash
docker logs -f agent
```

Press `Ctrl + C` to stop following the logs.

#### Step 9: Test the System

Stop the web container to test the monitoring system:

```bash
docker stop web
```

Check the mailer logs to see if it recorded the service down event and triggered an email notification:

```bash
docker logs mailer
```

Look for a line like:

```
Sending email: To: admin@work Message: The service is down!
```

### Conclusion

You have successfully set up a Docker-based system with an NGINX web server, a mailer for notifications, and an agent for monitoring. You learned how to create and manage both detached and interactive containers, view logs, and handle container lifecycle operations. This foundational knowledge will be invaluable as you build more complex systems using Docker.