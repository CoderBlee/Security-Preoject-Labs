
# **Performing Digital Forensics: Hidden Partition Discovery and Analysis**

---

##  **Objective**

The goal of this lab is to conduct post-incident forensic analysis of a compromised system by inspecting a raw forensic drive image. The exercise demonstrates how to:

* Discover hidden partitions,
* Analyze partition structures,
* Recover deleted files,
* Perform file carving,
* Understand partition metadata and filesystem information.

---

##  **Lab Environment Setup**

* **Virtual Machine**: Kali Linux (KALI VM)

* **Tools Used**:

  * `fdisk`
  * `testdisk`
  * `fiwalk`
  * `fsstat`, `istat`, `mmls`, `fls` (from The Sleuth Kit - TSK)
  * `losetup`, `mount`, `unzip`

* **Resources**:

  * ISO Image: `Student-Resources-L25.ISO`
  * Forensic Image: `ext-part-test-2.dd` (extracted from `1-extend-part.zip`)
  * Repository: [http://dftt.sourceforge.net/](http://dftt.sourceforge.net/)

---

##  **Step-by-Step Forensic Process**

---

###  1. **Mount ISO and Extract Forensic Files**

* Mount the provided ISO:

  ```bash
  mount /media/cdrom0/
  ```
* Copy files to Downloads:

  ```bash
  cp /media/cdrom0/* /root/Downloads/
  ```
* Unzip the forensic archive:

  ```bash
  unzip 1-extend-part.zip
  ```

---

###  2. **Initial Partition Analysis Using `fdisk`**

* View partition layout:

  ```bash
  fdisk -l ext-part-test-2.dd
  ```
* Observations:

  * 4th partition is of **type Extended**.
  * Indicates additional **logical volumes** within.

> **Importance**: `fdisk` gives a high-level view of the disk structure. Extended partitions often hide logical drives attackers may exploit.

---

###  3. **Deeper Partition Discovery with `testdisk`**

* Analyze hidden partitions:

  ```bash
  testdisk -l ext-part-test-2.dd
  ```
* Output shows **7 entries**, one more than `fdisk` revealed.

> **Importance**: `testdisk` can detect deeper-level partitions, even those missed or hidden from the OS. Useful in uncovering suspicious modifications.

---

###  4. **File System Analysis with `fiwalk`**

* List and browse files:

  ```bash
  fiwalk ext-part-test-2.dd | less
  ```

> **Importance**: `fiwalk` provides insight into partition and file layout, helping you map inodes and file contents systematically.

---

###  5. **Using `fsstat` and `mmls` for Filesystem Details**

* Basic fsstat fails:

  ```bash
  fsstat ext-part-test-2.dd
  ```
* Use `mmls` to find the correct offset:

  ```bash
  mmls ext-part-test-2.dd
  ```
* Re-run `fsstat` using offset (e.g., `262143` for partition 6):

  ```bash
  fsstat -f fat16 ext-part-test-2.dd -o 262143
  ```

> **Importance**: Essential for identifying filesystem types and navigating non-standard or hidden partitions. Helps detect tampering.

---

###  6. **Inode Inspection with `istat`**

* Inspect individual inodes:

  ```bash
  istat -f fat16 ext-part-test-2.dd -o 262143 2
  istat -f fat16 ext-part-test-2.dd -o 262143 3
  ...
  ```

* Inode 3 shows `second-3.txt`

* Inode 4 shows `SECOND-3.txt` (deleted or corrupted)

> **Importance**: Direct inode analysis reveals file creation, deletion, and metadata — useful in understanding attacker behavior or file tampering.

---

###  7. **Mounting the Hidden Partition**

* Create loop device:

  ```bash
  losetup --partscan --find --show ext-part-test-2.dd
  ```
* Create mount point:

  ```bash
  mkdir /mnt/p6
  ```
* Mount hidden volume:

  ```bash
  mount /dev/loop0p7 /mnt/p6
  ```
* View contents:

  ```bash
  ls -l /mnt/p6
  ```

> **Importance**: Mounting enables live access to file contents within hidden partitions. It's a critical step in any forensic chain-of-evidence workflow.

---

##  **Results Summary**

* The image contained a hidden partition (Partition 6 / `/dev/loop0p7`) inside an extended partition.
* It held a file: `second-3.txt`, confirming successful recovery.
* Another file (`SECOND-3.txt`) was likely deleted and unrecoverable.
* Partition structure and inode metadata indicated potential tampering or concealment.

---

##  **Key Forensic Concepts Demonstrated**

| Concept                   | Description                                                                                  |
| ------------------------- | -------------------------------------------------------------------------------------------- |
| **Extended Partition**    | Used to bypass the 4-partition MBR limit, often manipulated for hiding data.                 |
| **Hidden Partition**      | A logical volume not revealed in standard tools like `fdisk`, possibly created by intruders. |
| **Inodes**                | Contain metadata about files; helpful in reconstructing activity history.                    |
| **File System Analysis**  | `fiwalk`, `fsstat`, and `mmls` uncover low-level details inaccessible through GUI.           |
| **Mounting Loop Devices** | Critical for inspecting raw image contents safely without altering them.                     |

---

##  **Why This Matters**

Performing digital forensics on drive images is an essential skill in cybersecurity investigations. This lab:

* Demonstrates core forensic analysis techniques.
* Reinforces the need for multiple tools to discover concealed data.
* Emphasizes how attackers can use extended partitions to hide evidence.
* Highlights the role of metadata and filesystem knowledge in reconstructing incidents.

---

##  **Tools Summary**

| Tool                      | Purpose                                    |
| ------------------------- | ------------------------------------------ |
| `fdisk`                   | Lists partitions (basic)                   |
| `testdisk`                | Detects lost/hidden partitions             |
| `fiwalk`                  | Filesystem walker, maps contents           |
| `fsstat`, `mmls`, `istat` | Sleuth Kit tools for detailed FS analysis  |
| `losetup`, `mount`        | Creates virtual devices and mounts volumes |

---

##  **Conclusion**

This lab provides a practical approach to uncovering hidden partitions and analyzing filesystem artifacts in forensic images. It reinforces investigative thinking, tool usage, and forensic rigor — critical in real-world cybersecurity operations.

