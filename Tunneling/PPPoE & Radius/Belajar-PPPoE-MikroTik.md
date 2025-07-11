
# Belajar PPPoE dengan MikroTik (Server, Client, dan PC)

## 1. Apa Itu PPPoE?

**PPPoE (Point-to-Point Protocol over Ethernet)** adalah protokol yang memungkinkan koneksi point-to-point melalui media Ethernet. Protokol ini sering digunakan oleh ISP untuk mengautentikasi pelanggan menggunakan username dan password, serta memberikan IP address secara dinamis atau statis.

---

## 2. Manfaat PPPoE

- Mengelola koneksi per pelanggan
- Otentikasi pengguna via username/password
- Bisa integrasi dengan RADIUS server
- Bisa diberi limitasi bandwidth per user
- Alokasi IP otomatis melalui profile

---

## 3. Topologi Jaringan

```
[ Internet ]
     |
 [Router1] ⇽⇾ PPPoE Server
     |
  ether2 (PPPoE)
     |
 [Router2] ⇽⇾ PPPoE Client
     |
  ether2
     |
    [PC]
```

---

## 4. Alokasi IP dan Peran

| Perangkat | Interface      | Peran            | IP Address / Keterangan     |
|-----------|----------------|------------------|-----------------------------|
| Router1   | ether1         | Internet          | DHCP / Static dari ISP     |
| Router1   | ether2         | PPPoE Server      | IP Pool 10.10.10.0/24       |
| Router2   | ether1         | PPPoE Client      | PPPoE Dial ke Router1       |
| Router2   | ether2         | LAN to PC         | 192.168.1.1/24              |
| PC        | -              | Client LAN        | 192.168.1.10/24             |

---

## 5. Konfigurasi PPPoE Server (Router1)

### 5.1 Tambahkan IP Pool
```bash
/ip pool
add name=pppoe-pool ranges=10.10.10.2-10.10.10.100
```

### 5.2 Buat PPP Profile
```bash
/ppp profile
add name=pppoe-profile local-address=10.10.10.1 remote-address=pppoe-pool dns-server=8.8.8.8 use-encryption=no
```

### 5.3 Tambah PPPoE Server pada interface ether2
```bash
/interface pppoe-server server
add interface=ether2 service-name=pppoe-server disabled=no authentication=pap,chap default-profile=pppoe-profile
```

### 5.4 Tambah PPPoE User
```bash
/ppp secret
add name=user1 password=123456 service=pppoe profile=pppoe-profile
```

---

## 6. Konfigurasi PPPoE Client (Router2)

### 6.1 Tambah PPPoE Client di ether1
```bash
/interface pppoe-client
add name=pppoe-out1 interface=ether1 user=user1 password=123456 disabled=no add-default-route=yes use-peer-dns=yes
```

---

## 7. Konfigurasi LAN di Router2

```bash
/ip address
add address=192.168.1.1/24 interface=ether2

/ip pool
add name=lan-pool ranges=192.168.1.10-192.168.1.100

/ip dhcp-server
add name=lan-dhcp interface=ether2 address-pool=lan-pool disabled=no

/ip dhcp-server network
add address=192.168.1.0/24 gateway=192.168.1.1 dns-server=8.8.8.8
```

---

## 8. Pengujian

- Cek di Router1 → `/ppp active` apakah user `user1` sedang aktif
- Dari PC → pastikan dapet IP DHCP dari Router2
- Dari PC → test ping ke 8.8.8.8 → harus berhasil
- Dari Router1 → test lihat `pppoe-out1` status dan log koneksi

---

## 9. Kesimpulan

Dengan konfigurasi ini, kita telah berhasil membuat lab PPPoE yang terdiri dari:
- 1 Router sebagai PPPoE Server
- 1 Router sebagai PPPoE Client
- 1 PC sebagai end user
Konfigurasi ini cocok digunakan sebagai dasar untuk setup ISP skala kecil atau simulasi sistem login berbasis protokol Point-to-Point.

---

**Penulis:**  
byalak | Dokumentasi PPPoE MikroTik | 2025
