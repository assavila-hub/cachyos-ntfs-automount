Com certeza! Aqui estão os campos exatos em inglês para você preencher na criação do seu repositório no GitHub, além da versão em inglês do tutorial para colar no `README.md`.

---

## 1. Como preencher os campos do GitHub

* **Repository name ***:
`cachyos-ntfs-automount` *(ou outro nome claro de sua preferência)*
* **Description**:
`A guide and configuration script to automatically mount NTFS partitions in Arch Linux / CachyOS without password prompts.`
* **Configuration**:
* **Choose visibility ***: `Public`
* **Add README**: **Ative** o botão (*On*) — assim o repositório já é criado com o arquivo onde você vai colar o tutorial.
* **Add .gitignore**: Mantenha em `No .gitignore`
* **Add license**: Opcional (você pode escolher `MIT License` se desejar deixar 100% aberto).



Depois, basta clicar no botão verde **Create repository**.

---

## 2. Conteúdo em Inglês para o `README.md`

Após criar o repositório, clique em editar o arquivo `README.md` e cole o conteúdo abaixo:

```markdown
# 📌 Automatic NTFS Partition Mounting Guide for Arch Linux / CachyOS

This guide explains how to configure your system to automatically mount NTFS partitions (such as secondary Backup drives or Windows 11 partitions) at boot time with full read/write permissions, eliminating system password prompts (`Not authorized to perform operation`).

---

## 🛠️ Step-by-Step Instructions

### 1. Identify the Partition UUID
Open your terminal and list all block devices to locate the **UUID** of your desired partition:

```bash
sudo blkid

```

> **Example output:**
> * Backup Partition: `/dev/sda1` $\rightarrow$ `UUID="01D9E1C76F146790"`
> * Windows 11 Partition: `/dev/nvme0n1p3` $\rightarrow$ `UUID="F424C94A24C91092"`
> 
> 

---

### 2. (Optional) Rename Partition Label

To change the displayed volume label (e.g., from `Basic data partition` to `Windows 11`):

```bash
sudo ntfslabel /dev/nvme0n1p3 "Windows 11"

```

---

### 3. Create Mount Directories

Create the destination folders under `/mnt` where the drives will be accessed:

```bash
sudo mkdir -p /mnt/Backup
sudo mkdir -p "/mnt/Windows 11"

```

---

### 4. Configure Automatic Mounting (`/etc/fstab`)

Edit the system mounts configuration file:

```bash
sudo micro /etc/fstab

```

Add the following lines at the end of the file:

```ini
# Backup Partition
UUID=01D9E1C76F146790  /mnt/Backup       ntfs-3g  defaults,nofail,uid=1000,gid=1000,remove_hiberfile  0  0

# Windows 11 Partition
UUID=F424C94A24C91092  "/mnt/Windows 11"  ntfs-3g  defaults,nofail,uid=1000,gid=1000,remove_hiberfile  0  0

```

#### 💡 Parameter Explanation:

* **`UUID=...`**: Unique and immutable hardware identifier for the partition.
* **`ntfs-3g`**: Driver providing full read/write support for NTFS file systems.
* **`defaults`**: Applies default options (`rw`, `suid`, `dev`, `exec`, `auto`, `nouser`, `async`).
* **`nofail`**: Prevents boot errors if the drive is disconnected.
* **`uid=1000,gid=1000`**: Assigns your user account as the file owner, eliminating root/admin authorization prompts.
* **`remove_hiberfile`**: Allows writing to the partition even if Windows 11 was shut down with Fast Startup enabled.

---

### 5. Test and Apply Mounts

Without rebooting, test the newly added `/etc/fstab` entries:

```bash
sudo mount -a

```

If no errors appear in the terminal, your drives are now mounted at `/mnt/Backup` and `/mnt/Windows 11`.

---

### 6. Adjust File Manager Shortcuts (Dolphin)

1. Open **Dolphin**.
2. Right-click the old dynamic device shortcuts asking for passwords on the left sidebar and select **Hide**.
3. Navigate to `/mnt`.
4. Drag the **`Backup`** and **`Windows 11`** folders to the **Places** section in the left sidebar.

```

```
