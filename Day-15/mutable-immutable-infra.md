## Mutable vs Immutable Infrastructure

### Mutable Infrastructure

In Mutable infrastructure, the existing resources are modified to meet the new requirements. This means that the existing resources are updated with the new configuration or code changes. Here we'll be using the same Infrastructure but will be modifying the existing software resources to meet the new requirements. This is also called as "In-place" updates. This process is a example for Mutable infrastructure.

#### Configuration Drifts

If we are using mutable infrastructure, we need to be careful about configuration drifts. If we have more number of servers, making changes to the software in all servers might result in configuration drifts. If some servers don't have necessary dependencies to update the software, it might result in some servers not being able to update the software. This can lead to inconsistencies in the software versions across the servers. This is a common problem with mutable infrastructure.

### Immutable Infrastructure

In Immutable infrastructure, the existing resources are not modified. Instead, new servers are created with the updated software configuration. Once the new servers are created, the old servers are terminated. Any updates needed will be done by creating a new server with the updated software configuration. This process is a example for Immutable infrastructure.

#### Container Immutability

Containers are immutable by default. Since containers are created from images, if we need to update the software in the container, we need to create a new image with the updated software. Once the new image is created, we can create a new container from the new image. If we are using deployments in Kubernetes, we can update the deployment with the new image. This will create a new pod with the new image and terminate the old pod. But still it is possible for us to modify the existing container by copying files to the container or accessing the files inside container using container shell. This will increase the risk of security Vulnerabilities.

Date of Commit: 21/05/2025