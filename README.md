# Networking Lab

Selamat datang di repositori **Networking Lab** – tempat dokumentasi dan eksplorasi pribadi saya dalam dunia jaringan komputer. Repo ini berisi kumpulan pembelajaran mulai dari konsep dasar hingga topik lanjutan seperti **network automation**, **monitoring**, serta berbagai **proyek nyata** yang pernah saya kerjakan atau simulasikan.

Tujuan utama repositori ini adalah untuk menjadi:
-  Catatan pembelajaran
-  Panduan step-by-step konfigurasi
-  Portofolio untuk karier di bidang Network & Security Engineering

---

## Struktur Direktori

```
networking-lab/
├── basic/
├── dinamic routing/         
├── vlan/              
├── firewall/          
├── tunneling/         
├── QoS/            
├── monitoring/        
├── automation/        
├── pentest/           
├── projects/
├── troubleshooting/        
└── README.md
```

---

## Topik yang Dibahas

### Dasar Jaringan
- IP addressing, subnetting, CIDR
- Routing statik
- Switching dan bridging

### Firewall & Keamanan
- Konfigurasi dasar firewall MikroTik 
- Akses kontrol, NAT, port forwarding
- Filtering traffic berdasarkan interface, port, IP

### VPN & Tunneling
- EoIP, IPIP, GRE, WireGuard
- L2TP over IPSec
- SSTP
- Dokumentasi setup site-to-site

### VLAN & Trunking
- Konfigurasi VLAN pada switch
- VLAN routing antar segmen
- VLAN dengan MikroTik & Cisco


### Monitoring & Visualisasi
- Zabbix dan Grafana
- Konfigurasi agent, SNMP, dan alert
- Dashboards dan visualisasi metrik

### Network Automation
- Otomasi konfigurasi switch/router menggunakan:
  - Ansible
  - Python + Netmiko / Paramiko
  - Script backup & restore konfigurasi
- Inventory jaringan otomatis

### Proyek-Proyek
- Simulasi ISP sederhana dengan VLAN dan PPPoE
- Lab jaringan enterprise (LAN + WAN)
- Monitoring jaringan pelanggan
- Keamanan jaringan dengan IDS/IPS sederhana
- Integrasi Zabbix + Telegram bot untuk notifikasi
