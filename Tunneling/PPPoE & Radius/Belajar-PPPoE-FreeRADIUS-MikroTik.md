
# Belajar PPPoE + FreeRADIUS dengan MikroTik dan Ubuntu

## 1. Apa Itu PPPoE?

**PPPoE (Point-to-Point Protocol over Ethernet)** adalah protokol yang membungkus protokol PPP di atas Ethernet. Protokol ini memungkinkan otentikasi pengguna dan manajemen koneksi internet secara individual. Digunakan secara luas oleh ISP untuk melayani pelanggan rumahan dan bisnis.

---

## 2. Apa Itu FreeRADIUS?

**FreeRADIUS** adalah server RADIUS open-source yang berfungsi untuk otentikasi, otorisasi, dan akuntansi pengguna jaringan. Digunakan bersama dengan PPPoE untuk mengelola:
- Login pengguna
- Alokasi IP
- Pembatasan bandwidth
- Logging (accounting) koneksi

---

## 3. Topologi Jaringan

```
[ Internet ]
     |
 [Router1] ⇽⇾ PPPoE Server + RADIUS Client
     |
   ether2
     |
 [Router2] ⇽⇾ PPPoE Client
     |
   ether2
     |
     [PC]

[Ubuntu Server] ⇽⇾ FreeRADIUS Server (192.168.88.250)
```

---

## 4. Alokasi IP dan Peran

| Perangkat      | Interface   | Peran                | IP Address            |
|----------------|-------------|----------------------|------------------------|
| Router1        | ether1      | Internet              | DHCP/Static            |
| Router1        | ether2      | PPPoE Server          | 10.10.10.1             |
| Router2        | ether1      | PPPoE Client          | via PPPoE              |
| Router2        | ether2      | LAN ke PC             | 192.168.1.1/24         |
| PC             | -           | End user              | DHCP dari Router2      |
| Ubuntu Server  | ensX        | FreeRADIUS Server     | 192.168.88.250         |

---

## 5. Setup FreeRADIUS (Ubuntu Server)

### 5.1 Install FreeRADIUS
```bash
sudo apt update
sudo apt install freeradius -y
```

### 5.2 Tambahkan user di `/etc/freeradius/3.0/users`
```text
user1 Cleartext-Password := "123456"
```

### 5.3 Edit `/etc/freeradius/3.0/clients.conf`
```text
client mikrotik {
    ipaddr = 10.10.10.1
    secret = radius123
    require_message_authenticator = no
}
```

### 5.4 Restart dan cek status
```bash
sudo systemctl restart freeradius
sudo systemctl status freeradius
```

### 5.5 Tes menggunakan `radtest`
```bash
radtest user1 123456 127.0.0.1 0 radius123
```

---

## 6. Konfigurasi PPPoE Server di MikroTik (Router1)

### 6.1 IP dan PPPoE Setup
```bash
/ip address
add address=10.10.10.1/24 interface=ether2

/ip pool
add name=pppoe-pool ranges=10.10.10.2-10.10.10.100

/ppp profile
add name=pppoe-profile local-address=10.10.10.1 remote-address=pppoe-pool use-radius=yes
```

### 6.2 Tambah PPPoE Server
```bash
/interface pppoe-server server
add interface=ether2 service-name=pppoe-server disabled=no default-profile=pppoe-profile authentication=pap
```

---

## 7. Konfigurasi Radius Client di MikroTik (Router1)

```bash
/radius
add address=192.168.88.250 service=ppp secret=radius123

/radius incoming
set accept=yes
```

---

## 8. Konfigurasi PPPoE Client di Router2

```bash
/interface pppoe-client
add name=pppoe-out1 interface=ether1 user=user1 password=123456 add-default-route=yes use-peer-dns=yes disabled=no
```

---

## 9. Setup LAN di Router2

```bash
/ip address
add address=192.168.1.1/24 interface=ether2

/ip pool
add name=lan-pool ranges=192.168.1.10-192.168.1.100

/ip dhcp-server
add name=dhcp1 interface=ether2 address-pool=lan-pool disabled=no

/ip dhcp-server network
add address=192.168.1.0/24 gateway=192.168.1.1 dns-server=8.8.8.8
```

---

## 10. Pengujian

- Di Router1: `/ppp active` untuk lihat koneksi user1
- Di FreeRADIUS: `tail -f /var/log/freeradius/radius.log` untuk log otentikasi
- Dari PC: ping ke internet
- Tes Disconnect → radius akan mencatat akunting sesi

---

## 11. Best Practice & Standar Industri

✅ Gunakan PAP untuk keamanan minimum (CHAP lebih aman, tapi sesuaikan dengan dukungan)  
✅ IP pool diatur dari MikroTik, user hanya tersimpan di RADIUS  
✅ Selalu tes `radtest` sebelum integrasi dengan MikroTik  
✅ Gunakan RADIUS accounting untuk tracking user (tambahkan `accounting=yes`)  
✅ FreeRADIUS bisa diintegrasikan dengan database (MySQL/PostgreSQL) untuk skala besar  
✅ Gunakan VPN/IPSec untuk mengamankan koneksi antar site jika perlu

---

## 12. Kesimpulan

Dengan setup ini, kita membuat sistem otentikasi PPPoE skala kecil berbasis FreeRADIUS yang:
- Modular (bisa dikembangkan ke banyak client)
- Aman dan terstandarisasi
- Cocok untuk simulasi ISP atau lab manajemen koneksi user

---

**Penulis:**  
byalak | Dokumentasi PPPoE + FreeRADIUS MikroTik | 2025
