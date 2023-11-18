---
title: "Turing RK1"
description: "Installing Talos on Turing RK1 SOM using raw disk image."
aliases: 
  - ../../../single-board-computers/turing_rk1
---

[![asciicast](https://asciinema.org/a/635709.svg)](https://asciinema.org/a/635709)

## Prerequisites

You will need

- `talosctl`
- `tpi` from [github](https://github.com/turing-machines/tpi/releases)

Download the latest `talosctl`.

```bash
curl -Lo /usr/local/bin/talosctl https://github.com/siderolabs/talos/releases/download/{{< release >}}/talosctl-$(uname -s | tr "[:upper:]" "[:lower:]")-amd64
chmod +x /usr/local/bin/talosctl
```


## Download the Image

Download the image and decompress it:

```bash
curl -LO https://github.com/nberlee/talos/releases/download/{{< release >}}/metal-turing_rk1-arm64.raw.xz
xz -d metal-turing_rk1-arm64.raw.xz
```

The user has two options to proceed:

- booting from eMMC
- booting from a USB or NVMe (requires a spi image on the eMMC)

### Booting from eMMC

```bash
tpi flash -n <NODENUMBER> -i metal-turing_rk1-arm64.raw
tpi power on -n <NODENUMBER> 
```
Proceed to [bootstrapping the node](#bootstrapping-the-node).

### Booting from USB or NVMe

This requires the user to flash the Turing RK1 with a u-boot SPI image.

If you have a USB to NVMe adapter, write Talos image to the USB drive:

```bash
sudo dd if=metal-turing_rk1-arm64.raw of=/dev/sda
```
install the NVMe drive in the Turing Pi board.

Otherwise, if the NVMe drive is already installed in the Turing Pi board, you can:
- Flash the Turing RK1 variant of [Ubuntu](https://docs.turingpi.com/docs/turing-rk1-flashing-os) to the eMMC.
- Boot into the ubuntu image, write Talos image to the SSD drive right from your Turing PI board:

```bash
sudo dd if=metal-turing_rk1-arm64.raw of=/dev/NVMe0n1
```
- Download u-boot image from talos u-boot:

```bash
mkdir _out

docker pull ghcr.io/nberlee/u-boot:v1.6.0-29-g7be6f52-dirty
docker save ghcr.io/nberlee/u-boot:v1.6.0-29-g7be6f52-dirty | tar x --strip-components=1 -C _out
tar xvf _out/layer.tar turing_rk1/u-boot-rockchip-spi.bin
```

As the spi-image may be too small for the BMC you may have to flash the raw image first to the eMMC:
```
tpi flash -n <NODENUMBER> -i metal-turing_rk1-arm64.raw
```

After this flash the spi image to get rid of all the extra partitions and changed the boot order on the spi 
image will prefer the nvme drive:

```
tpi flash -n <NODENUMBER> -i turing_rk1/u-boot-rockchip-spi.bin
tpi power on -n <NODENUMBER> 
```


After these steps, Talos will boot from the NVMe/USB and enter maintenance mode.
Proceed to [bootstrapping the node](#bootstrapping-the-node).

## Bootstrapping the Node

To check bootmessages, repeat until you see instructions for bootstrapping the node:
```sh
tpi uart -n <NODENUMBER> get
```

Wait for the uart to show you the instructions for bootstrapping the node.
Following the instructions in the uart output to connect to the interactive installer:

```bash
talosctl apply-config --insecure --mode=interactive --nodes <node IP or DNS name>
```

Once the interactive installation is applied, the cluster will form and you can then use `kubectl`.

## Retrieve the `kubeconfig`

Retrieve the admin `kubeconfig` by running:

```bash
talosctl kubeconfig
```
