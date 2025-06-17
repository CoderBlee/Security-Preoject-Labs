#  **NTFS File Deletion Investigation**

Your goal is to analyze a forensic image to recover files that were deleted from an NTFS-formatted drive.

---

##  **Steps You Performed**

### 1. **Preparation**

* You used the ISO: `Student-Resources-L25.ISO`
* Navigated to and extracted `7-undel-ntfs.zip`:

  ```bash
  cd /root/Downloads
  unzip 7-undel-ntfs.zip
  cd 7-undel-ntfs
  ls -l
  ```

  > Revealed the image: `7-ntfs-undel.dd`

---

### 2. **Mounting the Image**

You created a mount point and mounted the image:

```bash
mkdir /mnt/temp7
mount 7-ntfs-undel.dd /mnt/temp7
ls -l /mnt/temp7
```

* **Observation**: Only `System Volume Information` was visible.
* **Conclusion**: Files might have been deleted — recovery required.

---

### 3. **Recovering Deleted Files**

Used `tsk_recover` to attempt recovery:

```bash
tsk_recover 7-ntfs-undel.dd output
```

* **Result**: `Files Recovered: 8`

Checked recovered files:

```bash
ls -l output
```

* **Observation**: You saw some directory structures but not all files immediately.

---

### 4. **Drilling Down into Recovered Directories**

Explored nested directories:

```bash
ls -l output/dir1
ls -l output/dir1/dir2
```

* **Result**: Recovered files became visible.

---

##  **Key Takeaways**

* **Mounting** an NTFS image may not reveal deleted files directly.
* Tools like `tsk_recover` are useful for **undeleting files** from forensic images.
* Even if content is not useful in a test image, in a **real-world case**, metadata (timestamps, names) may point to:

  * Timeline of activity
  * User actions (creation/deletion)
  * Possibly hidden or renamed suspicious files

