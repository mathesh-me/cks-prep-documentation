## AquaSec Tracee

Tracee uses eBPF (Extended Berkeley Packet Filter) technology to monitor and trace system calls on Containers at runtime without modifying kernel source code or loading additional kernel moules.

### Running Tracee as a Container

#### Prerequisites

- By default Tracee stores at `/tmp/tracee` directory. Make sure to mount the /tmp/tracee directory from the host to the container.
- Trace requires access to kernel headers to compile epBF programs. These headers are located at `/lib/modules` and their dependencies in `/usr/src`. Make sure to mount these directories from the host to the container.
- Tracee need extended privileges for system call tracing. Make sure to run the container with `--privileged` flag.

#### Running Tracee

- For Single Command
```bash
docker run --name tracee --rm --privileged --pid=host \
  -v /lib/modules/:/lib/modules:ro \
  -v /usr/src:/usr/src:ro \
  -v /tmp/tracee:/tmp/tracee \
  aquasec/tracee:0.4.0 --trace comm=ls
```

- For new Processes
```bash
sudo docker run --name tracee --rm --privileged --pid=host \
  -v /lib/modules/:/lib/modules:ro \
  -v /usr/src:/usr/src:ro \
  -v /tmp/tracee:/tmp/tracee \
  aquasec/tracee:0.4.0 --trace pid=new
```

- For new Containers
```bash
sudo docker run --name tracee --rm --privileged --pid=host \
  -v /lib/modules/:/lib/modules:ro \
  -v /usr/src:/usr/src:ro \
  -v /tmp/tracee:/tmp/tracee \
  aquasec/tracee:0.4.0 --trace container=new
```

Date of Commit: 13/05/2025