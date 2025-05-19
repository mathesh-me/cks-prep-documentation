## Minimize Base Image Footprint

### Minimizing the Base Image Footprint in Docker

```Dockerfile
FROM alpine:latest
RUN apk add --no-cache \
    python3 \
    py3-pip \
    && pip install --no-cache-dir \
    requests \
    flask
```

- In the above example, we use the `alpine` as parent image, on top of which we are building our application. So, any image that acts as foundation to build our image is called parent image. And any image that built from scratch is called Base Image.

### Best Practices for Building Docker Images

- **Modular Design**: Break down your application into smaller, reusable components. Fo example, if you have a web application, separate the frontend and backend into different images. This allows for easier updates and scaling. Also each image is going to manage their own libraries and dependencies only.
- **Avoid Data Storage in Container**: Containers are ephemeral by nature. Avoid storing data in the container itself. Instead, use volumes or bind mounts to persist data outside the container. This ensures that your data remains intact even if the container is removed or recreated.
- **Use Official Images**: Whenever possible, use official images from Docker Hub or other trusted sources. These images are maintained by the community and are often optimized for performance and security. For example, instead of using a generic `ubuntu` image, consider using `ubuntu:20.04` to ensure you are using a specific version.
- **Use smaller base images**: Use smaller base images like `alpine` to reduce the overall size of your image. This not only saves disk space but also speeds up the build and deployment process. For example, instead of using `ubuntu`, consider using `alpine` as your base image. Install only the necessary packages and dependencies required for your application. Unessary tools and libraries should be removed.
- **Separate Dev and Prod Images**: Create separate images for development and production environments. The development image can include additional tools and libraries needed for debugging, while the production image should be minimal and optimized for performance. For example, you can create a `Dockerfile.dev` for development and a `Dockerfile.prod` for production.

### Minimizing Vulnerabiltites

- Reducing number of packages and libraries in the image reduces the attack surface. For example, if you are using a web server like Nginx, consider using a minimal image that only includes the necessary modules for your application. This reduces the number of potential vulnerabilities in your image.
- **Google Distroless Images**: Consider using Google Distroless images, which are designed to be minimal and secure. These images contain only the application and its runtime dependencies, without any package manager or shell. This reduces the attack surface and minimizes the risk of vulnerabilities. For example, instead of using a full `ubuntu` image, consider using `gcr.io/distroless/base` as your base image.
- Normally larger images have more vulnerabilities compared to smaller images. 

Date of Commit: 18/05/2025