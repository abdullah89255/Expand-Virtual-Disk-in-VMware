# Expand-Virtual-Disk-in-VMware

### 🛠️ Step-by-step Guide:

---

## 🧩 PART 1: Expand Virtual Disk in VMware

1. **Shut down** your Kali Linux VM completely (not suspend).
2. Open **VMware Workstation / Player**.
3. Select your Kali VM → click **Edit virtual machine settings**.
4. Go to the **Hard Disk** section.
5. Click **Expand…** → Set **SIZE** → Click **Expand**.
6. Click **OK** to close settings.

You’ve now increased the virtual disk size.

---

## 🧰 PART 2: Resize Partition Inside Kali Linux

1. **Boot the VM** and open a terminal.

2. Check disk:

   ```bash
   lsblk
   ```

   You should see your main disk (e.g., `/dev/sda`) and the main partition (e.g., `/dev/sda1` or `/dev/sda2`).

3. Launch GParted (GUI) or install it if missing:

   ```bash
   sudo apt update && sudo apt install gparted
   sudo gparted
   ```

   OR use **terminal-only option**:

   * If using LVM: `sudo lvextend` + `resize2fs` (I’ll guide if needed).
   * If not using LVM and just `/dev/sda1`: use `fdisk` + `resize2fs`.

---

### Option A: Using GParted (Easiest)

1. In **GParted**, locate your disk (e.g., `/dev/sda`).
2. You’ll see unallocated space.
3. **Right-click** your main partition → **Resize/Move** → drag slider to fill unallocated space.
4. Click **Apply**.
5. Once done, **reboot**.

---

### Option B: Using Terminal (No GParted)

If you're not using LVM:

```bash
sudo fdisk /dev/sda
```

Inside `fdisk`:

* Type `p` to print partition table.
* Note the start sector of `/dev/sda1`.
* Delete the partition (`d`), recreate it (`n`) with same start sector but larger end.
* Write (`w`) and reboot.

Then resize the filesystem:

```bash
sudo resize2fs /dev/sda1
```

---

## ✅ Verify:

```bash
df -h
```

You should now see total size.

---

### 📝 Optional Notes:

* If you’re using **LVM**, let me know—steps differ slightly (you use `lvextend`).
* Always **back up your VM** before modifying partitions.

Would you like help checking if you're using LVM or not?

