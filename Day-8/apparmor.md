## AppArmor

AppArmor is a Linux kernel security module that helps us to restrict the capabilities of applications to access system resources. We can use AppArmor to restrict appliactions to limited set of resources like specific file or dirs, network settings and other linux capabilities.

### Working with AppArmor

- Verify the status of AppArmor on the system:
```bash
systemctl status apparmor
```
- To check whether AppArmor Kernel module is loaded
``bash
cat /sys/module/apparmor/parameters/enabled
```
- To view all loaded AppArmor profiles:
```bash
cat /sys/kernel/security/apparmor/profiles
```
- Checking the status of AppArmor profiles:
```bash
aa-status
```

### AppArmor Profile Modes

- **Enforce Mode**: In this mode, the AppArmor profile is enforced, and any violations of the profile will result in the application being denied access to the specified resources.
- **Complain Mode**: In this mode, the AppArmor profile is not enforced, but any violations of the profile will be logged. This mode is useful for testing and debugging profiles without enforcing them.
- **Unconfined Mode**: In this mode, the application is not confined by any AppArmor profile, and it has full access to the system resources. This mode is typically used for applications that do not require confinement or for testing purposes.

Date of Commit: 14/05/2025