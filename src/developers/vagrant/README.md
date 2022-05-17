# Development environment (Vagrant)

There is a possibility to run lockc build from source in a virtual machine using
Vagrant:

```bash
vagrant up
```

Our `Vagrantfile` supports the following environment variables:

* `LOCKC_VAGRANT_CPUS` - number of vCPUs
* `LOCKC_VAGRANT_MEMOORY` - memory (in MB)

When VM is provisioned successfully, you can access it using:

```bash
vagrant ssh
```

That VM contains is running [k3s](https://k3s.io/). It's also running lockc as
a systemd service, which can be checked with:

```bash
sudo systemctl status lockc
sudo journalctl -fu lockc
```

lockc source tree is available in `/vagrant` directory. After making changes in
code, you can sync the changes (from the host):

```bash
vagrant rsync
```

Then build, install and restart lockc in VM (inside `vagrant ssh` session):

```bash
sudo systemctl stop lockc
cargo xtask build-ebpf
cargo build
cargo xtask install
sudo systemctl start lockc
```
