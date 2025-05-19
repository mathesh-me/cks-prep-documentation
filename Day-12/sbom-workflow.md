## SBOM Workflow

### Overview

- Generate SBOM
- Store it in Secure Storage(Registry)
- Scan the SBOM for Vulnerabilities
- Analyze the Scan results
- Remediate the identified Vulnerabilities
- Continuously monitor the SBOM for new vulnerabilities

### Generate SBOM

- Based upon our needs, we can choose the SBOM format. Use SPDX for license compliance and CycloneDX for vulnerability tracking.
- Use tools like `Syft`, `CycloneDX`, or `Trivy` to generate SBOMs for your software components.
- Example command to generate SBOM using Syft:
  ```bash
  syft <image-name> -o json > sbom.json
  ```
- Once SBOM is generated, it can be stored in a secure storage solution like JFrog, Sonatype Nexus, and GitHub Packages.

### Scan SBOM for Vulnerabilities

- Use tools like `Trivy`, `Grype`, or `Anchore` to scan the SBOM for known vulnerabilities.
- Example command to scan SBOM using Trivy:
  ```bash
  grype sbom:sbom.json
  ```

- The scan results will provide a list of known vulnerabilities associated with the components in the SBOM, including their CVE IDs and descriptions.

### Analyze Scan Results

- Review the scan results to identify critical and high-severity vulnerabilities.
- Prioritize vulnerabilities based on their severity and potential impact on your application.
- Use the CVSS (Common Vulnerability Scoring System) to assess the severity of vulnerabilities and prioritize remediation efforts.

### Remediate Identified Vulnerabilities

- Update or patch vulnerable components to their latest versions.
- If a patch is not available, consider using alternative components or libraries that do not have known vulnerabilities.

### Continuously Monitor SBOM

- Establish a process for continuously monitoring and alerting on new vulnerabilities using CI/CD pipelines.

Date of Commit: 18/05/2025