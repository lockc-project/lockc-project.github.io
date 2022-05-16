# Cargo

lockc is written entirely in Rust and uses Cargo as a build system.

## Prerequisites

This document assumes that you have Rust installed with
[rustup](https://rustup.rs/).

To build lockc, you will need Rust stable and nightly:

```bash
rustup install stable
rustup toolchain install nightly --component rust-src
```

Then you need to install `bpf-linker` for linking eBPF programs:

```bash
cargo install bpf-linker
```

By default, `bpf-linker` is trying to use the internal LLVM library available
in Rust. That might not work if you are using musl target. In such case, you
need to install LLVM with static libraries on your host system and then install
`bpf-linker` with a different command:

```bash
cargo install --git https://github.com/aya-rs/bpf-linker --tag v0.9.3 --no-default-features --features system-llvm -- bpf-linker
```

## Building lockc

After installing all needed dependencies, it's time to build lockc.

You can build and run the entire project with:

```bash
cargo xtask run
```

If you prefer to only build the project, it be done with:

```bash
cargo xtask build-ebpf
cargo build
```

Running tests:

```bash
cargo test
```

Running lints:

```bash
cargo clippy
```

## Installing lockc

To install lockc on your host, use the following command:

```bash
cargo xtask install
```

Do not run this command with sudo! Why?

tl;dr: you will be asked for password when necessary, don't worry!

Explanation: Running cargo with sudo ends with weird consequences like not
seing cargo content from your home directory or leaving some files owned by
root in `target`. When any destination directory is owned by root, sudo will
be launched automatically by `xtask install` just to perform necessary
installation steps.

By default it tries to install lockcd binary in `/usr/local/bin`, but the
destination directory can be changed by the following arguments:

* `--destdir` - the rootfs of your system, default: `/`
* `--prefix` - prefix of the most of installation destinations, default:
  `usr/local`
* `--bindir` - directory for binary files, default: `bin`
* `--unitdir` - directory for systemd units, default: `lib/systemd/system`
* `--sysconfdir` - directory for configuration files, default: `etc`

By default, binaries are installed from the `debug` target profile. If you want
to change it, use the `--profile` argument. `--profile release` is what you
most likely want to use when packaging or installing on the production system.

## Building tarball with binary and unit

To make distribution of lockc for Docker users easier, we have a possibility of
building an archive with binary and systemd unit which can be just unpacked in
`/` directory. It can be done by the following command:

```bash
cargo xtask bintar
```

By default it archives lockcd binary in `usr/local/bin`, but the
destination directory can be changed by the following arguments:

* `--prefix` - prefix of the most of installation destinations, default:
  `usr/local`
* `--bindir` - directory for binary files, default: `bin`
* `--unitdir` - directory for systemd units, default: `lib/systemd/system`
* `--sysconfdir` - directory for configuration files, default: `etc`

By default, binaries are installed from the `debug` target profile. If you want
to change it, use the `--profile` argument. `--profile release` is what you
most likely want to use when creating a tarball for releases and production
systems.

The resulting binary should be available as `target/[profile]/lockc.tar.gz`
(i.e. `target/debug/lockc.tar.gz`).
