# 🔐 Penetration Testing Report: Password Cracking & Hash Auditing Phases

**Modules:** `W3-PM1` & `W3-PM2` | **Program:** Cybersecurity & Ethical Hacking Internship at Networkwalks

---

## 📋 Engagement Metadata

| Field | Details |
| :--- | :--- |
| **Pentester Name** | Tebogo Mokone (Cybersecurity Professional) |
| **Program / Batch** | B084-Networkwalks |
| **Date** | 16 November 2026 |
| **Modules Completed** | • `W3-PM1` (Password Cracking with John the Ripper & Johnny GUI)<br>• `W3-PM2` (Password Cracking with Networkwalks Tools) |
| **Client / Target** | 1. Authorized Lab Encrypted Archives: `My Locked PDF1.pdf`, `My Locked PDF2.pdf`, `My Locked PDF3.pdf` *(W3-PM1)*<br>2. Authorized Lab Encrypted Archive: `My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf` *(W3-PM2)* |
| **Authorization Status** | ✅ Yes (Authorized Networkwalks Cybersecurity Internship Lab Environment) |
| **Phases Covered** | • **Phase 3:** System Access & Credential Auditing (Offline Hash Extraction & Dictionary Password Recovery) |

---

## ⚠️ 1. Liability Disclaimer

> I have performed these password recovery and cryptographic auditing activities strictly on authorized educational lab files provided by Networkwalks where explicit permission was granted. All materials, commands, and techniques documented in this repository are for authorized security auditing, education, and research purposes only. Unauthorized password cracking or decryption of protected files and systems without explicit owner consent is illegal and can result in severe criminal and civil penalties.

---

## 📖 2. Introduction

This repository documents the Week 3 practical deliverables (**W3-PM1** and **W3-PM2**) for the **Networkwalks Cybersecurity Internship**. Both modules evaluate how weak or predictable document passwords can be recovered offline once cryptographic hashes are extracted from password-protected PDF files.

* **W3-PM1:** Utilized **John the Ripper (JTR)** and its graphical interface, **Johnny**, alongside PDF hash extraction utilities on Windows to audit and unlock three protected lab documents.
* **W3-PM2:** Performed browser-based local hash extraction and dictionary cracking inside **Kali Linux** using the **Networkwalks Hash Calculator** and **Networkwalks Password Cracker** tools.

---

## 🛠️ 3. Tools Used

| Tool / Utility | Purpose & Operational Role |
| :--- | :--- |
| **Windows 11 & Kali Linux** | Dual-platform testing environments used to execute local desktop and browser-based cracking workflows |
| **John the Ripper (`1.9.0-jumbo-1`)** | Core command-line password auditing engine supporting PDF cryptographic hash formats (`$pdf$4*4*128...`) |
| **Johnny GUI (`v2.2`)** | Graphical frontend for John the Ripper used to load extracted hash files, run dictionary attacks, and monitor cracked credentials |
| **PDF Hash Extractor (`pdf2john`)** | Extracts the 128-bit encryption metadata and password hash string from locked PDF files into JTR-compatible format |
| **Networkwalks Hash Calculator** | Client-side browser utility (`networkwalks.com/hash-calculator/`) used to locally parse PDF encryption headers into a crackable `$pdf$` hash |
| **Networkwalks Password Cracker** | Client-side dictionary attack utility (`networkwalks.com/password-cracker/`) used to match wordlist candidates against extracted PDF hashes |

---

## 🔍 4. Activities Performed

### 4.1 Module W3-PM1: Password Cracking with John the Ripper & Johnny GUI

For Project Module 1, John the Ripper (`john-1.9.0-jumbo-1-win64`) was configured and linked inside **Johnny GUI** on Windows:

1. **Hash Extraction:** Extracted the PDF encryption hash strings (beginning with `$pdf$4*4*128*-1060...`) from each locked PDF document and saved them as plain-text hash files (e.g., `hash1.txt`).
2. **Attack Execution in Johnny:** Loaded each hash file in Johnny (*Open password file → PASSWD format*) and initiated a dictionary attack against the `PDF` hash format.
3. **Verification & Flag Capture:** Used the recovered plaintext passwords to unlock each PDF file and verify the embedded capture flags.

| Target File | Method / Tool | Recovered Password | Captured Flag |
| :--- | :--- | :--- | :--- |
| `My Locked PDF1.pdf` | JTR + Johnny GUI | `good-luck` | `nw{cybersecurity_flag_captured_2608}` |
| `My Locked PDF2.pdf` | JTR + Johnny GUI | `password1` | `nw{networkwalks_persistence_jtr_270521}` |
| `My Locked PDF3.pdf` | JTR + Johnny GUI | `1qaz2wsx` | `nw{networkwalks_flag_260821_1}` |

---

### 4.2 Module W3-PM2: Password Cracking with Networkwalks Tools

For Project Module 2, the password recovery workflow was executed inside **Kali Linux** using browser-based utilities without installing local cracking binaries:

1. **Local Hash Extraction:** Opened the **Networkwalks Hash Calculator**, selected the PDF parsing tab, and loaded `My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf` (Revision R4, Version V4, 128-bit key length) to extract its `$pdf$` hash locally in the browser.
2. **Dictionary Attack Execution:** Copied the extracted hash into the **Networkwalks Password Cracker** and executed a dictionary attack using the built-in wordlist.
3. **Document Unlock & Verification:** Matched the candidate password (`password1`), unlocked the document in the Kali Linux PDF viewer, and captured Flag 1: `nw{networkwalks_flag1_jtr_270521_1}`.

---

## 📊 5. Risk Analysis & Impact

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
| :---: | :--- | :--- | :--- | :---: |
| **1** | **Use of common dictionary passwords** | `My Locked PDF1.pdf` (PM2) unlocked rapidly with `password1`and `My Locked PDF1.pdf` (PM1) unlocked with `good-luck` | Immediate unauthorized disclosure of confidential document contents upon file leakage | 🔴 **High** |
| **2** | **Predictable keyboard-walk patterns** | `My Locked PDF3.pdf` unlocked using keyboard sequence `1qaz2wsx` | Keyboard patterns are prioritized in standard wordlists and cracked within seconds | 🔴 **High** |
| **3** | **Offline hash extractability** | `pdf2john` and Networkwalks Hash Calculator extracted `$pdf$4*4*128` hashes without rate limits | Once an encrypted file is obtained, attackers can run unlimited offline cracking attempts | 🟠 **Medium** |
| **4** | **Legacy PDF R4 128-bit encryption** | Target PDFs used Revision R4 / V4 128-bit encryption schemes | Lower computational iteration cost allows faster candidate testing per second than modern AES-256 | 🔵 **Low** |

---

## 🛡️ 6. Security Recommendations

1. **Enforce High-Entropy Passphrases:** Replace single dictionary words (`password1`) and keyboard walks (`1qaz2wsx`) and predictable words like (`good-luck`) with random passphrases of at least 14–16 characters mixing uppercase, lowercase, numbers, and symbols.
2. **Use Modern Document Encryption Standards:** Save sensitive PDFs using Acrobat X or later (**AES-256 / Revision 6**) rather than legacy 128-bit Revision 4 encryption to significantly increase key-derivation computational cost.
3. **Avoid Password Reuse Across Files:** Ensure every sensitive archive or document uses a unique password so compromising one file does not expose related documents.
4. **Implement Access Controls & DRM:** Store confidential documents inside access-controlled repositories with multi-factor authentication (MFA) and audit logging rather than relying solely on file-level passwords.

---

## 🏁 7. Conclusion

Completing Week 3 Project Modules 1 and 2 provided practical insight into the mechanics of cryptographic hashing versus reversible encryption. Extracting `$pdf$` hashes from four protected PDF files and cracking them using both desktop tools (John the Ripper and Johnny GUI) and browser-based utilities (Networkwalks Hash Calculator and Password Cracker) demonstrated how quickly weak passwords fall to offline dictionary attacks.

---

## 📸 8. Evidence Collected

> *Tip: Upload your screenshot files into an `evidence/` folder in your GitHub repository to link them directly below.*

### 8.1 Project Module 1 (`W3-PM1`) — John the Ripper & Johnny GUI

<img width="1919" height="1079" alt="JTR Unlock 1" src="https://github.com/user-attachments/assets/4be7b992-3cd9-444e-ba00-7d1c6b5501cd" />

* **Figure 1:** Unlocked My Locked PDF1.pdf revealing Flag 1 (n w{cybersecurity_flag_captured_2608} ) 

<img width="1919" height="1079" alt="JTR Unlock 2" src="https://github.com/user-attachments/assets/925d4449-a7f3-4a6d-82ce-92e92d116096" />

* **Figure 2:** Unlocked My Locked PDF2.pdf revealing Flag 2 (nw{networkwalks_persistence_jtr_270521})

<img width="1919" height="1079" alt="JTR Unlock 3" src="https://github.com/user-attachments/assets/f5774531-8f9b-4c2b-a6c5-cc6efdac7b84" />

* **Figure 3:**  Johnny GUI displaying recovered password 1qaz2wsx (100% cracked) alongside unlocked My Locked PDF3.pdf revealing Flag 3 (nw{networkwalks_flag_260821_1}) 

### 8.2 Project Module 2 (`W3-PM2`) — Networkwalks Tools

<img width="1919" height="1079" alt="NW tool pass crack" src="https://github.com/user-attachments/assets/5914461e-1ac3-4979-ae92-59e43b620e78" />

* **Figure 4 (W3-PM2):** Unlocked My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf inside Kali Linux revealing 
Flag 1 (nw{networkwalks_flag1_jtr_270521_1}) 

---

## 👤 Author & Project Information

* **Author:** Tebogo Mokone — Cybersecurity Professional (Batch B084)
* **LinkedIn:** [linkedin.com/in/tebogo-mokone-ba2b35261](https://www.linkedin.com/in/tebogo-mokone-ba2b35261/)
* **Program:** Cybersecurity & Ethical Hacking Program at Networkwalks | **Week:** 03 (`W3-PM1` & `W3-PM2`) | **Repository:** GitHub
