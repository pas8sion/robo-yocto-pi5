# robo-yocto-pi5

Custom Linux image for Raspberry Pi 5 built with the Yocto Project 5.0 (Scarthgap).

## Layers

| Layer | Source | Purpose |
|---|---|---|
| `poky` | git submodule | Yocto reference distro + BitBake |
| `meta-raspberrypi` | git submodule | Raspberry Pi 5 board support |
| `meta-robot` | this repo | Project config templates and own recipes |

## Requirements

- Linux build host (tested: Ubuntu 22.04 container on Apple M1 Pro, Docker Desktop, arm64)
- At least 50 GB of free disk space, 16+ GB RAM recommended

## Clone

```bash
git clone --recurse-submodules https://github.com/pas8sion/robo-yocto-pi5.git
cd robo-yocto-pi5
```

## Build

```bash
TEMPLATECONF=$PWD/meta-robot/conf/templates/default source poky/oe-init-build-env build
bitbake core-image-minimal
```

The `TEMPLATECONF` variable is only needed the first time, when `build/conf` does not exist yet.

Output image:

build/tmp/deploy/images/raspberrypi5/core-image-minimal-raspberrypi5.rootfs.wic.bz2


## Image configuration

Defined in `meta-robot/conf/templates/default/local.conf.sample`:

- `MACHINE = "raspberrypi5"`
- `INIT_MANAGER = "systemd"`
- OpenSSH server (`ssh-server-openssh`)
- Wi-Fi firmware license accepted (`synaptics-killswitch`)
- Boots from USB storage only (`/dev/sda`): USB flash drive or USB SSD, not microSD

## Access

Login as `root` with an empty password (via SSH or console).

> **Warning:** passwordless root login comes from `debug-tweaks` and is for lab use only. Never use this image on an untrusted network or in production.

