## Setup Arch Linux on RaspberryPi 3B
Reference: https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-3 (AArch64 Installation)

Pre-requisites:
1. A linux system to follow these instructions on, if not available use VM and allow USB access
2. SD Card and a USB Card reader- Used a 64GB one
3. On linux run `lsblk` command to get the sd-card name shown as USB 

Steps to follow:
Replace sdX in the following instructions with the device name for the SD card as it appears on your computer.

1. Start fdisk to partition the SD card:
  `fdisk /dev/sdX`
2. At the fdisk prompt, delete old partitions and create a new one:
  ```
  a. Type o. This will clear out any partitions on the drive.
  b. Type p to list partitions. There should be no partitions left.
  c. Type n, then p for primary, 1 for the first partition on the drive, press ENTER to accept the default first sector, then type +1G for the last sector.
  d. Type t, then c to set the first partition to type W95 FAT32 (LBA).
  e. Type n, then p for primary, 2 for the second partition on the drive, and then press ENTER twice to accept the default first and last sector.
  f. Write the partition table and exit by typing w.
  ```
3. **Become a root used not using sudo command**
   `su`
5. Create and mount the FAT filesystem:
  ```
  mkfs.vfat /dev/sdX1
  mkdir boot
  mount /dev/sdX1 boot
  ```
5. Create and mount the ext4 filesystem:
  ```
  mkfs.ext4 /dev/sdX2
  mkdir root
  mount /dev/sdX2 root
  ```
6. Download and extract the root filesystem (as root, not via sudo):
  ```
  wget http://os.archlinuxarm.org/os/ArchLinuxARM-rpi-aarch64-latest.tar.gz
  bsdtar -xpf ArchLinuxARM-rpi-armv7-latest.tar.gz -C root
  sync
  ```
7. Move boot files to the first partition:
  `mv root/boot/* boot`
8. Unmount the two partitions:
  `umount boot root`
9. Insert the SD card into the Raspberry Pi, connect ethernet, and apply 5V power.
10. Use the serial console or SSH to the IP address given to the board by your router.
  - Login as the default user alarm with the password alarm.
  - The default root password is root.
11. Initialize the pacman keyring and populate the Arch Linux ARM package signing keys:
  ```
  pacman-key --init
  pacman-key --populate archlinuxarm
  ```
12. Update
    `pacman -Syu`
14. Install sudo, nano, networkmanager
    ```
    paceman -S nano
    pacman -S sudo
    pacman -S networkmanager
    ```
16. Create hostname
    `hostnamectl set-hostname <pi>`
17. Reboot: `reboot`
18. Shutdown: `systemctl poweroff`
19. Diskspace: `df -h`
20. Connect to WiFi
    ```
    systemctl enable --now NetworkManager
    nmcli device
    nmcli device wifi list
    nmcli device wifi connect "WifiName" password "WifiPassword"
    nuclei conne cation show --active
    ```
