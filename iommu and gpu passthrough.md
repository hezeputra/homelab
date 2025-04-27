# Enabling IOMMU and GPU Driver Installation

## Enabling and Test IOMMU State

> [!IMPORTANT]
> An IOMMU (Input-Output Memory Management Unit) is a hardware component that manages the memory used by I/O devices (like graphics cards, storage devices, and network interfaces). It translates the virtual addresses used by these devices to physical addresses, allowing for more efficient and secure direct memory access (DMA). Essentially, it's a specialized MMU (Memory Management Unit) for I/O devices.

### Enabling IOMMU

1. Enable IOMMU on in grub configuration within Node > Shell.

```
nano /etc/default/grub
```

2. Please add `intel_iommu=on` or `amd_iommu=on` depending on your processor (whether it is Intel or AMD) in line `GRUB_CMDLINE_LINUX_DEFAULT="quiet"`

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```

> [!IMPORTANT]
> for AMD processor will be `GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on"`

3. safe the file, update the grub and reboot your system

```
update-grub
```

```
reboot
```

### Test IOMMU State

1. Check Direct Memory Access (DMA) Remapping that includes IOMMU in kernel ring buffer.

```
dmesg | grep -e DMAR -e IOMMU
```

It should show you message that includes :

```
#....any other mesage
[ X.XXXXXX] DMAR: IOMMU enabled
#....any other mesage
```

2. Check for Interrupt Remapping in kernel ring buffer.

```
dmesg | grep 'remapping'
```

It should show you message that include :

```
[ X.XXXXXX] DMAR-IR: Queued invalidation will be enabled to support x2apic and Intr-remapping.
[ X.XXXXXX] DMAR-IR: Enabled IRQ remapping in x2apic mode
```

## GPU Driver Installation

## Pre Installation

1. Instal VA Info to check whether the GPU driver is installed or not

```
apt install vainfo -y
```

2. run `vainfo` to check the GPU driver. Normally, it should not available.

```
vainfo
```

It should show you message that includes :

```
#....any other mesage
libva info: va_openDriver() returns -1
#....any other mesage
libva info: va_openDriver() returns -1
vaInitialize failed with error code -1 (unknown libva error),exit
```

## GPU Driver Installation

1. Install `software-properties-common` to easily manage software sources by scripts and commands

```
apt install software-properties-common -y
```

2. Add `non-free` repositories

```
add-apt-repository -y non-free
```

> [!IMPORTANT]
> The `non-free` software is a software that is not open source but is still available (like Nvidia drivers, specialized firmware, Intel microcode, etc.)
> Debian and Debian-based systems (like Proxmox, Ubuntu) have different sections in their repositories:
>
> 1. main : completely free and open-source software.
> 2. contrib : free software that depends on non-free stuff.
> 3. non-free : software that is not free/not open-source.

3. Finally, install the GPU driver for intel N100

```
apt install intel-media-va-driver-non-free -y
```

## Check GPU Driver Installation

1. run `vainfo` to confirm the driver installation

```
vainfo
```

It should show you message that includes :

```
#....any other mesage
libva info: Trying to open /usr/lib/x86_64-linux-gnu/dri/XXXXXXXXXXXXXXXXXX.so
libva info: Found init function __vaDriverInit_1_17
libva info: va_openDriver() returns 0
#....any other mesage
vainfo: Driver version: XXXXXXXXXXXXXXXXXX
vainfo: Supported profile and entrypoints
#....any other mesage
```
