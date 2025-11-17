# banking-frontoffice-security-and-secure-sdlc-policy.md

**Versi:** 1.0
**Status:** -
**Klasifikasi:** Internal – Confidential**

---

# **1. Pendahuluan**

Dokumen ini merupakan kebijakan komprehensif yang mengatur keamanan perangkat front-office, keamanan aplikasi, Secure Software Development Lifecycle (Secure SDLC), serta Security by Design (SbD) di lingkungan perbankan.

Tujuan dokumen ini adalah memastikan:

* aplikasi front-office tidak dapat di-*escape* dari UI ke OS,
* tidak ada data sensitif yang disimpan lokal,
* perangkat bekerja di lingkungan yang terkontrol (domain join, GPO, EDR),
* vendor aplikasi & perangkat mematuhi standar keamanan bank,
* seluruh pengembangan aplikasi mengikuti Secure SDLC dan SbD berbasis standar global.

---

# **2. Dasar Regulasi & Standar Referensi**

## **2.1. Regulasi Indonesia (OJK & BI)**

* POJK 38/POJK.03/2016 – Manajemen Risiko TI
* SEOJK 29/SEOJK.03/2022 – Keamanan Sistem Informasi Perbankan
* POJK 11/POJK.03/2022 – Transformasi Digital Perbankan
* SE BI No. 9/30/DPNP – Risiko TI
* UU Perlindungan Data Pribadi (UU PDP)

## **2.2. Standar Internasional**

* NIST SP 800-218 – Secure Software Development Framework (SSDF)
* NIST SP 800-53 Rev5 – Security & Privacy Controls
* NIST SP 800-160 – Systems Security Engineering
* NIST Cybersecurity Framework 2.0
* ISO/IEC 27001:2022 (A.8, A.8.7, A.8.9, A.14)
* CIS Controls v8 – Control 4, 10, 16
* PCI DSS v4.0 – Data Security
* FFIEC IT Examination Handbook

---

# **3. Lingkup Kebijakan**

Kebijakan ini diberlakukan untuk:

* seluruh aplikasi front-office, teller, customer service, onboarding, dan lain-lain,
* perangkat AIO PC, teller workstation, kiosk, terminal pelanggan,
* seluruh perangkat cabang yang digunakan nasabah/staf,
* seluruh vendor pengembang aplikasi dan vendor perangkat,
* seluruh fase Secure SDLC internal bank.

---

# **4. Kebijakan Keamanan Perangkat Front-Office**

## **4.1. Domain Join (Mandatory)**

Semua perangkat wajib:

* join Active Directory Domain,
* masuk OU khusus Front-Office Workstation,
* mengikuti seluruh GPO lockdown.

Perangkat non-domain → **non-compliant** dan dilarang digunakan.

---

## **4.2. Hardening Perangkat (Critical)**

Semua perangkat harus mengikuti:

* Kiosk Mode / Assigned Access
* Fullscreen lock, tanpa minimize
* Disable gesture: swipe kiri/kanan/atas/bawah
* Disable hotkey: ALT+TAB, Windows, CTRL+ALT+DEL
* Disable Task View, Action Center, File Explorer
* Disable CMD, PowerShell, Run
* USB storage disabled
* BitLocker aktif
* Tidak ada local admin account

Referensi: **CIS Benchmarks, NIST CM-7, ISO 27001 A.8.9**

---

## **4.3. Endpoint Security (Mandatory EDR)**

Semua perangkat harus memiliki:

* EDR aktif dengan tamper protection
* integrasi SIEM pusat
* alerting real-time
* agent tidak boleh dinonaktifkan
* aplikasi front-office menolak berjalan jika EDR nonaktif

---

## **4.4. Penyimpanan Data Sensitif**

Tidak ada data sensitif yang boleh disimpan di perangkat:

* foto nasabah
* biometrik
* KTP/KK
* dokumen nasabah
* konfigurasi rahasia
* log sensitif

Semua pemrosesan harus dilakukan **in-memory**, bukan via file.

Referensi: **UU PDP, NIST SC-28**

---

# **5. Kebijakan Keamanan Aplikasi Front-Office**

## **5.1. Secure Front-Office Mode**

Aplikasi harus mendukung:

* fullscreen lock
* anti-minimize
* UI anti-escape
* tidak spawn window / dialog OS
* tidak bisa membuka file explorer
* tidak menyimpan data lokal
* auto-session clear

---

## **5.2. OS Escape Prevention**

Aplikasi **tidak boleh** dapat keluar ke OS via:

* swipe gesture
* hotkey
* right-click / long-press
* HTML file upload dialog
* WebView escape
* multi-window exploit

Jika satu saja test gagal → aplikasi **tidak boleh digunakan di cabang**.

---

## **5.3. Konfigurasi Aman**

* API key/secret harus dienkripsi
* file konfigurasi tidak boleh readable oleh user workstation
* tidak ada default password
* tidak ada debug mode di production
* TLS 1.2/1.3 mandatory

---

# **6. Kebijakan untuk Vendor Perangkat (AIO/Kiosk)**

Vendor wajib memastikan perangkat:

* kompatibel dengan domain join
* kompatibel dengan GPO lockdown
* kompatibel dengan EDR bank
* bebas bloatware
* memiliki BIOS lock
* lulus OS escape test
* lulus gesture bypass test
* lulus hardening compatibility test

Perangkat yang gagal → ditolak.

---

# **7. Pengujian Keamanan (Aplikasi & Perangkat)**

## **7.1. Pengujian aplikasi**

* SAST
* DAST
* SCA
* Penetration testing
* Threat modeling
* Secure UAT
* Local storage test
* Privacy compliance test

## **7.2. Pengujian perangkat**

* OS escape test
* swipe gesture bypass test
* hotkey bypass test
* USB rogue device test
* BIOS bypass test
* thumbnail/residue test

## **7.3. End-to-End Testing**

Dilakukan di perangkat AIO vendor dengan:

* domain join
* GPO aktif
* EDR aktif

Jika gagal → tidak boleh go-live.

---

# **8. Secure SDLC Governance (Banking)**

## **8.1. Executive Sponsor (CTO / Head of ICT)**

* menyetujui kebijakan
* mengalokasi anggaran
* mengambil keputusan risiko strategis

## **8.2. Business Owner**

* membuat BRD
* melakukan UAT
* pemilik risiko
* memberikan approval go-live bisnis

## **8.3. IT Solution / Development Oversight**

* menyusun arsitektur
* mengawasi pekerjaan vendor
* melakukan architecture review

## **8.4. IT Security (CISO Office)**

* menyusun Security Requirements
* threat modeling
* SAST, DAST, Pentest
* Security Sign-Off (mandatory)
* menolak implementasi jika risiko High/ Critical
* validasi environment keamanan perangkat

## **8.5. IT Operations**

* environment deployment
* domain join & GPO
* EDR deployment
* production deployment

## **8.6. Vendor Management / Procurement / Legal**

* kontrak dengan security addendum
* SLA dan penalti
* compliance vendor

## **8.7. QA / UAT**

* functional testing
* security UAT (escape, gesture, privacy)

## **8.8. Risk Management**

* risk assessment
* residual risk validation
* rekomendasi pengendalian

## **8.9. Internal Audit**

* audit berkala
* audit SSDLC
* temuan & rekomendasi

---

# **9. RACI Matrix – Secure SDLC (Banking)**

```markdown
| SSDLC Activity                    | BO | IT Sol | IT Sec | IT Ops | QA/UAT | Vendor Dev | Vend Mgmt | Risk | Audit |
|----------------------------------|----|--------|--------|--------|--------|-------------|-----------|-------|--------|
| Planning                         | A  | R      | C      | I      | I      | C           | I         | C     | I      |
| Security Requirements            | C  | C      | A/R    | I      | I      | C           | I         | C     | I      |
| Architecture & Design Review     | C  | A/R    | R      | C      | I      | C           | I         | C     | I      |
| Threat Modeling                  | I  | C      | A/R    | C      | I      | C           | I         | C     | I      |
| Development                      | I  | C      | C      | I      | I      | A/R         | I         | I     | I      |
| SAST/SCA Review                  | I  | C      | A/R    | I      | I      | R           | I         | I     | I      |
| DAST/Pentest                     | I  | C      | A/R    | I      | I      | R           | I         | C     | I      |
| UAT Functional                   | A  | C      | I      | I      | R      | C           | I         | I     | I      |
| UAT Security                     | C  | C      | A/R    | I      | R      | C           | I         | C     | I      |
| Environment Preparation          | I  | C      | C      | A/R    | I      | C           | I         | I     | I      |
| Deployment to Production         | C  | C      | A      | R      | I      | C           | I         | C     | I      |
| Go-Live Approval                 | A  | C      | C      | C      | C      | I           | I         | C     | I      |
| Post-Implementation Monitoring   | I  | I      | A/R    | R      | I      | I           | I         | C     | C/R    |
| Periodic Audit                   | I  | I      | C      | C      | I      | I           | I         | C     | A/R    |
```

---

# **10. Security by Design (SbD) – Versi Khusus Perbankan**

## **10.1. Data Protection by Design**

* tidak boleh ada data sensitif di endpoint
* foto diproses in-memory
* TLS mandatory
* encryption mandatory
* data minimization

## **10.2. Least Privilege & Zero Trust**

* aplikasi berjalan sebagai restricted user
* environment isolation
* process sandboxing

## **10.3. Secure UI/UX for Banking**

* anti-gesture
* anti-minimize
* anti-window-spawn
* auto-clear sensitive UI

## **10.4. Device Hardening**

* domain join
* GPO lockdown
* BitLocker
* USB disabled

## **10.5. Secure Media Handling**

* tidak boleh ada penyimpanan foto nasabah lokal
* tidak boleh ada cache
* backend encrypted storage only

## **10.6. Secure Authentication & Session**

* timeout 2–3 menit
* no persistent session
* session stored server-side

## **10.7. Anti-Tampering**

* code obfuscation
* integrity check
* anti-debug

## **10.8. Environment Trust Enforcement**

Aplikasi wajib menolak berjalan jika:

* EDR nonaktif
* domain join tidak valid
* GPO tidak aktif
* hardening gagal

## **10.9. Testability by Design**

Testing wajib sebelum go-live:

* OS escape test
* gesture test
* privacy test
* local storage exposure test
* thumbnail test
* SAST/DAST/SCA
* Pentest

---

# **11. Review dan Maintenance**

Dokumen ini direview minimal setiap **12 bulan** atau ketika:

* terdapat perubahan teknologi atau arsitektur,
* terdapat perubahan regulasi OJK/BI,
* terdapat temuan audit,
* terjadi insiden keamanan signifikan.

---

# **12. Penutup**

Dokumen ini menjadi standar resmi bank dalam memastikan:

* keamanan perangkat cabang,
* keamanan aplikasi front-office,
* integrasi vendor yang aman,
* penerapan Secure SDLC dan SbD yang sesuai standar internasional,
* kepatuhan terhadap regulasi OJK, BI, dan UU PDP.

Kebijakan ini **wajib dipatuhi** oleh seluruh unit dan vendor tanpa pengecualian.

---
