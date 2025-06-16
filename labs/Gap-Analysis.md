
#  Perform System Configuration Gap Analysis

A gap analysis compares the current configuration of a system to a known security baseline—such as those provided by Microsoft—to identify any differences or misconfigurations.

This process is important because it helps:
1. Detect security weaknesses,
2. Ensure compliance with standards and policies,
3. Guide necessary system hardening steps.

In this lab, you’ll perform a gap analysis on a Windows Server 2019 system using Microsoft Policy Analyzer. You'll compare the system’s current settings to the official Microsoft security baseline and identify areas that require adjustment to improve security

##  **Objectives**

1. Understand and explain the purpose of a gap analysis.
2. Identify current OS version and build.
3. Utilize Microsoft Policy Analyzer to review baseline security policies.
4. Compare current system settings with a baseline configuration.
5. Document all non-compliant settings.
6. Reflect on potential remediation strategies.
7. Understand the importance of customizing baselines to fit business needs.

---

##  **Tools Required**

* Microsoft Windows Server 2019 (PC10 VM)
* PowerShell (Admin)
* Microsoft Security Compliance Toolkit

  * PolicyAnalyzer.zip
  * Baseline ZIP file (Windows 10 v1809 and Windows Server 2019)
* `winver` command
* Microsoft documentation for group policy and security baselines

---

##  Step-by-Step Guide

###  Step 1: Define Gap Analysis

**Definition:**
Gap analysis in cybersecurity involves comparing a system's current configuration against a known security baseline to identify misconfigurations or deviations.

**Purpose:**

* Enforce security controls
* Identify vulnerabilities
* Ensure compliance (e.g., CIS, NIST, ISO)
* Prepare for audits

---

###  Step 2: Determine the System Version and Build

1. Click **"Type here to search"** on the Windows taskbar.
2. Type `winver` and press **Enter**.
3. In the **About Windows** window:

   * Confirm the **version is 1809**
   * Confirm the **OS build is 17763.4377**

 **Why this matters:**
Baselines are OS version-specific; using the wrong one yields inaccurate comparisons.

---

###  Step 3: Launch PowerShell as Administrator

1. Open PowerShell via the search bar.
2. Right-click it and choose **"Run as administrator"**.
3. Select **Yes** when prompted by User Account Control (UAC).

---

###  Step 4: Copy Files from Virtual Optical Drive

Run:

```powershell
copy D:\* C:\LABFILES
```

This transfers:

* `PolicyAnalyzer.zip`
* `Windows 10 Version 1809 and Windows Server 2019 Security Baseline.zip`

Change to the working directory:

```powershell
cd C:\LABFILES
ls
```

---

###  Step 5: Extract the Toolkit Contents

```powershell
Expand-Archive -Path PolicyAnalyzer.zip
Expand-Archive -Path "Windows 10 Version 1809 and Windows Server 2019 Security Baseline.zip"
```

---

###  Step 6: Launch the Policy Analyzer Tool

```powershell
C:\LABFILES\PolicyAnalyzer\PolicyAnalyzer_40\PolicyAnalyzer.exe
```

* Maximize the Policy Analyzer window.
* If necessary, increase the screen resolution for better visibility.

---

###  Step 7: Load the Baseline Policy

1. Click **“Policy Rules sets in”**.
2. Navigate to:

```
C:\LABFILES\Windows 10 Version 1809 and Windows Server 2019 Security Baseline\Documentation
```

3. Click **Select Folder**.

---

###  Step 8: View the Baseline Configuration

1. Select `MSFT-Win10-v1809-RS5-WS2019-FINAL`.
2. Click **View / Compare**.
3. Observe values such as:

   * `LockoutBadCount`: **Expected = 10**
   * `MinimumPasswordLength`: **Expected = 14**
4. Close the viewer.

---

###  Step 9: Perform Gap Analysis (Compare to Effective State)

1. Again, select `MSFT-Win10-v1809-RS5-WS2019-FINAL`.
2. Click **Compare to Effective State**.
3. Approve UAC if prompted.
4. Observe:

   * `LockoutBadCount`: **Actual = 0**
   * `MinimumPasswordLength`: **Actual = 7**

Yellow highlights indicate mismatches (non-compliance).

---

##  Additional Steps (New Content Below)

---

###  Step 10: Export the Analysis Results (Optional but Recommended)

If available, export the results:

1. In Policy Analyzer, click **File → Export Results**.
2. Save as a CSV or HTML file.
3. This provides documentation for audits or remediation planning.

---

###  Step 11: Review Critical Security Deviations

Key areas to check:

* **Account lockout policies**
* **Password policies**
* **Audit policies**
* **User rights assignments**
* **Security options**

💡 Tip: Sort or filter by column to prioritize **critical or high-impact** gaps.

---

###  Step 12: Plan Remediation Strategy

For each non-compliant policy:

* Describe the risk
* Propose a fix
* Recommend whether to adopt the baseline or customize

 Example Table for Report:

| Policy Name           | Baseline Value | Actual Value | Risk Level | Recommended Action         |
| --------------------- | -------------- | ------------ | ---------- | -------------------------- |
| LockoutBadCount       | 10             | 0            | High       | Configure via Group Policy |
| MinimumPasswordLength | 14             | 7            | Medium     | Increase to 14             |

---

###  Step 13: Customize the Baseline

**Why?**
Not every organization has the same risk profile. You should evaluate:

* Legal/regulatory needs (e.g., HIPAA, PCI-DSS)
* Internal policies
* Operational tolerance for change

 **Best Practice:**
Involve IT leadership in defining which baseline settings to modify or leave as-is.

---

###  Step 14: Schedule Periodic Reviews

* Perform this analysis **quarterly** or after major updates.
* Include it in **change management** and **patch management** cycles.

---

##  Reflection and Analysis

### What Did You Learn?

* How to use Microsoft's tools to conduct a security configuration review.
* How even minor configuration differences (e.g., 7 vs. 14 character passwords) can have large security implications.
* Why it's essential to tailor security baselines to your organization.

### What Could Go Wrong?

* Using the wrong OS version’s template = misleading results.
* Failing to follow up on remediation = lingering vulnerabilities.
* Blindly applying templates = breaking needed system functions.

---

##  Deliverables

You may be required to submit the following:

1. Screenshot or report of the Policy Analyzer “Compare to Effective State”
2. A table of 5+ non-compliant settings with recommended actions
3. A short written reflection (1 paragraph)
4. (Optional) Exported Policy Analyzer results (CSV or HTML)

---

##  Conclusion

Conducting a configuration gap analysis is a **low-cost, high-value** exercise that improves security, enforces compliance, and increases operational confidence in system hardening. This hands-on experience with Microsoft Policy Analyzer is an essential step toward building a secure and compliant infrastructure.

