# Docker Container Restart Policies

It's often a good idea to run containers with a restart policy. This is a basic form of self-healing that allows the local Docker engine to automatically restart failed containers.

Restart policies are applied per container. They can be configured imperatively on the command line as part of `docker run` commands or declaratively in YAML files for use with higher-level tools such as Docker Swarm, Docker Compose, and Kubernetes.

At the time of writing, the following restart policies exist:

- **always**
- **unless-stopped**
- **on-failure**

## Restart Policies Explained

### always

The `always` policy is the simplest. It always restarts a failed container unless it’s been explicitly stopped. 

#### Demonstration

1. Start a new interactive container with the `--restart always` policy and run a shell process:
   ```sh
   $ docker run --name neversaydie -it --restart always alpine sh
   /#
   ```

2. Type `exit` from the shell to kill the container's PID 1 process and stop the container. Docker will automatically restart it because it has the `--restart always` policy.

3. Check the container’s status:
   ```sh
   $ docker ps
   CONTAINER ID    IMAGE     COMMAND    CREATED           STATUS         NAME
   0901afb84439    alpine    "sh"       35 seconds ago    Up 9 seconds   neversaydie
   ```
   The container was created 35 seconds ago but has only been up for 9 seconds. This is because the exit command killed it, and Docker restarted it.

Be aware that Docker has restarted the same container and not created a new one. If you inspect it with `docker inspect`, you can see the `restartCount` has been incremented.

An interesting feature of the `--restart always` policy is that if you stop a container with `docker stop` and then restart the Docker daemon, the container will be restarted. 

To illustrate:

1. Start a new container with the `--restart always` policy and intentionally stop it with the `docker stop` command.
2. The container will be in the Stopped (Exited) state.
3. Restart the Docker daemon, and the container will be automatically restarted when the daemon comes back up.

### unless-stopped

The main difference between the `always` and `unless-stopped` policies is that containers with the `--restart unless-stopped` policy will not be restarted when the daemon restarts if they were in the Stopped (Exited) state.

#### Example

1. Create two new containers:
   ```sh
   $ docker run -d --name always --restart always alpine sleep 1d
   $ docker run -d --name unless-stopped --restart unless-stopped alpine sleep 1d
   ```

2. Verify both containers are running:
   ```sh
   $ docker ps
   CONTAINER ID   IMAGE     COMMAND       STATUS       NAMES
   3142bd91ecc4   alpine    "sleep 1d"    Up 2 secs    unless-stopped
   4f1b431ac729   alpine    "sleep 1d"    Up 17 secs   always
   ```

3. Stop both containers:
   ```sh
   $ docker stop always unless-stopped
   $ docker ps -a
   CONTAINER ID   IMAGE     STATUS                        NAMES
   3142bd91ecc4   alpine    Exited (137) 3 seconds ago    unless-stopped
   4f1b431ac729   alpine    Exited (137) 3 seconds ago    always
   ```

4. Restart Docker:
   ```sh
   $ systemctl restart docker
   ```

5. Check the status of the containers:
   ```sh
   $ docker ps -a
   CONTAINER   CREATED             STATUS                       NAMES
   314..cc4    2 minutes ago      Exited (137) 2 minutes ago    unless-stopped
   4f1..729    2 minutes ago      Up 9 seconds                  always
   ```

The `always` container has been restarted, but the `unless-stopped` container has not.

### on-failure

The `on-failure` policy will restart a container if it exits with a non-zero exit code. It will also restart containers when the Docker daemon restarts, even those that were in the stopped state.
