# External Communication from a Container

## Introduction

In this documentation, we'll showcase how to set up an Nginx container, connect it to a custom bridge network, and demonstrate external communication by pinging Google from within the container.

### Launching an Nginx Container

Launch the Nginx container named `nginx-container` as before:

```shell
docker run -d --name nginx-container nginx
```
- Description: Starts a detached Docker container named "nginx-container" running Nginx using the official image from Docker Hub.

### Accessing the Nginx Container

Access the terminal of the Nginx container:

```shell
docker exec -it nginx-container sh
```

### Installing Networking Tools

Install the necessary networking tools, including `ping`, within the container:

```shell
apt-get update
apt-get install iputils-ping -y
```

This will update the package lists and install the `ping` tool.

### Pinging Google

Now that we have `ping` installed, let's ping Google:

```shell
ping google.com -c 5
```

This command will send 5 ICMP echo requests to Google's servers and display the responses, confirming external connectivity from within the container.

## Conclusion

By following these steps, you've successfully demonstrated external communication from an Nginx container by pinging Google. The installation of networking tools within the container allowed us to use the `ping` command to verify external connectivity. This highlights the flexibility of Docker containers in adapting to different networking requirements.