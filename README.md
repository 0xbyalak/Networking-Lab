# 🧠 Networking Lab

Selamat datang di repositori **Networking Lab** – tempat dokumentasi dan eksplorasi pribadi saya dalam dunia jaringan komputer. Repo ini berisi kumpulan pembelajaran mulai dari konsep dasar hingga topik lanjutan seperti **network automation**, **monitoring**, serta berbagai **proyek nyata** yang pernah saya kerjakan atau simulasikan.

Tujuan utama repositori ini adalah untuk menjadi:
- 📘 Catatan pembelajaran yang tersusun rapi
- 🧑‍💻 Panduan praktikal untuk latihan langsung
- 🚀 Portofolio terbuka untuk karier di bidang Network & Security Engineering

---

## 🧭 Struktur Direktori

Setiap topik utama memiliki direktori sendiri yang berisi dokumentasi, konfigurasi, dan jika diperlukan, file topologi atau lab GNS3/PNETLab.

```
networking-lab/
│
├── basic/             # Konsep dasar jaringan, IP, subnetting, routing statik
├── vlan/              # Segmentasi jaringan menggunakan VLAN
├── firewall/          # Konfigurasi firewall dasar hingga advanced (MikroTik, iptables)
├── tunneling/         # Berbagai metode tunneling (EoIP, IPIP, L2TP, SSTP, WireGuard, dsb)
├── server/            # Setup DNS, DHCP, File Server, Web Server, dll.
├── monitoring/        # Instalasi dan setup Zabbix, Grafana, PRTG
├── automation/        # Ansible, Python, Netmiko, dan otomasi konfigurasi jaringan
├── pentest/           # Lab pentest jaringan (DVWA, TryHackMe, exploit jaringan)
├── projects/          # Proyek-proyek simulasi dan nyata dengan dokumentasi lengkap
└── README.md
```

---

## 📌 Topik yang Dibahas

### 🔰 Dasar Jaringan
- IP addressing, subnetting, CIDR
- Routing statik dan dinamis
- Switching dan bridging

### 🛡️ Firewall & Keamanan
- Konfigurasi dasar firewall MikroTik / iptables Linux
- Akses kontrol, NAT, port forwarding
- Filtering traffic berdasarkan interface, port, IP

### 🌐 VPN & Tunneling
- EoIP, IPIP, GRE, WireGuard
- L2TP over IPSec
- SSTP
- Dokumentasi setup end-to-end

### 🧱 VLAN & Trunking
- Konfigurasi VLAN pada switch
- VLAN routing antar segmen
- VLAN dengan MikroTik & Cisco

### 🖥️ Server Jaringan
- DNS server (bind9, MikroTik)
- DHCP server Linux & MikroTik
- File sharing (Samba, FTP)
- Web server (LAMP stack)

### 📊 Monitoring & Visualisasi
- Zabbix, Grafana, dan PRTG
- Konfigurasi agent, SNMP, dan alert
- Dashboards dan visualisasi metrik

### ⚙️ Network Automation
- Otomasi konfigurasi switch/router menggunakan:
  - Ansible
  - Python + Netmiko / Paramiko
  - Script backup & restore konfigurasi
- Inventory jaringan otomatis

### 🛠️ Proyek-Proyek
- Simulasi ISP sederhana dengan VLAN dan PPPoE
- Lab jaringan enterprise (LAN + WAN)
- Monitoring jaringan pelanggan
- Keamanan jaringan dengan IDS/IPS sederhana
- Integrasi Zabbix + Telegram bot untuk notifikasi

---

## 📚 Cara Menggunakan

1. Pilih direktori sesuai topik yang ingin dipelajari
2. Buka file `README.md` atau file konfigurasi di dalamnya
3. Ikuti instruksi step-by-step sesuai topologi/lab
4. Sebagian lab dapat dijalankan di GNS3, PNETLab, atau di VM lokal

---

## 💬 Kontak & Diskusi

Repositori ini bersifat terbuka untuk pembelajaran dan dokumentasi pribadi. Namun jika kamu ingin berdiskusi atau memberi saran, feel free untuk:
- Membuka [Issues](https://github.com/username/networking-lab/issues)
- Fork dan buat PR untuk menambahkan lab kamu sendiri

---

## 🧾 Lisensi

Proyek ini dilisensikan di bawah [MIT License](LICENSE), bebas digunakan untuk belajar dan pengembangan pribadi.