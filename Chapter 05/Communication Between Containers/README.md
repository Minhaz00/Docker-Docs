# Communication Between Containers in a Custom Bridge Network

In this demonstration, we'll showcase how to create and connect three containers on a user-defined bridge network , allowing them to communicate with each other seamlessly.

## Creating the User-Defined Bridge Network

Let's start by creating the bridge network:

```shell
docker network create --driver bridge my-bridge-network
```

This command sets up a user-defined bridge network named `my-bridge-network`.

## Launching Containers and Connecting to the Network

Next, we'll launch three containers named `container1`, `container2`, and `container3`, and connect them to the `my-bridge-network` we created.

### Launching Container 1

```shell
docker run -d --name container1 --network=my-bridge-network nginx
```

### Launching Container 2

```shell
docker run -d --name container2 --network=my-bridge-network nginx
```

### Launching Container 3

```shell
docker run -d --name container3 --network=my-bridge-network nginx
```

## Verifying Communication

### Container Shell Access

Access the shell of any container:

```shell
docker exec -it container1 /bin/bash
```

Once inside the container, we can ping the other containers by their names or IP addresses:

```shell
ping container2
ping container3
```

## Conclusion

By following these steps, we can create a user-defined bridge network in Docker and demonstrate successful communication between multiple containers connected to the same network. This setup showcases the flexibility and efficiency of Docker networking, enabling seamless interaction between containerized applications.