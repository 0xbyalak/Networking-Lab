# 🌐 Dokumentasi Belajar EoIP (Ethernet over IP) di MikroTik

## 🔰 Apa itu EoIP?

**EoIP (Ethernet over IP)** adalah protokol tunneling yang dikembangkan oleh MikroTik untuk mengenkapsulasi frame Layer 2 (Ethernet) ke dalam paket IP, sehingga memungkinkan koneksi Layer 2 (bridge) antar dua lokasi berbeda melalui jaringan IP seperti Internet.

> 🎯 Tujuan utama EoIP:
> - Menghubungkan dua jaringan LAN secara Layer 2 seolah-olah berada di jaringan yang sama.
> - Cocok untuk bridging VLAN, Hotspot roaming, atau site-to-site dengan kebutuhan broadcast Layer 2.

---

## 🧠 Karakteristik EoIP

- Protokol eksklusif MikroTik (hanya bisa antar MikroTik)
- Bisa dibridge dengan interface lokal
- Mendukung VLAN dan broadcast traffic
- Tidak terenkripsi (disarankan digabung dengan IPsec)

---

## ⚙️ Opsi/Opsi Konfigurasi EoIP

| Opsi | Penjelasan |
|------|------------|
| `Remote Address` | IP publik dari router lawan (peer) |
| `Tunnel ID` | Angka identifikasi tunnel (harus sama di kedua sisi) |
| `Local Address` | IP lokal router pengirim (optional) |
| `Keepalive` | Timeout dan interval untuk deteksi status tunnel |
| `MTU` | Ukuran maksimum frame yang bisa lewat (default 1500 - overhead) |

---

## 🛠️ Step-by-Step Konfigurasi EoIP Tunnel

### 🎯 Tujuan:
Menghubungkan 2 router MikroTik (Router A dan B) via EoIP agar seolah berada di jaringan Layer 2 yang sama.

### 🗺️ IP Planning:

| Router | IP Publik | EoIP IP (virtual) |
|--------|-----------|-------------------|
| R1     | 1.1.1.1   | 192.168.100.1     |
| R2     | 2.2.2.2   | 192.168.100.2     |

---

### 🔹 Langkah 1: Buat EoIP di **Router A (R1)**

```bash
/interface eoip add name=eoip-tunnel remote-address=2.2.2.2 tunnel-id=10
/ip address add address=192.168.100.1/30 interface=eoip-tunnel
```

---

### 🔹 Langkah 2: Buat EoIP di **Router B (R2)**

```bash
/interface eoip add name=eoip-tunnel remote-address=1.1.1.1 tunnel-id=10
/ip address add address=192.168.100.2/30 interface=eoip-tunnel
```

---

### 🔹 Langkah 3: Uji Konektivitas

Coba ping dari Router A ke Router B via EoIP IP:
```bash
ping 192.168.100.2
```

Jika reply, maka tunnel EoIP sudah aktif.

---

### 🔹 Langkah 4 (Opsional): Bridge Interface Lokal

Jika kamu ingin menyatukan LAN kedua router:

```bash
/interface bridge add name=bridge1
/interface bridge port add bridge=bridge1 interface=eoip-tunnel
/interface bridge port add bridge=bridge1 interface=ether2
```

Ulangi di kedua router.

---

## 🔐 Tips Keamanan

- Gunakan IPsec untuk mengenkripsi EoIP (karena EoIP sendiri tidak aman)
- Batasi akses port EoIP (47) di firewall
- Gunakan tunnel ID unik untuk tiap koneksi

---

## 📚 Referensi Resmi

- [MikroTik EoIP Documentation](https://help.mikrotik.com/docs/display/ROS/EoIP+Tunnel)
- MikroTik Wiki: https://wiki.mikrotik.com/wiki/Manual:EoIP

---

Created by: byalak (https://github.com/byalak)