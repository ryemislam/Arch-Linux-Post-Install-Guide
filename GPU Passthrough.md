## 1. Editing EFI Bootloader


```
sudo micro /boot/refind_linux.conf
```
OR
```
sudo micro /boot/EFI/limine/limine.conf
```

Add the following line at the end of "options root=" or "cmdline"

```
kvm.ignore_msrs=1 video=efifb:off rd.driver.pre=vfio-pci vfio-pci.ids=1002:73bf,1002:ab28,1002:73a6,1002:73a4
```

**Reboot**
## 2. IOMMU Groups

1. Configuring Libvirt


```
sudo pacman -S virt-manager qemu vde2 ebtables iptables-nft nftables dnsmasq bridge-utils ovmf swtpm
```

**Please note:** Conflicts may happen when installing these programs.  
A warning like the below example may apear in your terminal:

`:: iptables and iptables-nft are in conflict. Remove iptables? [y/N]`

If you do encounter this kind of message, press `y` and `enter` to continue the installation.

## 3. Configuration of libvirt and Virtual Machine Manager

1. libvirtd.conf

The first config file to edit is libvirtd.conf, located at `/etc/libvirt/libvirtd.conf`. Use your terminal editor (`vim`, `nvim` or `nano`) to edit this file. For example:

```
sudo micro /etc/libvirt/libvirtd.conf
```

**Avoid using graphical editors, as you will encounter permission problems.**

2. Read/Write permissions and group

Uncomment (remove the `#`) off the following lines:

```
unix_sock_group = "libvirt"
```

```
unix_sock_rw_perms = "0770"
```

Add these lines at the end of the file:

```
log_filters="3:qemu 1:libvirt"
log_outputs="2:file:/var/log/libvirt/libvirtd.log"
```

3. Adding your user to the libvirt group

You now need to add your user to the libvirt group, to allow libvirt to write files properly. This command assigns your user the libvirt group:

```
sudo usermod -a -G kvm,libvirt $(whoami)
```

4. You then need to enable and start the libvirtd service, just run these 2 commandes one after the other to do so:

```
sudo systemctl enable libvirtd
sudo systemctl start libvirtd
```

5. You can verify libvirt has been added to your users groups by using the following command.

```
sudo groups $(whoami)
```

### 4. qemu.conf

The second config file to edit is qemu.conf, located at `/etc/libvirt/qemu.conf`. Again, use your terminal editor to edit this file:

```
sudo micro /etc/libvirt/qemu.conf
```

Un-comment the following lines:

```
user = "root"
```

```
group = "root"
```

And replace `root` with your username:

```
user = "yourusername"
```

```
group = "yourusername"
```

Then restart the libvirt daemon with the following.

```
sudo systemctl restart libvirtd
```

### 5.  Enabling the virtual machine default network

**Important:** This will make your virsh internal network automatically start when you start up your computer.

```
sudo virsh net-autostart default
sudo virsh net-start default
```

### 6. GPU Isolation

#### 1. Mkinitcpio (Load VFIO Drivers)

```
sudo micro /etc/mkinitcpio.conf
```

Add **vfio_pci vfio vfio_iommu_type1** to MODULES
Add **modconf** to HOOKS

```
vfio_pci vfio vfio_iommu_type1
```

```
MODULES=(btrfs vfio_pci vfio vfio_iommu_type1)
BINARIES=(/usr/bin/btrfs)
FILES=()
HOOKS=(base udev autodetect microcode keyboard keymap modconf block filesystems fsck)
```

#### 2. Modprobe (Bind GPU to VFIO)

```
sudo micro /etc/modprobe.d/vfio.conf
```

Add the following line:

```
options vfio-pci ids=1002:73bf,1002:ab28,1002:73a6,1002:73a4
```

Regenerate the initramfs and reboot.

```
sudo mkinitcpio -P
```

**REBOOT**
