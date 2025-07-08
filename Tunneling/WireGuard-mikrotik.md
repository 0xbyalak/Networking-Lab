# Apa itu WireGuard?

[WireGuard](https://www.wireguard.com/) adalah protokol VPN generasi baru yang mengutamakan kesederhanaan, performa tinggi, dan keamanan modern. Dibanding OpenVPN atau IPsec, WireGuard lebih ringan dan mudah dikonfigurasi karena hanya terdiri dari sekitar 4.000 baris kode saja.

## Keunggulan WireGuard:
-  Sangat cepat dan ringan (berjalan di kernel)
-  Menggunakan kriptografi modern (Curve25519, ChaCha20, Poly1305)
-  Konfigurasi lebih sederhana
-  Support di berbagai platform (Linux, MikroTik, Android, Windows, iOS, macOS)

---

# Penjelasan di MikroTik WireGuard

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

# Topologi Site-to-Site VPN (WireGuard)

![image](https://github.com/user-attachments/assets/1f3ceb5a-2608-4346-a1aa-15876ca448c2)

# Langkah-langkah Setup WireGuard MikroTik (Site-to-Site)

## 1. Buat interface Wireguard
### Di **Router A**:

di `wireguard > +` 

![image](https://github.com/user-attachments/assets/eff1b5dc-bb52-4f46-acf9-c2cada01afc1)

di `Addresses > +` 

![image](https://github.com/user-attachments/assets/ce21d115-a5ba-470d-be66-6a4e1974b31d)

### Di **Router B**:

di `wireguard > +`

![image](https://github.com/user-attachments/assets/479776dd-27a0-47ec-94d7-d0fc0175c367)

di `Addresses > +`

![image](https://github.com/user-attachments/assets/b8131630-1d64-4349-bfe2-51c79f2fde8a)

---

## 2. Tambahkan Peer

### Di **Router A**:
di `Wireguard > peers > +`

![image](https://github.com/user-attachments/assets/16b62d71-0936-4b3d-935e-0777e413b70b)

- `Public key`     : Public key router 2
- `Endpoint`       : Ip public router 2
- `Endpoint port`  : Listen port router 2
- `Allowed address`: Interface router 2 yang diijinkan
- `persistent-keepalive`: Jeda untuk cek koneksi

### Di **Router B**:
di `Wireguard > peers > +`

![image](https://github.com/user-attachments/assets/9fed1043-a51d-45e2-a4c8-c76ca027bc6c)

- `Public key`     : Public key router 2
- `Endpoint`       : Ip public router 2
- `Endpoint port`  : Listen port router 2
- `Allowed address`: Interface router 2 yang diijinkan
- `persistent-keepalive`: Jeda untuk cek koneksi

---

## 3. Tambahkan Routing ke Jaringan Peer

### Di Router A:

![image](https://github.com/user-attachments/assets/4d481c63-736d-4713-bc51-c2818e09e961)

### Di Router B:

![image](https://github.com/user-attachments/assets/b3edf99a-ea73-4155-90dd-5937780a723e)

---

## 4. Pastikan Firewall Mengizinkan Port

Tambahkan rule di firewall MikroTik:
```bash
/ip firewall filter add chain=input action=accept protocol=udp dst-port=51820
```

---

# Verifikasi Koneksi

Cek status peer:

![image](https://github.com/user-attachments/assets/99d9b269-884f-4043-81b8-3f841f0f7ab5)

Jika sukses:
- Ada **Last Handshake** (bukan `00:00:00`)
- Rx dan Tx bertambah

Tes ping antar PC:

`PC1`:

![image](https://github.com/user-attachments/assets/f76dc1d5-b547-44ec-898d-6449157f8167)

`PC2`:

![image](https://github.com/user-attachments/assets/212ab0f6-0cca-4169-862f-c4b6514c03f8)

---

# Troubleshooting

| Masalah | Penyebab Umum | Solusi |
|--------|----------------|--------|
| `host unreachable` | Routing tidak ada / salah | Tambahkan manual `/ip route` |
| Handshake 00:00:00 | Salah endpoint / public key / firewall | Periksa ulang config dan port |
| Tidak bisa akses LAN | `Allowed Address` kurang lengkap | Tambahkan subnet LAN ke field tersebut |
| Rx/Tx = 0 | NAT atau firewall drop | Tambahkan `persistent-keepalive=25` |
