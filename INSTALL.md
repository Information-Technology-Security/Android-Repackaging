<p align="center">
  <img src="https://www.especial.gr/wp-content/uploads/2019/03/panepisthmio-dut-attikhs.png" alt="UNIWA" width="150"/>
</p>

<p align="center">
  <strong>UNIVERSITY OF WEST ATTICA</strong><br>
  SCHOOL OF ENGINEERING<br>
  DEPARTMENT OF COMPUTER ENGINEERING AND INFORMATICS
</p>

<p align="center">
  <a href="https://www.uniwa.gr" target="_blank">University of West Attica</a> ·
  <a href="https://ice.uniwa.gr" target="_blank">Department of Computer Engineering and Informatics</a>
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
  <a href=https://scholar.google.com/citations?user=Djium2IAAAAJ&hl=en" target="_blank">Scholar</a> ·
  <a href="https://www.linkedin.com/in/aggelos-georgoulas/?originalSubdomain=uk" target="_blank">LinkedIn</a>
</p>

</hr>

---

<p align="center">
  Athens, June 2023
</p>

---

<p align="center">
  <img src="https://seedsecuritylabs.org/Labs_16.04/Mobile/Android_Repackaging/android_repackaging_m.png" width="250"/>
</p>

---

# INSTALL

## Android Repackaging

This guide describes how to set up the required environment to reproduce the Android Repackaging Attack laboratory exercise using the SEED virtual machines.

---

## 1. Prerequisites

### 1.1 Host System Requirements

- Host OS: Windows / Linux / macOS
- Virtualization Software:
  - Oracle VirtualBox (recommended)
- Minimum Hardware:
  - 8 GB RAM (4 GB minimum)
  - CPU with virtualization enabled (Intel VT-x / AMD-V)
  - ~20 GB free disk space

### 1.2 Virtual Machines Required

Two virtual machines are required and must be connected to the same NAT Network.

#### 1.2.1 Attacker VM

- SEEDUbuntu 16.04 (32-bit)
- Used for:
  - APK disassembly
  - Malicious code injection
  - Repackaging and signing

#### 1.2.2 Victim VM

- SEEDAndroid
- Used for:
  - Installing and executing the repackaged APK
  - Observing malicious behavior

---

## 2. Virtual Machine Setup

### 2.1 Step 1: Import SEEDUbuntu 16.04 (Attacker)

1. Download the SEEDUbuntu 16.04 (32-bit) VM image.
2. Open VirtualBox → File → Import Appliance.
3. Select the SEEDUbuntu .ova file.
4. Assign:
   - RAM: ≥ 2048 MB
   - CPU: ≥ 1 core
5. Import the appliance.

### 2.2 Step 2: Import SEEDAndroid (Victim)

1. Download the SEEDAndroid VM image.
2. Import it into VirtualBox using the same procedure.
3. Do not start the VM yet.

### 2.3 Step 3: Configure Networking (Critical)

Both VMs must be on the same NAT Network.
For each VM:

1. Settings → Network
2. Adapter 1:
   - Attached to: NAT Network
   - Name: e.g. seed-nat

This enables communication between the attacker and victim environments.

---

## 3. Attacker Environment Setup (SEEDUbuntu)

### 3.1 Step 4: Update System Packages

```bash
sudo apt update
sudo apt upgrade
```

### 3.2 Step 5: Install Required Tools

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

## 4. Victim Environment Setup (SEEDAndroid)

### 4.1 Step 6: Enable Developer Options

Inside the SEEDAndroid VM:

- Open Settings
- Go to About Tablet / Phone
- Tap Build Number multiple times to enable Developer Mode
- Enable:
  - USB Debugging
  - Mock Locations (if required for the location attack)

### 4.2 Step 7: Verify ADB Connectivity

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

## 5. Repository Setup

### 5.1 Step 8: Clone the Repository

```bash
git clone https://github.com/Information-Technology-Security/Android-Repackaging.git
cd Android-Repackaging
```

---

## 6. Ready-to-Use Environment

At this point, the environment is fully configured and ready for:

- APK disassembly using apktool
- Smali code modification
- Manifest permission injection
- APK rebuilding and signing
- Deployment to the Android VM via adb

Proceed to the Attack Workflow section of this repository to execute the repackaging scenarios.

---

## 7. Notes & Troubleshooting

- Always sign the rebuilt APK before installation.
- Unsigned or improperly signed APKs will fail to install.
- Ensure both VMs remain on the same NAT Network.
- Use 32-bit SEEDUbuntu only, as required by the lab.

---

## 8. Open the Documentation

1. Navigate to the `docs/` directory
2. Open the report corresponding to your preferred language:
   - English: `Android-Repackaging.pdf`
   - Greek: `Ανασυσκευασία-Εφαρμογών-Android.pdf`
