#  Tugas 1 Jarkom: Dokumentasi Subnetting, Routing, & Troubleshooting

Dokumentasi ini berisi seluruh proses pengerjaan, konfigurasi, dan verifikasi untuk Tugas 1 Mata Kuliah Komunikasi Data dan Jaringan Komputer.

- **Nama:** Nisrina Bilqis
- **NRP:** 5027241054
- **Base Network:** 10.94.0.0 (Didapat dari `10.(5027241054 mod 256).0.0`)

---

## 1. Perencanaan Alokasi IP (VLSM & CIDR)

Perencanaan IP Address dimulai dengan Base Network `10.94.0.0`.

### Tabel VLSM
| Ruang/Kebutuhan | Jml Host | IP Block | Network Address | Subnet Mask | Prefix | Range Host | Broadcast | Gateway |
| :--- | :---: | :---: | :--- | :--- | :---: | :--- | :--- | :--- |
| Sekretariat | 380 | 512 | `10.94.0.0` | `255.255.254.0` | /23 | `10.94.0.1` - `10.94.1.254` | `10.94.1.255` | `10.94.0.1` |
| B. Kurikulum | 220 | 256 | `10.94.2.0` | `255.255.255.0` | /24 | `10.94.2.1` - `10.94.2.254` | `10.94.2.255` | `10.94.2.1` |
| B. Guru & Tendik | 95 | 128 | `10.94.3.0` | `255.255.255.128` | /25 | `10.94.3.1` - `10.94.3.126` | `10.94.3.127` | `10.94.3.1` |
| B. Sarpras | 45 | 64 | `10.94.3.128`| `255.255.255.192` | /26 | `10.94.3.129` - `10.94.3.190`| `10.94.3.191` | `10.94.3.129`|
| B. Pengawas | 18 | 32 | `10.94.3.192`| `255.255.255.224` | /27 | `10.94.3.193` - `10.94.3.222`| `10.94.3.223` | `10.94.3.193`|
| Server & Admin | 6 | 8 | `10.94.3.224`| `255.255.255.248` | /29 | `10.94.3.225` - `10.94.3.230`| `10.94.3.231` | `10.94.3.225`|
| Link Antar-Router | 2 | 4 | `10.94.3.232`| `255.255.255.252` | /30 | `10.94.3.233` - `10.94.3.234`| `10.94.3.235` | (N/A) |

### Tabel Agregasi CIDR (Untuk Static Routing)
| Lokasi | Alamat Supernet (Agregasi) | Subnet Mask Supernet |
| :--- | :--- | :--- |
| **Kantor Pusat** | **`10.94.0.0/22`** | `255.255.252.0` |
| **Kantor Cabang** | **`10.94.3.192/27`** | `255.255.255.224` |
| **Link WAN** | **`10.94.3.232/30`** | `255.255.255.252` |

### Tabel Visualisasi Supernetting (Gabungan Total)
| Keterangan | Network Address | Biner Oktet ke-3 | Biner Oktet ke-4 |
| :--- | :--- | :--- | :--- |
| **Network Pertama**<br>(Sekretariat) | `10.94.0.0` | `00000000` | `.00000000` |
| **Alamat Terakhir**<br>(Broadcast Link WAN) | `10.94.3.235` | `00000011` | `.11101011` |
| **Bit yang Sama** | **`10.94.`** | **`000000`**`xx` | `.` `xxxxxxxx` |
| **Total Bit Sama** | 8 (oktet 1) + 8 (oktet 2) + 6 (oktet 3) | = **22 bit** | |
| **Alamat Supernet** | **`10.94.0.0/22`** | | |
| **Mask Supernet** | `255.255.252.0` | | |

---

## 2. Desain Topologi Jaringan

Topologi final yang dirancang di Cisco Packet Tracer, menghubungkan Kantor Pusat dan Kantor Cabang menggunakan kabel **Copper Cross-Over**.

<img width="1627" height="685" alt="image" src="https://github.com/user-attachments/assets/a8a09b6e-942c-4e2a-a1a7-7e80f7897e52" />


---

## 3. Konfigurasi IP Address Perangkat

Konfigurasi IP statis dilakukan pada setiap End Device (PC).

### Konfigurasi PC-Sekretariat
<img width="1636" height="479" alt="image" src="https://github.com/user-attachments/assets/b3e1d283-5565-4c77-95ab-983a3b69e6af" />

### Konfigurasi PC-Kurikulum
<img width="1302" height="475" alt="image" src="https://github.com/user-attachments/assets/9293f279-6a60-4671-bf7c-c616cfa992ef" />

### Konfigurasi PC-Guru Tendik
<img width="1650" height="565" alt="image" src="https://github.com/user-attachments/assets/09d37184-9dd5-4817-940c-c696890b1257" />

### Konfigurasi PC-Sarpras
<img width="1632" height="723" alt="image" src="https://github.com/user-attachments/assets/0b3cf058-489f-431f-9d59-8dd50dc952e2" />

### Konfigurasi PC-Server & Admin
<img width="1626" height="771" alt="image" src="https://github.com/user-attachments/assets/dbc94f6f-a060-4bdc-87fa-09cd89416a5e" />

### Konfigurasi PC-Pengawas (PC5)
<img width="1565" height="467" alt="image" src="https://github.com/user-attachments/assets/ac71bc77-1a78-46cc-b89e-e99c0b028429" />

### Konfigurasi R-Cabang ke R-Pusat
<img width="1631" height="386" alt="image" src="https://github.com/user-attachments/assets/dfce6b02-dea3-4ea5-bcbf-b377272a86f8" />


---

## 4. Verifikasi Konfigurasi Router

Verifikasi dilakukan menggunakan `show` command untuk memastikan semua konfigurasi IP, interface, dan routing telah benar.

### Verifikasi Interface R-Pusat
Perintah `show ip interface brief` pada **R-Pusat** menunjukkan semua IP Gateway (termasuk VLANs) dan IP Link (`10.94.3.233`) berstatus **`up / up`**.
<img width="917" height="305" alt="image" src="https://github.com/user-attachments/assets/50e541e2-dd1c-4fef-968a-b2b3310353b9" />

### Verifikasi Interface R-Cabang
Perintah `show ip interface brief` pada **R-Cabang** menunjukkan IP Gateway LAN (`10.94.3.193`) dan IP Link (`10.94.3.234`) berstatus **`up / up`**.
<img width="920" height="139" alt="image" src="https://github.com/user-attachments/assets/2e9835dc-7cf6-4359-ba58-e342f076673c" />

### Verifikasi Rute Statis (Static Route)
**R-Pusat `show ip route static`:**
Membuktikan R-Pusat telah mempelajari rute ke jaringan Pengawas (`10.94.3.192/27`) via `10.94.3.234`.
<img width="636" height="82" alt="image" src="https://github.com/user-attachments/assets/af07b10d-c7e1-4a0f-9534-52dab0dd40c7" />

**R-Cabang `show ip route static`:**
Membuktikan R-Cabang telah mempelajari rute supernet ke Kantor Pusat (`10.94.0.0/22`) via `10.94.3.233`.
<img width="605" height="104" alt="image" src="https://github.com/user-attachments/assets/ecf7d681-5739-48c1-90b1-23e693a90d03" />

---

## 5. Pengujian & Analisis
Pengujian ping dari `PC0-Sekretariat` ke `PC-Pengawas` **BERHASIL**. Hal ini menandakan bahwa seluruh konfigurasi ini berhasil dan saling terhubung.

<img width="813" height="786" alt="image" src="https://github.com/user-attachments/assets/5144f7ab-1dc0-4f07-ab1e-aee8c9e6be69" />

### Kesimpulan
Jaringan telah berhasil dirancang, dikonfigurasi, dan diverifikasi. Konektivitas penuh *end-to-end* antara Kantor Pusat dan Kantor Cabang telah tercapai.
