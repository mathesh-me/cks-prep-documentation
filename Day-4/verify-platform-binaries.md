## Verify Platform Binaries

It is important to verify the platform binaries before deploying the Cluster. This is to ensure that the binaries are not tampered with and are from a trusted source. The following steps can be followed to verify the platform binaries:
1. **Download the Binaries**: Download the binaries from the official Kubernetes release page or the trusted source.
2. **Verify the Binaries**: Use the `sha256sum` command to verify the binaries. The command will generate a checksum for the binary and compare it with the checksum provided by the trusted source.
   ```bash
   sha256sum <binary-file>
   ```

If the output does not match the checksum provided by the trusted source, the binary may be tampered with and should not be used.

Date of Commit: 10/05/2025