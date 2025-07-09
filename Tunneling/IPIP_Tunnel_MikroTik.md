
# 📘 Dokumentasi Belajar IPIP Tunnel MikroTik

## 🧠 Apa Itu IPIP Tunnel?

**IPIP (IP over IP)** adalah salah satu jenis tunneling di MikroTik yang memungkinkan kita mengenkapsulasi paket IP di dalam paket IP lain. Tunnel ini bersifat **Layer 3 (network layer)** dan cocok digunakan untuk:

- Menghubungkan dua jaringan berbeda melalui internet
- Menghubungkan router dengan IP publik masing-masing
- Membuat jalur komunikasi privat antar site
- Transportasi protokol routing seperti OSPF, BGP, dsb.

---

## ⚙️ Opsi-Opsi pada IPIP Tunnel

| Opsi              | Penjelasan |
|-------------------|------------|
| **Name**          | Nama interface tunnel |
| **Remote Address**| IP publik router tujuan |
| **Local Address** | IP publik router kita sendiri *(bisa dikosongkan untuk otomatis)* |
| **MTU**           | Maximum Transmission Unit (default 1480) |
| **Clamp TCP MSS** | Menyesuaikan MSS secara otomatis agar menghindari fragmentasi |
| **Allow Fast Path** | Aktifkan FastTrack (jika routing tidak terlalu kompleks) |

---

## 🧭 Topologi Sederhana

```
[LAN A] -- R1 -- Internet -- R2 -- [LAN B]

R1 (IP Publik): 182.16.182.19
R2 (IP Publik): 182.16.182.20

IP Tunnel: 
R1 = 10.10.10.1/30
R2 = 10.10.10.2/30

LAN A: 192.168.10.0/24
LAN B: 192.168.20.0/24
```

---

## 🛠️ Langkah-langkah Konfigurasi IPIP Tunnel

### 🔹 Di Router 1 (R1)

```bash
/interface ipip add name=ipip-tunnel remote-address=182.16.182.20 local-address=182.16.182.19
/ip address add address=10.10.10.1/30 interface=ipip-tunnel
/ip route add dst-address=192.168.20.0/24 gateway=10.10.10.2
```

### 🔹 Di Router 2 (R2)

```bash
/interface ipip add name=ipip-tunnel remote-address=182.16.182.19 local-address=182.16.182.20
/ip address add address=10.10.10.2/30 interface=ipip-tunnel
/ip route add dst-address=192.168.10.0/24 gateway=10.10.10.1
```

---

## ✅ Pengujian

- Coba ping dari R1 ke IP LAN R2: `ping 192.168.20.1`
- Coba ping dari PC di LAN A ke PC di LAN B
- Pastikan tidak ada firewall yang memblokir

---

## 🧠 Catatan Penting

- IPIP membutuhkan IP publik di kedua sisi.
- Jika ada NAT di tengah, pertimbangkan pakai GRE over IPsec atau L2TP/IPsec.
- Jangan lupa cek `firewall > filter` dan `IP > route`.

---

## 📝 Penutup

Dengan IPIP, kita bisa membuat komunikasi antar dua router secara virtual dan privat. Ini berguna untuk perusahaan dengan cabang, atau koneksi antar data center.

