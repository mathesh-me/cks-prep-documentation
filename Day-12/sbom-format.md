## SBOM Formats

SBOM (Software Bill of Materials) can be represented in two main formats: SPDX and CycloneDX. Each format has its own strengths and is used in different contexts.
- **CycloneDX**: A lightweight SBOM standard designed for use in application security contexts. It is XML or JSON-based and focuses on providing a clear representation of software components, their relationships, and vulnerabilities.
- **SPDX (Software Package Data Exchange)**: An open standard for communicating software bill of materials information. SPDX is more comprehensive and can represent complex relationships between components. It is widely used in open source projects and compliance contexts.

### SPDX Format

It organizes information about software components, licenses, and copyrights in a structured format. SPDX files can be in RDF/XML, Tag/Value, or JSON formats. The SPDX format is widely used in open source projects and compliance contexts.

#### Structure of SPDX

- **Documents Information**: Contains metadata about the SBOM, including the document name, creation date, and license information. 
- **Relationships**: Defines the relationships between different components in the SBOM, such as dependencies or parent-child relationships.
- **Package Information**: Provides detailed information about each software package, including its name, version, download location, and license information.
- **Snippet**: A small portion of the SPDX file that provides details about smaller sections of code or components.
- **License Information**: Contains information about the licenses associated with each software component, including the license ID and any relevant comments.
- **Files Information**: Provides information about the files associated with each software component, including their names, checksums, and file types.
- **Annotations**: Allows for additional comments or notes to be added to the SBOM, providing context or explanations for specific components or relationships.

```json
{
  "SPDXID": "SPDXRef-DOCUMENT",
  "name": "Example SBOM",
  "dataLicense": "CC0-1.0",
  "documentNamespace": "http://spdx.org/spdxdocs/example-sbom-1.0",
  "creationInfo": {
    "created": "2023-10-01T12:00:00Z",
    "creators": [
      "Tool: Example SBOM Generator"
    ]
  },
  "packages": [
    {
      "SPDXID": "SPDXRef-Package",
      "name": "example-library",
      "versionInfo": "2.0.0",
      "downloadLocation": "https://example.com/example-library-2.0.0.tar.gz",
      "licenseConcluded": "Apache-2.0",
      "licenseDeclared": "Apache-2.0"
    }
  ]
}
```

### CycloneDX Format

CycloneDX is a lightweight SBOM standard designed for use in application security contexts. It is XML or JSON-based and focuses on providing a clear representation of software components, their relationships, and vulnerabilities.

#### Structure of CycloneDX

- **BOM Metadata**: Contains metadata about the SBOM, including the timestamp, tools used to generate the SBOM, and the component being described.
- **Components List**: A list of all components included in the SBOM, including their names, versions, licenses, and properties.
- **Vulnerabilities**: A list of known vulnerabilities associated with the components in the SBOM, including their CVE IDs and descriptions.
- **Software Service**: Information about the software service being described, including its name, version, and description.
- **Dependencies**: A list of dependencies between components, including their relationships and versions.
- **Annotations**: Allows for additional comments or notes to be added to the SBOM, providing context or explanations for specific components or relationships.

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
          "name": "vulnerability",
          "value": [
            {
              "cveId": ["CVE-2023-12345"],
              "description": ["Description of the vulnerability"]
            }
          ]
        }
      ]
    }
  ]
}
```

Date of Commit: 18/05/2025