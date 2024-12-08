# Proxmox GPU Passthrough


- Add this to `/etc/modprobe.d/vfio.conf`
  - Take the devide id of the GPU to passthrough
    ```
    lspci -vn
    ```
  - Replace {device-id} by the device id of the GPU
    ```
    echo "options vfio-pci ids={device-id} disable vga=1"> /etc/modprobe.d/vfio.conf
    ```
- Add this to `/etc/modules`
  ```
  echo -e "vfio\nvfio_iommu_type1\nvfio_pci\nvfio_virqfd">> /etc/modules
  ```
- Add this to `/etc/kernel/cmdline`
  - Intel CPU
    ```
    echo "root=ZFS=rpool/ROOT/pve-1 boot=zfs intel_iommu=on"> /etc/kernel/cmdline
    ```
  - AMD CPU
    ```
    echo "root=ZFS=rpool/ROOT/pve-1 boot=zfs amd_iommu=on"> /etc/kernel/cmdline
    ```
- Add this to `/etc/modprobe.d/pve-blacklist.conf`
  ```
  echo -e "blacklist nvidiafb\nblacklist nvidia\nblacklist radeon\nblacklist nouveau"> /etc/modprobe.d/pve-blacklist.conf
  ```
- Update the configuration
  ```
  proxmox-boot-tool refresh
  ```
- Update initramfs
  ```
  update-initramfs -u -k all
  ```
- Reboot the system
  ```
  reboot
  ```
