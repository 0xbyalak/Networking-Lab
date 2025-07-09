
# 📘 Dokumentasi Belajar GRE Tunnel MikroTik

## 🧠 Apa Itu GRE Tunnel?

**GRE (Generic Routing Encapsulation)** adalah protokol tunneling Layer 3 yang digunakan untuk mengenkapsulasi berbagai protokol jaringan dalam koneksi point-to-point. GRE digunakan untuk membentuk jalur privat antar-router melalui jaringan publik seperti internet.

GRE sangat berguna untuk:

- Menghubungkan dua jaringan melalui internet
- Mengangkut routing dinamis (OSPF, BGP)
- Membuat jalur backup antar lokasi
- Dapat digabungkan dengan IPsec untuk keamanan

---

## ⚙️ Opsi-Opsi GRE Tunnel di MikroTik

| Opsi              | Penjelasan |
|-------------------|------------|
| **Name**          | Nama interface tunnel |
| **Remote Address**| IP publik router tujuan |
| **Local Address** | IP publik router kita sendiri *(bisa dikosongkan untuk otomatis)* |
| **Keepalive**     | Paket untuk mengecek apakah tunnel masih hidup |
| **DSCP**          | Untuk QoS pada paket GRE |
| **Clamp TCP MSS** | Mengatur MSS agar tidak terjadi fragmentasi |
| **Allow Fast Path** | Mengaktifkan hardware acceleration (jika didukung) |

---

## 🧭 Topologi Sederhana

```
[LAN A] -- R1 -- Internet -- R2 -- [LAN B]

R1 (IP Publik): 203.0.113.1
R2 (IP Publik): 203.0.113.2

IP Tunnel: 
R1 = 10.255.255.1/30
R2 = 10.255.255.2/30

LAN A: 192.168.1.0/24
LAN B: 192.168.2.0/24
```

---

## 🛠️ Langkah-langkah Konfigurasi GRE Tunnel

### 🔹 Di Router 1 (R1)

```bash
/interface gre add name=gre-tunnel remote-address=203.0.113.2 local-address=203.0.113.1
/ip address add address=10.255.255.1/30 interface=gre-tunnel
/ip route add dst-address=192.168.2.0/24 gateway=10.255.255.2
```

### 🔹 Di Router 2 (R2)

```bash
/interface gre add name=gre-tunnel remote-address=203.0.113.1 local-address=203.0.113.2
/ip address add address=10.255.255.2/30 interface=gre-tunnel
/ip route add dst-address=192.168.1.0/24 gateway=10.255.255.1
```

---

## ✅ Pengujian

- Ping dari Router 1 ke `10.255.255.2`
- Ping dari PC di LAN A ke LAN B (misal `ping 192.168.2.1`)
- Pastikan tidak ada firewall yang memblokir protokol GRE (protocol 47)

---

## 🧠 Catatan Tambahan

- GRE **tidak menyediakan enkripsi**. Jika butuh keamanan, gunakan GRE over IPsec.
- Jika router berada di balik NAT, GRE bisa bermasalah kecuali NAT mendukung GRE passthrough.
- Bisa digunakan untuk membawa routing dinamis seperti OSPF dan BGP antar router.
- GRE cocok untuk backbone antar site dengan kontrol penuh.

---

## 📝 Penutup

GRE adalah solusi praktis untuk membangun jaringan privat virtual antar router dengan fleksibilitas tinggi. Jika dikombinasikan dengan protokol routing dan IPsec, dapat memberikan konektivitas dan keamanan tingkat lanjut antar site.
