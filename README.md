# 📌 Automatic NTFS Partition Mounting Guide for Arch Linux / CachyOS

This guide explains how to configure your system to automatically mount NTFS partitions (such as secondary Backup drives or Windows 11 partitions) at boot time with full read/write permissions, eliminating system password prompts (`Not authorized to perform operation`).

---

## 🛠️ Step-by-Step Instructions

### 1. Identify the Partition UUID
Open your terminal and list all block devices to locate the **UUID** of your desired partition:

  sudo blkid

> **Example output:**
> * Backup Partition: `/dev/sda1` -> `UUID="01D9E1C76F146790"`
> * Windows 11 Partition: `/dev/nvme0n1p3` -> `UUID="F424C94A24C91092"`

---

### 2. (Optional) Rename Partition Label
To change the displayed volume label in your system (e.g., changing from `Basic data partition` to `Windows 11`):

  sudo ntfslabel /dev/nvme0n1p3 "Windows 11"

---

### 3. Create Mount Directories
Create the destination folders under `/mnt` where the drives will be accessed:

  sudo mkdir -p /mnt/Backup
  sudo mkdir -p "/mnt/Windows 11"

---

### 4. Configure Automatic Mounting (`/etc/fstab`)
Edit the system mounts configuration file:

  sudo micro /etc/fstab

Add the following lines at the end of the file:

  # Backup Partition
  UUID=01D9E1C76F146790  /mnt/Backup       ntfs-3g  defaults,nofail,uid=1000,gid=1000,remove_hiberfile  0  0

  # Windows 11 Partition
  UUID=F424C94A24C91092  "/mnt/Windows 11"  ntfs-3g  defaults,nofail,uid=1000,gid=1000,remove_hiberfile  0  0

#### 💡 Parameter Breakdown:
* **`UUID=...`**: Unique hardware identifier for the partition.
* **`ntfs-3g`**: Driver providing full read and write support for NTFS file systems.
* **`defaults`**: Applies default mount options (`rw`, `suid`, `dev`, `exec`, `auto`, `nouser`, `async`).
* **`nofail`**: Prevents system boot failures if a drive is disconnected or missing.
* **`uid=1000,gid=1000`**: Assigns your user account as the owner of all files, preventing administrative password prompts.
* **`remove_hiberfile`**: Enables read/write access even if Windows 11 was shut down with Fast Startup enabled.

---

### 5. Test and Apply Mounts
Without rebooting, test the newly added `/etc/fstab` entries:

  sudo mount -a

If no errors appear in the terminal, your drives are now mounted at `/mnt/Backup` and `/mnt/Windows 11`.

---

### 6. Adjust File Manager Shortcuts (Dolphin)
1. Open **Dolphin**.
2. Right-click the old dynamic device shortcuts that prompt for passwords on the left sidebar and select **Hide**.
3. Navigate to `/mnt`.
4. Drag the **`Backup`** and **`Windows 11`** folders to the **Places** section in the left sidebar.
