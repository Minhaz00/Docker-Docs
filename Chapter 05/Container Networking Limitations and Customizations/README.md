# Container Networking Limitations and Customizations

## Introduction

Networking is crucial for various applications, but Docker containers come with some limitations and customization options. Here, we'll cover key topics to understand when working with containers for networked applications.

- **Network Policies**: Docker container networks lack access control or firewall mechanisms, allowing unrestricted communication between containers. Despite the namespace model's intention to address access control, application-level authentication and authorization are necessary to protect containers from each other on the same network. This design decision affects service dependencies and deployment strategies, requiring careful consideration of security measures.

- **Custom DNS Configuration**: Vital for hostname resolution within container networks, enabling enhanced communication despite private IP addresses.

## Custom DNS Configuration

Customizing DNS configuration within containers is essential for hostname resolution and network communication. Docker offers several options for customizing DNS settings:

### Setting Container Hostname

Using the `--hostname` flag in the `docker run` command allows setting the container's hostname, facilitating internal hostname resolution.


```shell
docker run --rm --hostname barker alpine:3.8 nslookup barker
```

 - **Description**: Runs an Alpine container with the hostname "barker" and performs a DNS lookup for "barker".

### Specifying DNS Servers

Specify DNS servers for a container using the `--dns` flag.

#### Description:

Set the primary DNS server for the container to Google's public DNS service (8.8.8.8) and perform a DNS lookup for "docker.com".

```shell
docker run --rm --dns 8.8.8.8 alpine:3.8 nslookup docker.com
```

 - **Description**: Spins up a temporary Alpine container with DNS server set to 8.8.8.8, then performs a DNS lookup for "docker.com".

### Setting DNS Search Domain

Set the DNS search domain to "docker.com" and perform a hostname lookup for "hub".

```shell
docker run --rm --dns-search docker.com alpine:3.8 nslookup hub
```

 - **Description**: Runs Alpine container with DNS search set to "docker.com", performs DNS lookup for "hub" using the search domain, displays the IP

### Overriding DNS Entries

Override DNS entries using the `--add-host` flag.

```shell
docker run --rm --add-host docker.com:127.0.0.1 alpine:3.8 nslookup docker.com
```

 - **Description**: Add a custom host entry mapping "docker.com" to the loopback address (127.0.0.1) and perform a DNS lookup for "docker.com".

## Demonstration

Let's demonstrate custom DNS configuration within a container:

1. **Set Container Hostname**:

   ```shell
   docker run --rm --hostname mycontainer alpine:3.8 cat /etc/hostname
   ```

2. **Specify DNS Server**:

   ```shell
   docker run --rm --dns 8.8.8.8 alpine:3.8 nslookup docker.com
   ```

3. **Set DNS Search Domain**:

   ```shell
   docker run --rm --dns-search docker.com alpine:3.8 nslookup hub
   ```

4. **Override DNS Entry**:

   ```shell
   docker run --rm --add-host docker.com:127.0.0.1 alpine:3.8 nslookup docker.com
   ```

## Conclusion

Understanding container networking caveats and customization options is crucial for deploying networked applications effectively. By leveraging custom DNS configurations, you can optimize hostname resolution and network communication within Docker containers.