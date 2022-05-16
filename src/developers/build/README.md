# Building lockc

The first step to try out lockc is to build it. There are several ways to do
that:

* **[Cargo]** - build binaries with Cargo (Rust build system) on the host
  * convenient for local development, IDE/editor integration
* **[Container image]** - build a container image which can be deployed on
  Kubernetes
  * the only method to try lockc on Kubernetes
  * doesn't work for Docker integration, where we rather install lockc as a
    binary on the host, managed by systemd

[Cargo]: cargo.md
[Container image]: container-image.md
