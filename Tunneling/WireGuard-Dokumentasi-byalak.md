
# 🔐 Dokumentasi Belajar WireGuard (MikroTik)

## 🧠 Apa itu WireGuard?

[WireGuard](https://www.wireguard.com/) adalah protokol VPN generasi baru yang mengutamakan kesederhanaan, performa tinggi, dan keamanan modern. Dibanding OpenVPN atau IPsec, WireGuard lebih ringan dan mudah dikonfigurasi karena hanya terdiri dari sekitar 4.000 baris kode saja.

### ✅ Keunggulan WireGuard:
- 🚀 Sangat cepat dan ringan (berjalan di kernel)
- 🔐 Menggunakan kriptografi modern (Curve25519, ChaCha20, Poly1305)
- 🧱 Konfigurasi lebih sederhana
- 📲 Support di berbagai platform (Linux, MikroTik, Android, Windows, iOS, macOS)

---

## ⚙️ Penjelasan Opsi di MikroTik WireGuard

| Field | Deskripsi |
|-------|-----------|
| **Interface** | Antarmuka virtual WireGuard (misal: `wireguard1`). |
| **Private Key** | Kunci privat router. Jangan dibagikan. |
| **Public Key** | Kunci publik router. Dibagikan ke peer. |
| **Listen Port** | Port yang digunakan WireGuard untuk menerima koneksi (default: `51820`). |
| **Peer** | Konfigurasi lawan komunikasi (router lain). |
| **Endpoint** | IP publik atau FQDN router peer (contoh: `203.0.113.2`). |
| **Endpoint Port** | Port peer yang digunakan untuk WireGuard. |
| **Allowed Address** | IP/subnet yang diizinkan melalui koneksi ini. |
| **Persistent Keepalive** | Paket "ping" setiap X detik untuk menjaga NAT tetap terbuka. |
| **Preshared Key** | Opsi tambahan untuk keamanan (shared secret selain keypair). |

---

## 🌐 Topologi Site-to-Site VPN (WireGuard)

```
+-----------+         VPN Tunnel         +-----------+
|  Router A |===========================>|  Router B |
|  192.168.1.0/24     10.0.0.1 <--> 10.0.0.2   192.168.2.0/24 |
+-----------+                            +-----------+
```

---

## 🛠️ Langkah-langkah Setup WireGuard MikroTik (Site-to-Site)

### 1. Generate Public/Private Key

Di terminal MikroTik:
```bash
/interface wireguard key
```

Salin:
- Private Key → untuk digunakan di interface
- Public Key → untuk dibagikan ke peer

---

### 2. Buat Interface WireGuard

#### Di **Router A**:
```bash
/interface wireguard add name=wg1 listen-port=51820 private-key="PRIVATE_KEY_A"
/ip address add address=10.0.0.1/30 interface=wg1
```

#### Di **Router B**:
```bash
/interface wireguard add name=wg1 listen-port=51820 private-key="PRIVATE_KEY_B"
/ip address add address=10.0.0.2/30 interface=wg1
```

---

### 3. Tambahkan Peer

#### Di **Router A**:
```bash
/interface wireguard peers add \
interface=wg1 \
public-key="PUBLIC_KEY_B" \
endpoint-address=203.0.113.2 \
endpoint-port=51820 \
allowed-address=10.0.0.2/32,192.168.2.0/24 \
persistent-keepalive=25
```

#### Di **Router B**:
```bash
/interface wireguard peers add \
interface=wg1 \
public-key="PUBLIC_KEY_A" \
endpoint-address=198.51.100.1 \
endpoint-port=51820 \
allowed-address=10.0.0.1/32,192.168.1.0/24 \
persistent-keepalive=25
```

---

### 4. Tambahkan Routing ke Jaringan Peer

#### Di Router A:
```bash
/ip route add dst-address=192.168.2.0/24 gateway=10.0.0.2
```

#### Di Router B:
```bash
/ip route add dst-address=192.168.1.0/24 gateway=10.0.0.1
```

---

### 5. Pastikan Firewall Mengizinkan Port

Tambahkan rule di firewall MikroTik:
```bash
/ip firewall filter add chain=input action=accept protocol=udp dst-port=51820
```

---

## ✅ Verifikasi Koneksi

Cek status peer:
```bash
/interface wireguard peers print
```

Jika sukses:
- Ada **Last Handshake** (bukan `00:00:00`)
- Rx dan Tx bertambah

Tes ping antar LAN:
```bash
ping 192.168.2.1
```

---

## 🧰 Troubleshooting

| Masalah | Penyebab Umum | Solusi |
|--------|----------------|--------|
| `host unreachable` | Routing tidak ada / salah | Tambahkan manual `/ip route` |
| Handshake 00:00:00 | Salah endpoint / public key / firewall | Periksa ulang config dan port |
| Tidak bisa akses LAN | `Allowed Address` kurang lengkap | Tambahkan subnet LAN ke field tersebut |
| Rx/Tx = 0 | NAT atau firewall drop | Tambahkan `persistent-keepalive=25` |

---

## 🔐 Contoh Ringkas Allowed Address

Di masing-masing peer, isi:
```text
10.0.0.x/32, 192.168.x.0/24
```

Contoh Router A ke B:
```
Allowed Address: 10.0.0.2/32, 192.168.2.0/24
```

Router B ke A:
```
Allowed Address: 10.0.0.1/32, 192.168.1.0/24
```

---

## 📚 Referensi

- 🌐 https://www.wireguard.com/
- 📖 https://help.mikrotik.com/docs/display/ROS/WireGuard
- 📓 https://wiki.archlinux.org/title/WireGuard

---

## 👨‍💻 Penulis

> Ditulis oleh: **byalak**  
> Tujuan: Dokumentasi belajar pribadi tentang setup VPN site-to-site menggunakan WireGuard di MikroTik.
