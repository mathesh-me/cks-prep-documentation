## Kata Containers

Kata Containers uses a lightweight virtual machine (VM) for each container. So, each container will have its own kernel and will be isolated from the host kernel thus providing isolation and security. It have some tradeoffs too. It requires additional memory and CPU resources compared to traditional container runtimes. Also it is based on hardware virtualization support, so we need a dedicated physical hardware or if we are using a VM from cloud provider, we need to make sure that the provider supports nested virtualization.

Date of Commit: 17/05/2025