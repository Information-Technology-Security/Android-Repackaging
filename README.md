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

# README

## Android Repackaging

This laboratory project demonstrates an **Android Repackaging Attack**, a technique in which a legitimate Android application is disassembled, modified to include malicious functionality, and then reassembled and redistributed.  
In this scenario, the injected malicious code is triggered by a system event and is capable of either deleting a victim’s contacts or tracking the victim’s location.

---

## Table of Contents

| Section | Path / File                                        | Description                                         |
| ------: | -------------------------------------------------- | --------------------------------------------------- |
|       1 | `assign/`                                          | Official laboratory exercise specifications         |
|     1.1 | `assign/Exercise 4 (Android Repackaging)_2023.pdf` | Assignment description (English)                    |
|     1.2 | `assign/Άσκηση 4 (Android Repackaging)_2023.pdf`   | Assignment description (Greek)                      |
|       2 | `docs/`                                            | Project report and analysis                         |
|     2.1 | `docs/Android-Repackaging.pdf`                     | Technical report (English)                          |
|     2.2 | `docs/Ανασυσκευασία-Εφαρμογών-Android.pdf`         | Technical report (Greek)                            |
|       3 | `manuals/`                                         | Reference manuals and lab environment documentation |
|     3.1 | `manuals/AndroidVM.html`                           | Android virtual machine setup guide                 |
|     3.2 | `manuals/SEEDAndroid_UserManual.pdf`               | SEED Android user manual                            |
|     3.3 | `manuals/SEEDAndroid_VirtualBox.pdf`               | SEED Android VirtualBox configuration               |
|       4 | `screens/`                                         | Experimental screenshots and step-by-step evidence  |
|       5 | `README.md`                                        | Project documentation                               |
|       6 | `INSTALL.md`                                       | Usage instructions                                  |

---

## 1. Environment Setup

The attack is carried out using two virtual machines connected to the same **NAT network**:

- **Attacker Environment:** SEEDUbuntu 16.04 (32-bit)
- **Victim Environment:** SEEDAndroid

---

## 2. Tools Used

- **adb (Android Debug Bridge):** Used to communicate with the Android VM and install applications.
- **apktool:** Used to disassemble and rebuild APK files.
- **keytool & jarsigner:** Used to generate digital keys and sign the modified APK so it can be installed on Android devices.

---

## 3. Attack Workflow

### 3.1 Application Dismantling

The target application (`RepackagingLab.apk`) is disassembled to access its internal components:

```bash
apktool d RepackagingLab.apk
```

This process produces:

- **smali files**: Compiled Java bytecode representation
- `AndroidManifest.xml`: Application configuration and permissions
- **Resources**: Layouts, assets, and metadata

### 3.2 Malicious Code Injection

#### 3.2.1 Smali Integration:

Malicious code (e.g., `MaliciousCode.smali`) is inserted into the application’s directory structure (such as `/smali/com`).

#### 3.2.2 Manifest Modification:

The `AndroidManifest.xml` file is altered to:

- Request unauthorized permissions (e.g., `READ_CONTACTS`, `WRITE_CONTACTS`)
- Register a custom BroadcastReceiver

#### 3.2.3 Trigger Event:

The malicious logic is configured to execute when a `TIME_SET` broadcast event occurs (i.e., when the system time is changed).

### 3.3 Reassembling and Signing

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

---

## 4. Execution and Verification

The signed malicious APK is installed on the victim device using:

```bash
adb install RepackagingLab.apk
```

Once the victim grants the requested permissions and a system time change occurs, the registered BroadcastReceiver activates and executes the injected smali code.

---

## 5. Scenarios Covered

### 5.1 Contact Deletion

Upon the `TIME_SET` event, the malicious code deletes all contacts stored on the victim’s device.

### 5.2 Location Tracking

The malicious code retrieves the victim’s GPS coordinates (simulated via MockLocation) and sends them to an attacker-controlled web server.

---

## 6. Conclusion

This lab highlights how Android repackaging attacks exploit user trust in legitimate applications. By modifying application code and abusing system broadcasts and permissions, attackers can introduce severe privacy and security threats. The exercise emphasizes the importance of application integrity checks, trusted distribution channels, and careful permission management in Android environments.
