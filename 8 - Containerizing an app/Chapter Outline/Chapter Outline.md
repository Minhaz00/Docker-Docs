# Containerizing an App with Docker

Docker simplifies the process of packaging application source code and getting it running in a container. This process is known as containerization.

### The Structure of This Chapter:
1. The TLDR
2. The Deep Dive
3. The Commands

### Let's Containerize an App!

### Containerizing an App - The TLDR
Containers make it straightforward to build, ship, and run applications. The end-to-end process is as follows:
1. Start with our application code and dependencies.
2. Create a Dockerfile that describes our app, dependencies, and how to run it.
3. Build it into an image using the `docker build` command.
4. Push the new image to a registry (optional).
5. Run a container from the image.

### Containerizing an App - The Deep Dive

#### Sections:
1. Containerize a single-container app.
2. Moving to Production with multi-stage builds.
3. Multi-platform builds.
4. A few best practices.