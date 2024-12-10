# Upstreamed
This fork has been upstreamed to the official Talos distribution. Starting from Talos 1.9, you will be able to download the Turing RK1 image from https://factory.talos.dev or select it from OMNI in the interface.

I didn't think is was worth the effort to release Talos 1.8.4. Just wait for 1.9.

## Updated components 1.8 -> 1.9

| Component | 1.8 | 1.9 |
| --- | --- | --- |
| Linux | 6.6.x | 6.12.x |
| U-Boot | 2024.01 | 2024.10 |
| Trusted ARM Firmware | 2.10.4 | 2.12.0 |
| RKBIN DDR Training | 1.16 | 1.18 |

## Changes

  - No more need for the RK3588 extension, everything is in the kernel of Talos 1.9
  - No more need for the `cpufreq-rockchip` module. The kernel uses cpufreq-dt, for memory frequency scaling the RKBIN DDR training has now dvfs support.
  - The hardware crypto offload module is removed from the kernel. Unfortunately, it was not upstreamed to kernel 6.12. Therefor I was not able to upstream it to the Talos Kernel. (feature patches to the kernel are not allowed)


### NVMe
In Talos 1.9 the bootloader (U-Boot/Trusted ARM Firmware and RKBIN DDR initialisation) got an update. If you boot from NVMe, you will need to update the bootloader (on eMMC) manually by following the instructions in the [Turing RK1 Talos documentation](https://www.talos.dev/v1.9/talos-guides/install/single-board-computers/turing_rk1/#booting-from-usb-or-nvme).

# Background on the upstream efforts

As Kernel 6.12 was merged in a later stage of the Talos 1.9 development, my timeframe to upstream the changes was very limited. The deadline was only a few days.

See https://taloscommunity.slack.com/archives/CJFGPG6DV/p1732882913886539

However, I was able to upstream everything to the Talos distribution. The following PRs were created:

 - Kernel: https://github.com/siderolabs/pkgs/pull/1100
 - Kernel Backport to 1.9: https://github.com/siderolabs/pkgs/pull/1101
 - sbc-rockchip overlay: https://github.com/siderolabs/sbc-rockchip/pull/35
 - sbc-overlay fix for eMMC boots: https://github.com/siderolabs/sbc-rockchip/pull/38
 - Image Factory: https://github.com/siderolabs/image-factory/pull/171
 - Omni: https://github.com/siderolabs/omni/pull/753
