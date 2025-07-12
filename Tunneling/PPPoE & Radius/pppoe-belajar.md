# Dokumentasi Belajar PPPoE dengan MikroTik

## 📘 Apa itu PPPoE?
**PPPoE (Point-to-Point Protocol over Ethernet)** adalah protokol jaringan yang mengenkapsulasi PPP dalam Ethernet. Protokol ini umum digunakan oleh ISP untuk mengautentikasi pelanggan dan mengalokasikan IP dinamis/statik.

Keunggulan PPPoE:
- Mendukung autentikasi (PAP/CHAP)
- Alokasi IP otomatis per user
- Integrasi dengan **RADIUS** untuk manajemen user terpusat
- Cocok untuk ISP berbasis MikroTik

---

## 🧰 Topologi yang Digunakan

```
[Internet]
    |
[Router1] ⇐ PPPoE Server + Internet Access + RADIUS
    |
[Router2] ⇐ PPPoE Client
    |
   [PC]     ⇐ PPPoE Dial dari OS
```

---

## 🎯 Tujuan
- Konfigurasi PPPoE Server di Router1
- Konfigurasi PPPoE Client di Router2 dan PC
- Integrasi dengan RADIUS server

---

## 📍 Langkah-langkah Konfigurasi

### 🔧 A. Konfigurasi PPPoE Server (Router1)

#### 1. Setup koneksi internet
```bash
/ip dhcp-client add interface=ether1 disabled=no
```
*anggap ether1 menghadap ke internet*

#### 2. IP address lokal untuk klien PPPoE
```bash
/ip address add address=192.168.10.1/24 interface=ether2
```
*ether2 menghadap ke Router2 (client)*

#### 3. Buat PPPoE Server Interface
```bash
/interface pppoe-server server add name=pppoe-in1 interface=ether2 default-profile=default use-radius=yes
```

#### 4. Buat PPP Profile
```bash
/ppp profile add name=pppoe-profile local-address=192.168.10.1 remote-address=pppoe-pool use-radius=yes
```

#### 5. Buat IP Pool untuk Klien
```bash
/ip pool add name=pppoe-pool ranges=192.168.10.10-192.168.10.50
```

#### 6. Aktifkan RADIUS
```bash
/radius add service=ppp address=192.168.10.100 secret=testing123
/radius incoming set accept=yes
```

---

### 🔧 B. Konfigurasi PPPoE Client (Router2)

#### 1. Buat interface PPPoE Client
```bash
/interface pppoe-client add name=pppoe-out1 interface=ether1 user=test password=test use-peer-dns=yes add-default-route=yes disabled=no
```
*ether1 menghadap ke ether2 Router1*

#### 2. Cek IP
```bash
/ip address print
/ip route print
```
> Harus mendapatkan IP dari PPPoE dan default route

---

### 💻 C. Konfigurasi PPPoE Client di PC (Windows/Linux)

**Windows:**
- Buka *Network & Sharing Center > Set up a new connection > Connect to Internet > Broadband (PPPoE)*
- Masukkan username dan password (misalnya: `test` / `test`)
- Klik Connect

**Linux (CLI):**
```bash
sudo pppoeconf
# ikuti wizard untuk koneksi
```

---

### 🧩 D. Setup RADIUS Server (FreeRADIUS)

**Asumsi IP: 192.168.10.100**

1. Install FreeRADIUS (contoh di Ubuntu):
```bash
sudo apt update && sudo apt install freeradius -y
```

2. Tambah user di `/etc/freeradius/3.0/users`:
```
test Cleartext-Password := "test"
    Service-Type = Framed-User,
    Framed-Protocol = PPP,
    Framed-IP-Address = 192.168.10.20,
    Framed-IP-Netmask = 255.255.255.0
```

3. Tambah MikroTik di `/etc/freeradius/3.0/clients.conf`:
```
client mikrotik {
    ipaddr = 192.168.10.1
    secret = testing123
}
```

4. Restart FreeRADIUS:
```bash
sudo systemctl restart freeradius
```

5. Cek log:
```bash
tail -f /var/log/freeradius/radius.log
```

---

## ✅ Verifikasi & Testing

- Dari Router2 dan PC, pastikan bisa mendapatkan IP dari PPPoE dan akses internet.
- Dari Router1, cek user aktif:
```bash
/ppp active print
```
- Cek log RADIUS jika terjadi error autentikasi.

---

## 📌 Tips Tambahan
- Gunakan `rate-limit` di PPP profile untuk membatasi bandwidth per user.
- Jika client gagal konek, cek RADIUS log dan IP Pool.
- Gunakan `Secrets` di PPP untuk testing lokal jika tanpa RADIUS.

---

## 🧠 Penutup
Dokumentasi ini menjelaskan konsep dan implementasi PPPoE dengan integrasi RADIUS menggunakan MikroTik. Cocok untuk belajar dasar sistem autentikasi jaringan berbasis PPP dan sentralisasi user management.