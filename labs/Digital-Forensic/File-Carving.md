# ** File Carving from a Damaged FAT Image**

You were given a corrupted FAT disk image suspected of containing destroyed or hidden evidence. Standard methods failed, so you used **manual file carving** with `testdisk`.

---

##  **Step-by-Step Summary**

### 1. **Initial Setup & Extraction**

```bash
cd /root/Downloads
unzip 11-carve-fat.zip
cd 11-carve-fat
ls -l
```

* Revealed the corrupted image: `11-carve-fat.dd`

---

### 2. **Mount Attempt Fails**

```bash
mkdir /mnt/temp11
mount 11-carve-fat.dd /mnt/temp11
```

*  **Result**: Mount fails — possible corruption or partition table issues.

---

### 3. **Partition/Filesystem Tools Fail**

```bash
fdisk -l 11-carve-fat.dd
```

* Disk ID is all zeros; no partition table found.

```bash
fiwalk 11-carve-fat.dd
```

* Numerous errors: **Possible encryption detected**.

```bash
fsstat -f fat16 11-carve-fat.dd
```

* Error: **Invalid magic value**

```bash
mmls 11-carve-fat.dd
```

* No partition layout data found.

---

##  **Solution: Use `testdisk` for File Carving**

### Step-by-Step Interactive Actions:

1. **Create Output Directory:**

   ```bash
   mkdir output
   ```

2. **Launch TestDisk:**

   ```bash
   testdisk 11-carve-fat.dd
   ```

3. **TestDisk Interactive Recovery:**

   * Select `[Proceed]`
   * Choose `[None]` (no partition table)
   * Choose `[FAT16]` partition type
   * `[Undelete]` → ❌ No files found
   * `[Boot]` → `[Rebuild BS]` → success message
   * `[List]` now shows files 

4. **Recover Files from Memory-Repaired FS:**

   * Press `a` → select all files
   * Press `C` (capital C) → copy
   * Navigate to `output` directory → press `C` again to confirm copy
   *  Message: **Copy done! 15 OK, 0 failed**

5. **Exit TestDisk:**

   ```bash
   CTRL+C
   ```

---

##  **View Recovered Files**

```bash
cd output
ls -l
```

* Shows all **15 recovered files**.

To view specific files:

```bash
xdg-open haxor2.jpg
xdg-open lin_1.2.pdf
xdg-open surf.mov
unzip wword60t.zip
```

---

##  **Key Lessons Learned**

* Filesystems can appear destroyed but still contain recoverable data via headers, file signatures, and low-level tools.
* **File carving** works independently of a file system by scanning raw data blocks for recognizable file structures.
* Tools like `testdisk`, `photorec`, `scalpel`, and `foremost` are invaluable for deep recovery.


