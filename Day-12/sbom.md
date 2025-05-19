## SBOM(Software Bill of Materials)

### 1. What is SBOM?

SBOM (Software Bill of Materials) is a comprehensive inventory of all components, libraries, and dependencies that make up a software application. It provides detailed information about the software's structure, including version numbers, licenses, and any known vulnerabilities associated with the components. SBOMs are essential for understanding the security posture of software and ensuring compliance with licensing requirements.
It helps organizations manage security risks, maintain compliance and ensure transparency and Integrity in their software supply chain. 
Simplifies dependency management by mapping out all components and their dependencies.It also ensure all componenets are up to date and secure.

### Benefits of SBOM

- **Transparency**: Provides a clear view of all components in a software application, making it easier to identify potential vulnerabilities and compliance issues.
- **Incident Response**: In the event of a security breach, an SBOM can help organizations quickly identify affected components and take appropriate action.
- **Dependency Management**: Helps organizations manage software dependencies more effectively.
- **Compliance**: Assists in ensuring that software components comply with licensing requirements and industry standards.
- **Vulnerability Tracking**: Enables organizations to track known vulnerabilities in their software components and take action to mitigate risks.

### SBOM Example

```json
{
  "bomFormat": "CycloneDX",
  "specVersion": "1.4",
  "version": 1,
  "metadata": {
    "timestamp": "2023-10-01T12:00:00Z",
    "tools": [
      {
        "vendor": "Example Corp",
        "name": "SBOM Generator",
        "version": "1.0.0"
      }
    ],
    "component": {
      "type": "application",
      "name": "example-app",
      "version": "1.0.0",
      "description": "An example application for demonstration purposes.",
      "licenses": [
        {
          "license": {
            "id": "MIT"
          }
        }
      ]
    }
  },
  "components": [
    {
      "type": "library",
      "name": "example-library",
      "version": "2.0.0",
      "licenses": [
        {
          "license": {
            "id": "Apache-2.0"
          }
        }
      ],
      "properties": [
        {
          "name": "vulnerabilityCount",
          "value": 5
        }
      ]
    }
  ]
}
```

The example above is a simplified SBOM in JSON format, following the CycloneDX specification. It includes metadata about the software application, such as its name, version, and licenses, as well as a list of components with their respective licenses and properties. The `vulnerabilityCount` property indicates the number of known vulnerabilities associated with the component.

Date of Commit: 18/05/2025