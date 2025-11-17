# **Kebijakan Keamanan Perangkat dan Aplikasi Front-Office**

**Versi:** 1.0
**Status:** -
**Dokumen:** `front-office-security-policy.md`

---

## **1. Pernyataan Kebijakan**

Perusahaan mewajibkan seluruh perangkat dan aplikasi front-office—termasuk PC AIO, workstation teller, terminal customer service, dan aplikasi yang dikembangkan oleh pihak ketiga—untuk beroperasi dalam lingkungan yang **aman, terkendali, dan terkelola secara terpusat**.

Perangkat atau aplikasi yang tidak memenuhi persyaratan keamanan dalam kebijakan ini **dilarang digunakan** dalam operasional cabang.

---

## **2. Tujuan**

* Melindungi data pribadi nasabah dari kebocoran, manipulasi, dan akses tidak sah.
* Mencegah insiden **OS escape**, pengambilan foto, atau akses konfigurasi aplikasi yang tidak diotorisasi.
* Menetapkan standar keamanan minimum untuk vendor pengembang aplikasi dan vendor penyedia perangkat AIO.
* Mengadopsi praktik terbaik keamanan berdasarkan NIST, CIS Controls, ISO 27001, dan UU PDP.

---

## **3. Ruang Lingkup**

Kebijakan ini berlaku untuk:

* Semua kantor cabang dan unit layanan.
* Seluruh perangkat endpoint front-office berbasis Windows (AIO, teller PC, CS terminal).
* Semua aplikasi front-office yang dikembangkan internal atau oleh pihak ketiga.
* Semua vendor penyedia perangkat dan vendor pengembang aplikasi.
* Seluruh pegawai dan pihak ketiga yang mengoperasikan atau mengelola perangkat tersebut.

---

## **4. Standar dan Regulasi Acuan**

* **NIST Cybersecurity Framework 2.0** — PR.AC, PR.PT, PR.DS, ID.SC, DE.CM
* **NIST SP 800-53 Rev5** — AC-3, CM-6, CM-7, SC-28, SI-3
* **CIS Controls v8** — Control 4, 5, 10, 14
* **ISO/IEC 27001:2022** — Annex A.8, A.8.7, A.8.9, A.14
* **UU Perlindungan Data Pribadi (UU PDP)** — Data pribadi sensitif & biometrik

---

## **5. Kebijakan Keamanan**

---

### **5.1. Domain Join dan Pengelompokan Perangkat (Mandatory)**

* Seluruh perangkat front-office **wajib join Active Directory Domain** perusahaan.
* Perangkat harus ditempatkan pada OU khusus:

  ```
  OU = Front-Office Application Workstation
  ```
* Perangkat non-domain dianggap **Non-Compliant** dan tidak boleh digunakan di cabang.

---

### **5.2. Hardening Perangkat dan Konfigurasi Sistem**

Perangkat front-office wajib mematuhi baseline berikut:

* Kiosk Mode / Assigned Access (fullscreen locked).
* Disable seluruh OS gesture:

  * swipe left/right/top/bottom
  * Task View
  * Action Center
* Disable seluruh fungsi navigasi OS:

  * ALT+TAB, Windows key, CTRL+ALT+DEL
  * File Explorer, Control Panel, Run, CMD, PowerShell
* Disable multi-tasking dan multi-desktop.
* BitLocker wajib aktif.
* USB dan removable media **dinonaktifkan**.
* Local admin **dilarang** digunakan.
* Hanya aplikasi yang diotorisasi yang boleh berjalan.

---

### **5.3. Endpoint Security (EDR) — Mandatory**

* Semua perangkat front-office wajib dipasang **EDR** yang telah ditetapkan perusahaan.
* Tamper Protection wajib aktif.
* EDR wajib terhubung ke SIEM pusat.
* Perangkat tanpa EDR otomatis masuk status **quarantine** dan tidak boleh digunakan.

---

### **5.4. Kebijakan Penyimpanan Foto & Data Sensitif**

* Foto nasabah, biometrik, dan data identitas **tidak boleh disimpan di perangkat lokal**.
* Pemrosesan foto hanya boleh dilakukan **in-memory**, lalu dikirim ke backend melalui **TLS 1.2/1.3**.
* Penyimpanan hanya boleh dilakukan di backend dalam bentuk terenkripsi (AES-256).
* Aplikasi dilarang menyimpan:

  * temporary files
  * cache
  * IndexedDB / LocalStorage
  * thumbnail / preview files

---

### **5.5. Kebijakan untuk Vendor Pengembang Aplikasi**

Vendor wajib:

* Mengembangkan aplikasi yang berjalan dalam **Secure Front-Office Mode**.
* Menjamin aplikasi **tidak dapat di-escape** ke OS dalam kondisi apapun.
* Menyediakan bukti **Secure SDLC**:

  * SAST, DAST, SCA
  * Threat Model
  * Data Flow Diagram
* Menyerahkan hasil pengujian berikut:

  * OS Escape Test
  * Gesture Bypass Test
  * Local Storage Exposure Test
  * File System Access Test

Aplikasi yang gagal salah satu test **tidak boleh di-deploy**.

---

### **5.6. Kebijakan untuk Vendor Penyedia Perangkat (AIO)**

Vendor perangkat wajib menjamin bahwa perangkat:

* Mendukung domain join dan kompatibel dengan GPO lockdown.
* Mendukung kiosk mode dan EDR perusahaan.
* Tidak berisi aplikasi bawaan (bloatware) yang berisiko.
* Lulus pengujian:

  * BIOS Lock Test
  * Boot Bypass Test
  * Gesture Disable Compatibility
  * OS Escape Test

Perangkat yang gagal tidak boleh digunakan dalam operasional cabang.

---

### **5.7. Pengujian Keamanan (Aplikasi + Perangkat)**

#### **Pengujian Aplikasi**

* SAST
* DAST
* SCA
* UI Privacy Test
* Config Protection Test

#### **Pengujian Perangkat (Endpoint Pentest)**

* OS Escape Test
* Gesture/Swipe Bypass Test
* Kiosk Mode Bypass Test
* File System Access Test
* Local Residue/Thumbnail Exposure Test
* USB Rogue Device Test
* BIOS Bypass Test

#### **Pengujian End-to-End**

* Dilakukan pada perangkat AIO vendor
* Dalam kondisi:

  * domain join aktif
  * GPO aktif
  * EDR aktif

---

### **5.8. Monitoring, Logging, dan Audit**

* Status domain join, EDR, GPO, dan compliance harus dipantau harian.
* Security event dikirim ke SIEM pusat.
* Audit dilakukan:

  * sebelum go-live,
  * setiap bulan,
  * setelah insiden.
* Perangkat non-compliant **tidak boleh digunakan**.

---

## **6. Penegakan dan Sanksi**

### **Vendor Pengembang Aplikasi**

* Gagal OS Escape Test → *deployment rejected*.
* Menyimpan foto lokal → *major violation*.

### **Vendor Penyedia Perangkat**

* Perangkat tidak kompatibel → *unit ditolak*.
* Hardening tidak dapat diterapkan → *vendor dapat diblacklist*.

### **Cabang Operasional**

* Menggunakan perangkat non-compliant → *sanksi operasional*.

---

## **7. Tanggung Jawab**

| Unit                    | Tanggung Jawab                             |
| ----------------------- | ------------------------------------------ |
| **IT Infrastructure**   | Domain join, GPO, OU, deployment EDR       |
| **Security Operations** | SIEM monitoring, pentest, audit cabang     |
| **Vendor Aplikasi**     | Secure SDLC, OS escape prevention          |
| **Vendor Perangkat**    | Hardening compliance, device compatibility |
| **Cabang**              | Menggunakan perangkat compliant            |

---

## **8. Review Kebijakan**

Dokumen ini harus direview minimal **setiap 12 bulan**, atau setelah:

* insiden keamanan,
* perubahan arsitektur aplikasi,
* perubahan standar NIST/CIS/ISO, atau
* perubahan peraturan pemerintah (UU PDP).

---

## **9. Catatan**

Dokumen ini merupakan bagian dari Information Security Management System (ISMS) perusahaan dan wajib dipatuhi oleh seluruh pegawai serta vendor.

---
