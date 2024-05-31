### Lab: Differentiating Docker `stop` vs `kill`

#### Objective:
Understand the differences between the Docker `stop` and `kill` commands by observing their behavior on a running container.

#### Prerequisites:
- Docker installed on your machine
- Basic knowledge of Docker commands

#### Lab Setup:
1. **Start a Docker Container**: We will use a simple container that runs indefinitely. For this lab, we'll use the `busybox` image, which is lightweight and suitable for demonstration purposes.

```bash
docker run -d --name test_container busybox sh -c "while true; do echo Hello World; sleep 1; done"
```

This command does the following:
- `docker run -d` runs the container in detached mode.
- `--name test_container` names the container `test_container`.
- `busybox sh -c "while true; do echo Hello World; sleep 1; done"` runs an infinite loop inside the container, printing "Hello World" every second.

2. **Verify the Container is Running**:

```bash
docker ps
```

You should see `test_container` listed as running.

#### Part 1: Using `docker stop`

1. **Send a Stop Signal**:

```bash
docker stop test_container
```

2. **Observe the Behavior**:
   - The `stop` command sends a `SIGTERM` signal to the main process inside the container, allowing it to shut down gracefully.
   - Docker waits for 10 seconds by default for the process to terminate. If the process does not stop within this period, Docker sends a `SIGKILL` signal to forcefully terminate it.

3. **Verify the Container Status**:

```bash
docker ps -a
```

- The container should be in the `Exited` state.
- To see the logs and confirm it stopped gracefully, you can inspect the logs:

```bash
docker logs test_container
```

#### Part 2: Using `docker kill`

1. **Restart the Container**:

```bash
docker start test_container
```

2. **Send a Kill Signal**:

```bash
docker kill test_container
```

3. **Observe the Behavior**:
   - The `kill` command sends a `SIGKILL` signal to the main process inside the container immediately, which does not allow for any graceful shutdown.
   - This results in the immediate termination of the process.

4. **Verify the Container Status**:

```bash
docker ps -a
```

- The container should again be in the `Exited` state, but this time it was terminated forcefully.

5. **Check the Logs**:
   - Since the container was killed immediately, the logs will not show any shutdown process:

```bash
docker logs test_container
```

#### Conclusion:
- **`docker stop`** allows for a graceful shutdown by sending `SIGTERM` followed by `SIGKILL` after a timeout.
- **`docker kill`** immediately terminates the container by sending `SIGKILL`.

#### Cleanup:
Remove the test container to clean up your environment:

```bash
docker rm test_container
```

#### Notes:
- Adjust the timeout for the `stop` command if needed:

```bash
docker stop -t 5 test_container
```

This command sets a 5-second timeout before sending the `SIGKILL` signal.

This lab helps you understand the practical differences between stopping and killing a container, which is crucial for managing Docker containers effectively.