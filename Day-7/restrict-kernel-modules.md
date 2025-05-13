## Restrict Kernel Modules

Linux Kernel is based on Modular architecture. It is possible to load and unload kernel modules at runtime. This can be done using the `modprobe` command. Kernel modules are used to extend the functionality of the kernel. However, loading arbitrary kernel modules can pose a security risk. kernel modules can be loaded either automatically or manually.

### Commands for Managing Kernel Modules

- To list all loaded kernel modules, use the following command:
```bash
lsmod
```

- To load a kernel module, use the following command:
```bash
modprobe <module-name>
```

- To unload a kernel module, use the following command:
```bash
modprobe -r <module-name>
```

### Blacklisting Kernel Modules

- To prevent a kernel module from being loaded, we can blacklist it. This can be done by adding the module name to the `/etc/modprobe.d/blacklist.conf` file. For example, to blacklist the `usb-storage` module, add the following line to the file:
```bash
blacklist usb-storage
```

Date of Commit: 13/05/2025