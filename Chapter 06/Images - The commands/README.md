Images - The commands
docker pull is the command to download images from remote registries. By default, images will be pulled from Docker Hub but you can pull from other registries. This command will pull the image tagged as latest from the alpine repository on Docker Hub: docker pull alpine:latest.
docker images lists all of the images stored in your Docker host’s local image cache. Add the --digests flag to see the SHA256 digests.
docker inspect is a thing of beauty! It gives you all of the glorious details of an image — layer data and metadata.
docker manifest inspect lets you to inspect the manifest list of any image stored on Docker Hub. This command will show the manifest list for the redis image: docker manifest inspect redis.
docker buildx is a Docker CLI plugin that extends the Docker CLI to support multi-arch builds.
docker rmi is the command to delete images. This command will delete the alpine:latest image — docker rmi alpine:latest. You cannot delete an image that is associated with a container in the running (Up) or stopped (Exited) states.