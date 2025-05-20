## Scanning Images

### CVE

- CVE Stands for Common Vulnerabilities and Exposures. It is a list of publicly known cybersecurity vulnerabilities and exposures. 
- Each CVE entry contains an identification number, a description, and at least one public reference. The CVE list is maintained by the MITRE Corporation and is used by various organizations and security tools to identify and track vulnerabilities in software and systems.
- CVE entries are assigned a unique identifier in the format CVE-YYYY-NNNN, where YYYY is the year of the entry and NNNN is a sequential number.
- Each CVE will have remediation steps and a CVSS score. The CVSS score is a numerical score that reflects the severity of the vulnerability, with higher scores indicating more severe vulnerabilities.<br>

**CVE Classification:**

CVE are classified as follows:
- Vulnerabilities that bypass security controls(Accessing sensitive data that are not intended to be accessed)
- Vulnerabilities that causes damage to the system(Exploiting a vulnerability to cause damage to the system)

Each CVE will have a CVSS Score from 0 to 10. The higher the score, the more severe the vulnerability is. The CVSS score is calculated based on several factors, including:

### Vulnerabilities Remediation

If a system have numerous packages and containers, it is not feasible to update all of them. In such cases, we can use the following methods to remediate vulnerabilities:
- **Update the vulnerable package**: This is the most straightforward approach. If a newer version of the package is available that fixes the vulnerability, we can update to that version.
- **Use a different package**: If the vulnerable package is not essential, we can consider using a different package that provides similar functionality without the vulnerability.
- **Apply security measures**: If updating the package is not possible, we can apply security measures to mitigate the risk of exploitation.
- **Remove the vulnerable package**: If the package is not essential, we can consider removing it from the system.

### Image Scanning using Trivy

- Trivy is a simple and comprehensive vulnerability scanner for containers and other artifacts. It detects vulnerabilities in OS packages and application dependencies. We can also integrate Trivy with CI/CD pipelines to ensure that only secure images are deployed to production.
- Trivy can be used to scan container images, file systems, and Git repositories for vulnerabilities. It provides detailed information about the vulnerabilities found, including CVE IDs, severity levels, and remediation steps.<br>

**Installation:**

- We can install Trivy on ubuntu using the following command:
```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```

- Once installed, we can run Trivy against a container image:
```bash
trivy image <image-name>
```
This will output a list of libraries and packages that are vulnerable, along with their CVE IDs and severity levels. We can also use the `--severity` flag to filter the results by severity level:
- We can also use the `--ignore-unfixed` flag to ignore vulnerabilities that do not have a fix available:
```bash
trivy image --ignore-unfixed <image-name>
```

### Best Practices for Image Scanning

- Regularly scan images for vulnerabilities: We should regularly scan our images for vulnerabilities to ensure that we are using secure images.
- We can integrate scanning itno our deployment workflow using Admission before deloying Pods to the cluster.
- Maintaining an internal registry that have pre-scanned images.
- Integrating Image scanning into CI/CD pipelines: We can integrate image scanning into our CI/CD pipelines to ensure that only secure images are deployed to production.

Date of Commit: 19/05/2025