<p align="center">
  <img src="https://www.especial.gr/wp-content/uploads/2019/03/panepisthmio-dut-attikhs.png" alt="UNIWA" width="150"/>
</p>

<p align="center">
  <strong>UNIVERSITY OF WEST ATTICA</strong><br>
  SCHOOL OF ENGINEERING<br>
  DEPARTMENT OF COMPUTER ENGINEERING AND INFORMATICS
</p>

<hr/>

<p align="center">
  <strong>Information Technology Security</strong>
</p>

<h1 align="center" style="letter-spacing: 1px;">
  Android Repackaging
</h1>

<p align="center">
  <strong>Vasileios Evangelos Athanasiou</strong><br>
  Student ID: 19390005
</p>

<p align="center">
  <a href="https://github.com/Ath21" target="_blank">GitHub</a> ·
  <a href="https://www.linkedin.com/in/vasilis-athanasiou-7036b53a4/" target="_blank">LinkedIn</a>
</p>

<hr/>

<p align="center">
  <strong>Supervision</strong>
</p>

<p align="center">
  Supervisor: Ioanna Kantzavelou, Associate Professor<br>
</p>

<p align="center">
  <a href="https://ice.uniwa.gr/en/emd_person/ioanna-kantzavelou/" target="_blank">UNIWA Profile</a> ·
  <a href="https://www.linkedin.com/in/ioanna-kantzavelou-74685934/" target="_blank">LinkedIn</a>
</p>


<p align="center">
  Co-supervisor: Angelos Georgoulas, Assistant Professor<br>
</p>

<p align="center">
  <a href=https://scholar.google.com/citations?user=Djium2IAAAAJ&hl=en" target="_blank">UNIWA Profile</a> ·
  <a href="https://www.linkedin.com/in/aggelos-georgoulas/?originalSubdomain=uk" target="_blank">LinkedIn</a>
</p>

</hr>


<p align="center">
  Athens, June 2023
</p>


---

# Project Overview

This laboratory project demonstrates an **Android Repackaging Attack**, a technique in which a legitimate Android application is disassembled, modified to include malicious functionality, and then reassembled and redistributed.  
In this scenario, the injected malicious code is triggered by a system event and is capable of either deleting a victim’s contacts or tracking the victim’s location.

---

## Table of Contents

| Section | Path / File | Description |
|--------:|-------------|-------------|
| 1 | `assign/` | Official laboratory exercise specifications |
| 1.1 | `assign/Exercise 4 (Android Repackaging)_2023.pdf` | Assignment description (English) |
| 1.2 | `assign/Άσκηση 4 (Android Repackaging)_2023.pdf` | Assignment description (Greek) |
| 2 | `docs/` | Project report and analysis |
| 2.1 | `docs/Android-Repackaging.pdf` | Technical report (English) |
| 2.2 | `docs/Ανασυσκευασία-Εφαρμογών-Android.pdf` | Technical report (Greek) |
| 3 | `manuals/` | Reference manuals and lab environment documentation |
| 3.1 | `manuals/AndroidVM.html` | Android virtual machine setup guide |
| 3.2 | `manuals/SEEDAndroid_UserManual.pdf` | SEED Android user manual |
| 3.3 | `manuals/SEEDAndroid_VirtualBox.pdf` | SEED Android VirtualBox configuration |
| 4 | `screens/` | Experimental screenshots and step-by-step evidence |
| 5 | `README.md` | Repository overview and usage notes |

## Environment Setup
The attack is carried out using two virtual machines connected to the same **NAT network**:

- **Attacker Environment:** SEEDUbuntu 16.04 (32-bit)
- **Victim Environment:** SEEDAndroid

---

## Tools Used
- **adb (Android Debug Bridge):** Used to communicate with the Android VM and install applications.
- **apktool:** Used to disassemble and rebuild APK files.
- **keytool & jarsigner:** Used to generate digital keys and sign the modified APK so it can be installed on Android devices.

---

## Attack Workflow

### 1. Application Dismantling
The target application (`RepackagingLab.apk`) is disassembled to access its internal components:

```bash
apktool d RepackagingLab.apk
```
This process produces:
- **smali files**: Compiled Java bytecode representation
- `AndroidManifest.xml`: Application configuration and permissions
- **Resources**: Layouts, assets, and metadata

### 2. Malicious Code Injection
#### Smali Integration:
Malicious code (e.g., `MaliciousCode.smali`) is inserted into the application’s directory structure (such as `/smali/com`).

#### Manifest Modification:
The `AndroidManifest.xml` file is altered to:
- Request unauthorized permissions (e.g., `READ_CONTACTS`, `WRITE_CONTACTS`)
- Register a custom BroadcastReceiver

#### Trigger Event:
The malicious logic is configured to execute when a `TIME_SET` broadcast event occurs (i.e., when the system time is changed).

### 3. Reassembling and Signing
After modifications, the application must be rebuilt and digitally signed:
```bash
apktool b RepackagingLab
```
```bash
keytool -genkey -v -keystore mykey.keystore -alias dist
```
```bash
jarsigner -keystore mykey.keystore RepackagingLab.apk dist
```

Signing is required because Android will not install unsigned or tampered applications.

### 4. Execution and Verification
The signed malicious APK is installed on the victim device using:
```bash
adb install RepackagingLab.apk
```

Once the victim grants the requested permissions and a system time change occurs, the registered BroadcastReceiver activates and executes the injected smali code.

---

## Scenarios Covered
### Contact Deletion
Upon the `TIME_SET` event, the malicious code deletes all contacts stored on the victim’s device.

### Location Tracking
The malicious code retrieves the victim’s GPS coordinates (simulated via MockLocation) and sends them to an attacker-controlled web server.

---

## Conclusion

This lab highlights how Android repackaging attacks exploit user trust in legitimate applications. By modifying application code and abusing system broadcasts and permissions, attackers can introduce severe privacy and security threats. The exercise emphasizes the importance of application integrity checks, trusted distribution channels, and careful permission management in Android environments.

---

# Installation & Setup Guide

This guide describes how to set up the required environment to reproduce the Android Repackaging Attack laboratory exercise using the SEED virtual machines.

## Prerequisites
### 1. Host System Requirements
- Host OS: Windows / Linux / macOS
- Virtualization Software:
  - Oracle VirtualBox (recommended)
- Minimum Hardware:
  - 8 GB RAM (4 GB minimum)
  - CPU with virtualization enabled (Intel VT-x / AMD-V)
  - ~20 GB free disk space

### 2. Virtual Machines Required
Two virtual machines are required and must be connected to the same NAT Network.

#### Attacker VM
- SEEDUbuntu 16.04 (32-bit)
- Used for:
  - APK disassembly
  - Malicious code injection
  - Repackaging and signing

#### Victim VM
- SEEDAndroid
- Used for:
  - Installing and executing the repackaged APK
  - Observing malicious behavior

---

## Virtual Machine Setup
### Step 1: Import SEEDUbuntu 16.04 (Attacker)
1. Download the SEEDUbuntu 16.04 (32-bit) VM image.
2. Open VirtualBox → File → Import Appliance.
3. Select the SEEDUbuntu .ova file.
4. Assign:
    - RAM: ≥ 2048 MB
    - CPU: ≥ 1 core
5. Import the appliance.

### Step 2: Import SEEDAndroid (Victim)
1. Download the SEEDAndroid VM image.
2. Import it into VirtualBox using the same procedure.
3. Do not start the VM yet.

### Step 3: Configure Networking (Critical)
Both VMs must be on the same NAT Network.
For each VM:
1. Settings → Network
2. Adapter 1:
    - Attached to: NAT Network
    - Name: e.g. seed-nat

This enables communication between the attacker and victim environments.

---

## Attacker Environment Setup (SEEDUbuntu)
### Step 4: Update System Packages
```bash
sudo apt update
sudo apt upgrade
```

### Step 5: Install Required Tools
1. Java Development Kit (JDK)
```bash
sudo apt install openjdk-8-jdk
```
Verify:
```bash
java --version
```
2. apktool
```bash
sudo apt install apktool
```
Verify:
```bash
apktool
```
3. Android Debug Bridge (adb)
```bash
sudo apt install adb
```
Verify:
```bash
adb version
```
4. Keytool & Jarsigner
These are included with the JDK installation.
Verify:
```bash
keytool -help
jarsigner -help
```

---

## Victim Environment Setup (SEEDAndroid)
### Step 6: Enable Developer Options
Inside the SEEDAndroid VM:
- Open Settings
- Go to About Tablet / Phone
- Tap Build Number multiple times to enable Developer Mode
- Enable:
  - USB Debugging
  - Mock Locations (if required for the location attack)

### Step 7: Verify ADB Connectivity
On SEEDUbuntu:
```bash
adb devices
```
You should see the SEEDAndroid device listed.
If not:
```bash
adb kill-server
adb start-server
```

---

## Repository Setup
### Step 8: Clone the Repository
```bash
git clone https://github.com/Information-Technology-Security/Android-Repackaging.git
cd Android-Repackaging
```

---

## Ready-to-Use Environment
At this point, the environment is fully configured and ready for:
- APK disassembly using apktool
- Smali code modification
- Manifest permission injection
- APK rebuilding and signing
- Deployment to the Android VM via adb

Proceed to the Attack Workflow section of this repository to execute the repackaging scenarios.

---

## Notes & Troubleshooting
- Always sign the rebuilt APK before installation.
- Unsigned or improperly signed APKs will fail to install.
- Ensure both VMs remain on the same NAT Network.
- Use 32-bit SEEDUbuntu only, as required by the lab.

---

## Open the Documentation
1. Navigate to the `docs/` directory
2. Open the report corresponding to your preferred language:
    - English: `Android-Repackaging.pdf`
    - Greek: `Ανασυσκευασία-Εφαρμογών-Android.pdf`


