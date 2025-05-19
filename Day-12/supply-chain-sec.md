## Supply Chain Security

### Secure Software Developement Lifecycle (SSDLC)

- **Devlopment**: Code is written and tested in a secure environment.
- **Build**: Code is Compiled and packaged into a deployable artifact. It is necessary to ensure that the build process is secure and that the artifacts are not tampered with. So isolating the build environment from the rest of the system is important. Also managing dependencies upto date and scanning for vulnerabilities is important.
- **Test**: Images or artifacts that are built will scanned for vulnerabilities. This can be done using tools like Trivy, Clair, or Anchore. These tools can scan the images for known vulnerabilities and provide a report on the vulnerabilities found.
- **Deployment**: Should implement security measures to protect Deployments environments from unauthorized access. In Kubernetes, this can be done using Role-Based Access Control (RBAC), Pod Security Policies, and Network Policies. These measures can help ensure that only authorized users and applications can access the deployment environment.

### Benefits of Suply Chain Security

- **Early detection on issues**: By implementing security measures at each stage of the software development lifecycle, organizations can detect and address security issues early in the process, reducing the risk of vulnerabilities in production.
- **Continuous Inspection**: Preventing security incidents disturbing Production.
- **Compliance:** Many organizations are required to comply with industry regulations and standards, such as PCI DSS, HIPAA, and GDPR. Implementing supply chain security measures can help organizations meet these compliance requirements.
- **Incident Response**: Streamlined process minimize damage in the event of a security incident.
- **Security Posture Management**: Integrity of the software supply chain from every stage of the software development lifecycle.

Date of Commit: 18/05/2025