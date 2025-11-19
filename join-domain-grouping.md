## ✅ **1. Pisahkan berdasarkan OU (Organizational Unit)**

Ini adalah cara paling umum dan **paling rapi**.

### **Contoh struktur OU:**

```
Domain.local
│
├── Workstations
│     ├── Internal-PC (1000 PC internal)
│     └── Customer-Facing-AIO (500 AIO untuk nasabah)
│
└── Servers
```

Dengan OU terpisah, Anda bisa:

* Terapkan **Group Policy (GPO)** khusus AIO
* Monitoring terpisah
* Deployment aplikasi berbeda
* Restriksi akses tertentu hanya untuk AIO
* Naming convention bisa dipaksa lewat GPO atau script

---

## ✅ **2. Gunakan “Security Group”** sebagai tagging tambahan

Selain OU, buat **security group**:

* `GRP-AIO-CustomerFacing`
* `GRP-Internal-PC`

Lalu masukkan setiap komputer ke grup sesuai kategori.

Ini berguna kalau:

* Ada software deployment (SCCM/Intune) yang berdasarkan group
* Access control tertentu (shared folder, printer, server)

---

## ✅ **3. Gunakan Naming Convention (opsional, tapi sangat membantu)**

Misal:

* Internal PC → `INT-PC-XXXX`
* AIO Nasabah → `AIO-CUST-XXXX`

Ini memudahkan:

* Identifikasi cepat di AD
* Filtering via script PowerShell
* Reporting via SCCM/Intune
* Asset management

---

## ✅ **4. (Opsional) Tagging menggunakan tools deployment**

Jika Anda pakai:

* **SCCM**
* **Intune**
* **ManageEngine**
* **PDQ**
* **Tanium**

Anda bisa mengelompokkan berdasarkan:

* Lokasi
* Role
* Tag hardware
* OU

---

## 📌 **Rekomendasi terbaik untuk skenario Anda**

Karena ada **500 AIO khusus nasabah**, saran struktur:

### **OU-based segregation**

```
Domain.local
  └── Workstations
       ├── Internal
       └── CustomerFacing
```

### **Security Group tagging**

Buat satu grup:

```
GRP-AIO-CustomerFacing
```

Lalu tiap AIO otomatis masuk grup via script:

```powershell
Add-ADGroupMember -Identity "GRP-AIO-CustomerFacing" -Members AIO-PC-001
```

---

# ✅ **1. Join Domain ≠ Masuk Grouping**

Ketika sebuah PC **join ke domain**, hanya ada 2 hal yang terjadi:

### ✔ PC terdaftar di Active Directory

Biasanya masuk ke:

```
Computers (default container)
```

### ✔ Mendapatkan domain policies *default*

Belum tentu dapat GPO khusus.

**TIDAK ADA** otomatis:

✘ Tidak otomatis masuk OU tertentu
✘ Tidak otomatis masuk security group
✘ Tidak otomatis masuk kategori “Internal-PC” atau “AIO-Nasabah”

---

# ✅ **2. Grouping (OU / Security Group) harus dilakukan manual atau via automation**

### 🔵 **OU (Organizational Unit)**

Untuk *pengelompokan perangkat berdasar fungsi*, misalnya:

* Internal-PC
* CustomerFacing-AIO
* TellerDevices
* Kiosk

OU digunakan untuk:

* Deploy GPO yang berbeda
* Manage SCCM/Intune dengan targeted collection
* Compliance rules
* Restriksi aplikasi

➡ PC **hanya masuk OU jika Anda memindahkan** komputer tersebut ke OU tersebut.

---

### 🔵 **Security Group**

Digunakan untuk:

* Akses aplikasi tertentu
* Deployment software
* Pengaturan permission folder
* Firewall grouping

Security group juga **tidak otomatis terisi** saat PC join domain.

---

# ⚠ Kenapa banyak perusahaan besar tidak pakai default container "Computers"?

Karena semua device akan numpuk di sana, dan **GPO tidak bisa di-link langsung ke "Computers"** (karena itu sebuah *container*, bukan OU).

Makanya dibuat struktur OU seperti yang sudah kita desain.

---

# 📌 Kesimpulan

### ✔ Join domain = PC masuk ke domain (default container)

### ✔ Grouping = Anda yang menentukan

### ✔ Dua hal ini 100% terpisah, kecuali Anda buat automation

---


