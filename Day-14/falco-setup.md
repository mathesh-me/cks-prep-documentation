## Setting up Falco

### Falco Working

- Falco will monitor the system calls made from User Space to Kernel Space.
- The system calls will be processed by the Falco engine and the rules will be applied to the system calls to detect any malicious activity.
- Falco will send alerts to the configured channels if any malicious activity is detected.
- Falco can be used to monitor the system calls made by a process and detect any malicious activity.

### Falco Installation

Since Falco needs access to the Kernel, We have two methods to install Falco:

- Kernel Module Method: In this approach, Falco will insert a kernel module into the kernel to monitor the system calls. But this is considered as intrusive and many cloud providers do not allow this.
- eBPF Method: In this approach, Falco will use eBPF to interact with the kernel and monitor the system calls. This is considered as non-intrusive and is supported by most cloud providers. So we will use this method to install Falco.

### Falco Installation Steps

- Installing Falsco as a Service
```
curl -s https://falco.org/repo/falcosecurity-3672BA8F.asc | apt-key add -
echo "deb https://download.falco.org/packages/deb stable main" | tee -a /etc/apt/sources.list.d/falcosecurity.list
apt update -y
apt-get install -y linux-headers-$(uname -r)
apt install -y falco
systemctl start falco
```
This will install Falco as a service on the host machine. Falco will monitor the system calls made by the processes running on the host machine and detect any malicious activity. It is considered as good practice to install Falco as service because even if the cluster is compromised, Falco will still be able to monitor the system calls made by the processes running on the host machine.
- Installing Falco using Helm
```
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm repo update
helm install falco falcosecurity/falco
```
This will install Falco in the `falco` namespace. Falco will deployed as a DaemonSet in the cluster. Falco will monitor the system calls made by the containers running in the cluster and detect any malicious activity.

Date of Commit: 20/05/2025