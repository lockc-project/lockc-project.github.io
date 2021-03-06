# Mount policies

lockc comes with the following policies about bind mounts from host filesystem
to containers (via `-v` option) for each policy level:

* **baseline** - allow bind mounting from inside `/home` and `/var/data`.
* **restricted** - does not allow any bind mounts from host
* **privileged** - no restrictions, everything can be bind mounted

The **baseline** behavior in lockc is slightly different than in the Kubernetes
Pod Security Admission controller, which disallows all host mounts for baseline
containers as well as for restricted. The motivation behind allowing `/home`
and `/var/data` by lockc is that they are often used in local container engines
(Docker, podman) for reasons like:

* mounting the source code to build or check
* storing database content on the host for local development

By default, with the **baseline** policy level, this is a good example of
not allowed behavior:

```bash
# docker run --rm -it -v /:/rootfs registry.opensuse.org/opensuse/toolbox:latest
docker: Error response from daemon: OCI runtime create failed: container_linux.go:380: starting container process caused: process_linux.go:545: container init caused: rootfs_linux.go:76: mounting "/" to rootfs at "/rootfs" caused: mount through procfd: operation not permitted: unknown.
```
