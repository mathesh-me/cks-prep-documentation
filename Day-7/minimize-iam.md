## Minimixing IAM Roles

- When creating IAM roles, it is important to follow the principle of least privilege. This means that you should only grant the permissions that are necessary for the role to perform its intended function.
- Group Users based on their job functions and assign permissions accordingly. This will help to minimize the number of permissions that are granted to each user.
- For assigning permissions to AWS Services, Use Instance Profiles if applicable. Instance profiles are a way to pass IAM roles to EC2 instances. This will  allow EC2 instances to access AWS services securely without doing any Programmatic access.
Regularly review IAM roles and permissions to ensure that they are still necessary and appropriate. If any roles or permissions are no longer needed, remove them to minimize the attack surface.
- Use tools like AWS Trusted Advisor to run Security checks.

Date of Commit: 13/05/2025