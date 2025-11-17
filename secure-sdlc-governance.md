# **Secure SDLC Governance — Roles & Responsibilities**

**Dokumen:** `secure-sdlc-governance.md`
**Versi:** 1.0
**Status:** Approved
**Klasifikasi:** Internal – Confidential

---

## **1. Pendahuluan**

Dokumen ini menetapkan **struktur tanggung jawab (roles & responsibilities)** dalam pelaksanaan **Secure Software Development Life Cycle (Secure SDLC / SSDLC)** di lingkungan perusahaan.
Tujuannya adalah memastikan bahwa seluruh aplikasi—baik dikembangkan internal maupun oleh pihak ketiga—mengikuti prinsip keamanan sejak tahap perencanaan hingga implementasi.

Dokumen ini wajib dipatuhi oleh seluruh unit TI, unit bisnis, vendor, dan pihak terkait lainnya.

---

## **2. Standar dan Regulasi Acuan**

Secure SDLC di perusahaan mengacu pada standar berikut:

* **NIST SP 800-218 – Secure Software Development Framework (SSDF)**
* **NIST SP 800-64 – Security Considerations in the System Development Life Cycle**
* **NIST SP 800-53 Rev5 – Security & Privacy Controls**
* **NIST Cybersecurity Framework 2.0**
* **ISO/IEC 27001:2022 – Annex A.14 Secure Development**
* **CIS Controls v8 – Control 16: Application Security**
* **Regulasi OJK & UU Perlindungan Data Pribadi (UU PDP)**

---

## **3. Tujuan**

* Mendefinisikan dengan jelas pihak bank yang bertanggung jawab pada setiap fase Secure SDLC.
* Menjamin seluruh aplikasi dan perangkat front-office tetap aman, terkontrol, dan sesuai regulasi.
* Menentukan akuntabilitas ketika aplikasi dikembangkan oleh **pihak ketiga** dan berjalan pada **perangkat yang juga disediakan oleh pihak ketiga**.
* Menyediakan struktur governance yang dapat diaudit oleh Internal Audit, Risk Management, dan regulator.

---

## **4. Peran (Roles) dan Tanggung Jawab (Responsibilities)**

Di bawah ini merupakan peran resmi dalam implementasi Secure SDLC dari pihak bank.

---

### **4.1. Executive Sponsor (CTO / Head of ICT)**

**Tanggung Jawab:**

* Menyetujui program SSDLC dan kebijakan keamanan aplikasi.
* Mengalokasikan sumber daya dan anggaran untuk keamanan.
* Memastikan seluruh organisasi mematuhi prinsip SSDLC.
* Mengeluarkan keputusan strategis terkait risiko residual.

---

### **4.2. Business Owner / System Owner**

**Tanggung Jawab:**

* Menyusun Business Requirements Document (BRD).
* Menentukan fitur, scope, dan kebutuhan keamanan tingkat bisnis.
* Mengelola risiko bisnis terkait sistem.
* Memberikan final approval terhadap implementasi sistem.

---

### **4.3. IT Solution / IT Development Team**

**Tanggung Jawab:**

* Menyusun desain teknis, arsitektur, dan data flow.
* Mengawasi kualitas solusi yang dikerjakan vendor.
* Melakukan code review jika source code diserahkan oleh vendor.
* Menjamin integrasi aplikasi mengikuti standar bank (network, EDR, GPO, domain join).

---

### **4.4. IT Security (CISO Office / Information Security Division)**

*(Peran paling kritikal dalam SSDLC berdasarkan NIST SSDF)*

**Tanggung Jawab:**

* Menyusun Security Requirements (SRD) untuk setiap aplikasi.
* Melakukan Architecture Security Review & Threat Modeling.
* Menentukan kontrol keamanan minimum yang wajib diterapkan vendor.
* Melakukan keamanan:

  * Secure Code Review (SAST)
  * Dynamic Analysis (DAST)
  * Penetration Testing (Pentest)
* Melakukan validasi terhadap:

  * OS escape prevention
  * no local storage policy
  * secure handling of personal data
* Mengeluarkan **Security Sign-Off** atau menolak Go-Live jika terdapat risiko High/Critical.

---

### **4.5. IT Operations / Infrastructure Team**

**Tanggung Jawab:**

* Menyediakan environment untuk DEV, SIT, UAT, Staging, dan Production.
* Mengimplementasikan hardening server & endpoint.
* Menjamin perangkat front-office:

  * join domain,
  * masuk OU yang benar,
  * menerima GPO lockdown,
  * dipasangi EDR.
* Menjalankan deployment aplikasi di lingkungan produksi.

---

### **4.6. Vendor Management / Procurement / Legal**

**Tanggung Jawab:**

* Memasukkan klausul keamanan (Security Addendum) ke dalam kontrak vendor.
* Menjamin vendor wajib memenuhi standar SSDLC bank.
* Mengatur SLA, penalti, dan hak audit terhadap vendor.
* Memastikan vendor menyerahkan dokumen keamanan:

  * SAST, DAST, SCA reports
  * Pentest report
  * Secure configuration documentation
  * Hardening compatibility

---

### **4.7. QA / UAT Team**

**Tanggung Jawab:**

* Menjalankan functional testing.
* Menjalankan UAT bersama Business Owner.
* Menjalankan *Security UAT*:

  * OS escape test
  * Gesture bypass validation
  * Local storage check
  * Data privacy validation

---

### **4.8. IT & Operational Risk Management**

**Tanggung Jawab:**

* Melakukan risk assessment pada setiap perubahan besar.
* Menilai inherent risk dan residual risk.
* Menentukan mitigasi atau compensating control.
* Memberikan rekomendasi risiko sebelum implementasi.
* Berhak menolak implementasi jika risiko tidak dapat diterima.

---

### **4.9. Internal Audit (Satuan Pengawasan Intern)**

**Tanggung Jawab:**

* Melakukan audit kepatuhan Secure SDLC secara berkala.
* Memastikan semua fase Secure SDLC terdokumentasi dan dijalankan.
* Mengeluarkan temuan audit dan rekomendasi perbaikan.
* Melaporkan ketidakpatuhan ke manajemen puncak.

---

## **5. Tanggung Jawab Secure SDLC per Fase (Matrix)**

### **Perencanaan (Planning)**

* Business Owner
* IT Solution
* IT Security

### **Analisis & Requirement (Analysis)**

* Business Owner
* IT Security (Security Requirements)
* IT Solution

### **Desain (Design)**

* IT Solution (Architecture)
* IT Security (Threat Modeling & Security Review)

### **Pengembangan (Development)**

* Vendor Developer
* IT Solution (Oversight)
* IT Security (Secure Code Review if applicable)

### **Pengujian (Testing)**

* QA/UAT Team
* IT Security (SAST, DAST, Pentest)
* Vendor Developer (fixing)

### **Implementasi (Deployment)**

* IT Operations
* IT Security (Final Security Sign-Off)
* Business Owner (Go-Live Approval)

### **Pasca Implementasi (Monitoring)**

* Security Operations
* IT Operations
* Internal Audit

---

## **6. Kesimpulan**

Secure SDLC bukan hanya tanggung jawab developer atau vendor, tetapi merupakan **kolaborasi lintas unit** yang mencakup:

* Business Owner
* IT Solution
* IT Security
* IT Operations
* Vendor Management
* QA/UAT
* Risk Management
* Internal Audit

Unit yang **bertanggung jawab utama** untuk memastikan keamanan ada pada:

### **→ IT Security (CISO Office)**

### **→ IT Solution & IT Operations**

### **→ Business Owner (risk owner)**

Semua unit wajib bekerja sesuai tanggung jawabnya agar aplikasi front-office berjalan aman, stabil, dan sesuai regulasi.

---

## **7. Revisi & Review**

Dokumen ini direview minimal **setiap 12 bulan**, atau ketika terjadi:

* perubahan regulasi,
* perubahan arsitektur sistem,
* temuan audit,
* insiden keamanan.

---
