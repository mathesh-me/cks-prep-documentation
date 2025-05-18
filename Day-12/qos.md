## Quality of Service (QoS) in Kubernetes

Quality of Service (QoS) in Kubernetes is a mechanism that allows you to classify and manage the resource allocation. In QoS, pods are classified into three different classes based on their resource requests and limits:
- **Guaranteed**: Pods that have both CPU and memory requests and limits set to the same value. These pods are guaranteed to get the requested resources and will not be evicted under memory pressure.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: guaranteed-pod
spec:
  containers:
    - name: guaranteed-container
      image: nginx
      resources:
        requests:
          memory: "512Mi"
          cpu: "500m"
        limits:
          memory: "512Mi"
          cpu: "500m"
```

- **Burstable**: Pods that have CPU and memory requests and limits set, but the requests and limits are not equal. These pods can burst above their requested resources if there are available resources in the cluster, but they are not guaranteed to get those resources. They are given a higher priority than BestEffort pods but lower than Guaranteed pods.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: burstable-pod
spec:
  containers:
    - name: burstable-container
      image: nginx
      resources:
        requests:
          memory: "256Mi"
          cpu: "250m"
        limits:
          memory: "512Mi"
          cpu: "1"
```

- **BestEffort**: Pods that do not have any CPU or memory requests or limits set. These pods are the lowest priority and can be evicted under memory pressure.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: besteffort-pod
spec:
  containers:
    - name: besteffort-container
      image: nginx
```

Apart from the above QoS classes, We can also implement `Network QoS` and `Storage QoS` in Kubernetes.<br>

### Network QoS

- Network QoS in Kubernetes is used to manage the network bandwidth for pods. It allows us to set limits on the network bandwidth that a pod can use. This is particularly useful in multi-tenant environments where different teams or applications may have different network requirements.It is not implemented by default in Kubernetes, but it can be achieved using various CNI (Container Network Interface) plugins that support QoS features. Some of the popular CNI plugins that support network QoS include Calico, Cilium, and Weave Net.

```yaml
apiVersion: crd.projectcalico.org/v1
kind: NetworkPolicy
metadata:
  name: tenant-a-network-policy
  namespace: namespace-a
spec:
  selector: all()  # Apply to all pods in Namespace A
  ingress:
    - action: Allow
      protocol: TCP
      destination:
        ports: [80]
      limits:
        rate: 10Mbps  # Limit ingress traffic to 10Mbps for tenant pods
  egress:
    - action: Allow
      protocol: TCP
      destination:
        ports: [80]
      limits:
        rate: 10Mbps  # Limit egress traffic to 10Mbps for tenant pods
```

### Storage QoS

It is used to manage the number of IOPS (Input/Output Operations Per Second) that a pod can perform in its volume. We can implement storage QoS in Kubernetes by using different storage classes and the QoS features provided by the underlying storage system. For example, if we are using a cloud provider like AWS or GCP, you can create different storage classes with different performance characteristics (e.g., SSD vs. HDD) and assign them to different pods based on their QoS requirements.


```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: high-performance
provisioner: kubernetes.io/aws-ebs
parameters:
  type: io1            # AWS io1 disks support high IOPS
  iopsPerGB: "50"      # Specify high IOPS per GB
  fsType: ext4
reclaimPolicy: Delete
allowVolumeExpansion: true
volumeBindingMode: Immediate
```

Date of Commit: 18/05/2025