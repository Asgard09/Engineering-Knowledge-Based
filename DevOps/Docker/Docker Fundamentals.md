# 1. What’s container?
Container is a lightweight, executable package of software include everything needed to run an application from code, runtime, system tools, library, config,…
# 2. Images and Layers
An image built from read only layers stacked on top of each other.
- Caching: If layer 1 and 2 not change docker reuse them from cache, only layer 3 need to rebuild.
- Sharing: Multiple image sharing the same base layer.
- Immutability: Layers are read-only
>The principle: put frequently changing at the top, rarely changing at the bottom (stack)
# 3. Syntax
```dockerfile
FROM openjdk:17
WORKDIR /app
COPY target/app.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
```
- FROM: define the base image.
- WORKDIR: set the working directory. Equivalent to cd.
- COPY: copy file from your computer into the image.
- EXPOSE: define which port that application use.
- CMD: define the default command when container start. Example: when running `docker run spring-app`, docker executes `java -jar app.jar`
# 4. Images and Containers
- Images: a read only packaged.
- Containers: a running instance of images, include writeable layers.
# 5. Virtual Machine vs Containers
- VM: visualize hardware, has it owns operating system, Kernel, virtual CPU/RAM/Storage.
- Container: visualize operating system, sharing: host OS kernel, host hardware.
