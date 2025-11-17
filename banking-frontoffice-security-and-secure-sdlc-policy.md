# banking-frontoffice-security-and-secure-sdlc-policy.md
**Versi:** 1.0  
**Status:** Approved  
**Klasifikasi:** Internal – Confidential  

---

# 1. Pendahuluan

Dokumen ini merupakan kebijakan komprehensif yang mengatur keamanan perangkat front-office, keamanan aplikasi, Secure Software Development Lifecycle (Secure SDLC), serta Security by Design (SbD) di lingkungan perbankan.

Tujuan dokumen ini adalah untuk memastikan bahwa seluruh aplikasi dan perangkat yang digunakan nasabah atau frontliner bank:

- aman dari OS escape, gesture exploit, dan file system exposure  
- tidak menyimpan data sensitif secara lokal  
- berjalan pada perangkat yang ter-hardening dengan domain join, GPO, dan EDR  
- mematuhi standar dan regulasi perbankan  
- mengikuti Secure SDLC dan Security by Design sejak tahap perencanaan  

---

# 2. Dasar Regulasi & Standar Referensi

## 2.1 Regulasi Indonesia (OJK & BI)
- POJK 38/POJK.03/2016 – Manajemen Risiko TI  
- SEOJK 29/SEOJK.03/2022 – Keamanan Sistem Informasi Perbankan  
- POJK 11/POJK.03/2022 – Transformasi Digital Perbankan  
- SE BI No. 9/30/DPNP – Risiko TI  
- UU Perlindungan Data Pribadi (UU PDP)

## 2.2 Standar Internasional
- NIST SP 800-218 – Secure Software Development Framework (SSDF)  
- NIST SP 800-53 Rev5 – Security & Privacy Controls  
- NIST SP 800-160 – Systems Security Engineering  
- NIST Cybersecurity Framework 2.0  
- ISO/IEC 27001:2022 (A.8, A.8.7, A.8.9, A.14)  
- CIS Controls v8 – Control 4, 10, 16  
- PCI DSS v4.0  
- FFIEC IT Examination Handbook  

---

# 3. Lingkup Kebijakan

Kebijakan ini berlaku untuk:

- seluruh aplikasi front-office, teller, dan onboarding  
- perangkat AIO PC, teller workstation, kiosk, CS terminal  
- vendor aplikasi dan vendor perangkat  
- seluruh proses internal Secure SDLC  

---

# 4. Kebijakan Keamanan Perangkat Front-Office

## 4.1 Domain Join (Mandatory)
Seluruh perangkat wajib:

- join ke Active Directory Domain  
- ditempatkan pada OU *Front-Office Workstation*  
- mengikuti seluruh GPO lockdown  

Perangkat non-domain dianggap **non-compliant**.

---

## 4.2 Hardening Perangkat (Critical)

Perangkat wajib mengikuti:

- Kiosk Mode / Assigned Access  
- Fullscreen lock tanpa minimize  
- Disable OS gesture (swipe kiri/kanan/atas/bawah)  
- Disable ALT+TAB, Windows key, CTRL+ALT+DEL  
- Disable Task View, Action Center, File Explorer  
- Disable CMD, PowerShell, Registry Editor  
- Disable USB storage  
- BitLocker aktif  
- Dilarang menggunakan local admin  

Referensi: **CIS Benchmarks, NIST CM-7, ISO 27001 A.8.9**

---

## 4.3 Endpoint Security (Mandatory EDR)

Perangkat wajib memiliki:

- EDR aktif dengan tamper protection  
- monitoring terhubung ke SIEM  
- auto-block untuk aktivitas mencurigakan  
- aplikasi front-office tidak boleh berjalan jika EDR nonaktif  

---

## 4.4 Penyimpanan Data Sensitif

Dilarang menyimpan data sensitif pada perangkat, termasuk:

- foto nasabah  
- biometrik  
- dokumen identitas  
- konfigurasi rahasia aplikasi  
- log sensitif  

Foto dan data hanya boleh diproses **in-memory**.

Referensi: **UU PDP, NIST SC-28**

---

# 5. Kebijakan Keamanan Aplikasi Front-Office

## 5.1 Secure Front-Office Mode

Aplikasi harus:

- berjalan fullscreen  
- tidak dapat di-minimize  
- tidak spawn window OS  
- tidak memiliki akses file system  
- auto-clear sensitive data  

---

## 5.2 OS Escape Prevention

Aplikasi tidak boleh dieksploitasi melalui:

- gesture touchscreen  
- hotkey  
- context menu  
- WebView escape  
- file upload dialog bypass  

Jika gagal satu test → aplikasi **tidak boleh go-live**.

---

## 5.3 Konfigurasi Aman

- API key / secret harus dienkripsi  
- tidak ada default password  
- tidak ada debug menu  
- TLS 1.2/1.3 mandatory  

---

# 6. Kebijakan untuk Vendor Perangkat (AIO/Kiosk)

Vendor wajib memastikan bahwa perangkat:

- kompatibel dengan domain join dan GPO  
- kompatibel dengan EDR  
- memiliki BIOS/UEFI lock  
- bebas bloatware  
- lulus hardening compatibility test  
- lulus OS escape & gesture bypass test  

---

# 7. Pengujian Keamanan (Aplikasi & Perangkat)

## 7.1 Pengujian Aplikasi
- SAST  
- DAST  
- SCA  
- Penetration Testing  
- Threat Modeling  
- Secure UAT  
- Privacy Compliance Test  

## 7.2 Pengujian Perangkat
- OS escape test  
- gesture bypass test  
- hotkey bypass test  
- USB rogue device test  
- thumbnail/residue test  
- BIOS bypass test  

## 7.3 End-to-End Testing
Dilakukan pada perangkat AIO vendor dengan:

- domain join  
- GPO aktif  
- EDR aktif  

---

# 8. Secure SDLC Governance

## 8.1 Executive Sponsor (CTO/Head of ICT)
- menyetujui kebijakan  
- menyediakan anggaran  
- keputusan risiko strategis  

## 8.2 Business Owner
- menyusun BRD  
- melakukan UAT  
- pemilik risiko  
- approver go-live  

## 8.3 IT Solution / Development Oversight
- arsitektur sistem  
- oversight vendor  
- design review  

## 8.4 IT Security (CISO Office)
- Security Requirements  
- Threat Modeling  
- SAST/DAST/Pentest  
- Security Sign-Off  
- environment validation  

## 8.5 IT Operations
- deployment environment  
- domain join & GPO  
- EDR deployment  
- production deployment  

## 8.6 Vendor Management / Legal
- security addendum  
- SLA & penalti  
- compliance vendor  

## 8.7 QA/UAT
- functional testing  
- security UAT  

## 8.8 Risk Management
- risk assessment  
- residual risk review  

## 8.9 Internal Audit
- audit berkala  
- audit SSDLC  
- rekomendasi perbaikan  

---

# 9. RACI Matrix – Secure SDLC

| SSDLC Activity                  | BO | IT Sol | IT Sec | IT Ops | QA | Vendor Dev | Vendor Mgt | Risk | Audit |
|--------------------------------|----|--------|--------|--------|----|-------------|-------------|------|--------|
| Planning                       | A  | R      | C      | I      | I  | C           | I           | C    | I      |
| Security Requirements          | C  | C      | A/R    | I      | I  | C           | I           | C    | I      |
| Architecture & Design Review   | C  | A/R    | R      | C      | I  | C           | I           | C    | I      |
| Threat Modeling                | I  | C      | A/R    | C      | I  | C           | I           | C    | I      |
| Development                    | I  | C      | C      | I      | I  | A/R         | I           | I    | I      |
| SAST/SCA Review                | I  | C      | A/R    | I      | I  | R           | I           | I    | I      |
| DAST/Pentest                   | I  | C      | A/R    | I      | I  | R           | I           | C    | I      |
| UAT Functional                 | A  | C      | I      | I      | R  | C           | I           | I    | I      |
| UAT Security                   | C  | C      | A/R    | I      | R  | C           | I           | C    | I      |
| Environment Preparation        | I  | C      | C      | A/R    | I  | C           | I           | I    | I      |
| Deployment to Production       | C  | C      | A      | R      | I  | C           | I           | C    | I      |
| Go-Live Approval               | A  | C      | C      | C      | C  | I           | I           | C    | I      |
| Post-Implementation Monitoring | I  | I      | A/R    | R      | I  | I           | I           | C    | C/R    |
| Periodic Audit                | I  | I      | C      | C      | I  | I           | I           | C    | A/R    |

---

# 10. Security by Design (SbD) – Perbankan

## 10.1 Data Protection by Design
- tidak menyimpan foto atau PII lokal  
- semua pemrosesan foto in-memory  
- TLS wajib  
- encryption mandatory  

## 10.2 Least Privilege & Zero Trust
- restricted account  
- process sandboxing  
- environment isolation  

## 10.3 Secure UI/UX for Banking
- anti-minimize  
- anti-gesture  
- auto-clear UI  

## 10.4 Device Hardening
- domain join & GPO lockdown  
- BitLocker  
- USB disabled  

## 10.5 Secure Media Handling
- tidak boleh ada cache  
- file hanya disimpan di backend terenkripsi  

## 10.6 Secure Authentication & Session
- timeout 2–3 menit  
- no persistent session  

## 10.7 Anti-Tampering
- code obfuscation  
- integrity checking  
- anti-debugger  

## 10.8 Environment Trust Enforcement
Aplikasi harus menolak berjalan jika:

- EDR nonaktif  
- domain join tidak valid  
- GPO tidak aktif  

## 10.9 Testability by Design
Testing wajib:

- OS escape  
- gesture  
- privacy  
- local residue  
- SAST/DAST/SCA  
- pentest  

---

# 11. Review & Maintenance

Dokumen ini direview minimal setiap 12 bulan atau ketika:

- arsitektur berubah  
- regulasi berubah  
- temuan audit muncul  
- terjadi insiden keamanan  

---

# 12. Penutup

Dokumen ini merupakan standar resmi keamanan front-office dan Secure SDLC di lingkungan bank.  
Seluruh unit internal dan vendor wajib mematuhi seluruh kebijakan ini tanpa pengecualian.

---

