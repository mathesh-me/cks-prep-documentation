# Certified Kubernetes Administrator (CKA) Exam Preparation Documentation

Welcome to my Certified Kubernetes Security Specialist (CKS) Exam Preparation Documentation. This documentation is a collection of my notes, commands, and tips for the Exam. I will update this documentation as I continue to prepare for the exam. 

<p align="center">
    <img src="https://github.com/user-attachments/assets/b405ef20-5876-4582-8ca6-335be334d677" alt="cks" />
</p>

## Resources I used for the Exam Preparation

- Kubernetes [Official Documentation](https://kubernetes.io/docs/home/)
- Killer Shell [CKS Exam Simulator](https://killer.sh/cks)
- Killer Coda [CKS Interactive Scenarios](https://killercoda.com/killer-shell-cks)
- Mumshad Mannambeth's [Certified Kubernetes Security Specialist Course on KodeKloud](https://learn.kodekloud.com/user/courses/certified-kubernetes-security-specialist-cks)
- [Free CKS Self-Study Course from RX-M](https://rx-m.com/cks-self-study-course/)
- Cilium [Documentation](https://docs.cilium.io/en/stable/index.html)
- Istio [Documentation](https://istio.io/latest/docs/)

## CKS Domains & Competencies(As of June 2025)

### Cluster Setup
- Use Role Based Access Controls to minimize exposure
- Exercise caution in using service accounts e.g. disable defaults, minimize permissions on newly created ones
- Restrict access to Kubernetes API
- Upgrade Kubernetes to avoid vulnerabilities

### System Hardening
- Minimize host OS footprint (reduce attack surface)
- Using least-privilege identity and access management
- Minimize external access to the network
- Appropriately use kernel hardening tools such as AppArmor, seccomp

### Minimize Microservice Vulnerabilities
- Use appropriate pod security standards
- Manage Kubernetes secrets
- Understand and implement isolation techniques (multi-tenancy, sandboxed containers, etc.)
- Implement Pod-to-Pod encryption (Cilium, Istio)

### Supply Chain Security
- Minimize base image footprint
- Understand your supply chain (e.g. SBOM, CI/CD, artifact repositories)
- Secure your supply chain (permitted registries, sign and validate artifacts, etc.)
- Perform static analysis of user workloads and container images (e.g. Kubesec, KubeLinter)

### Monitoring, Logging and Runtime Security20%
- Perform behavioral analytics to detect malicious activities
- Detect threats within physical infrastructure, apps, networks, data, users and workloads
- Investigate and identify phases of attack and bad actors within the environment
- Ensure immutability of containers at runtime
- Use Kubernetes audit logs to monitor access
