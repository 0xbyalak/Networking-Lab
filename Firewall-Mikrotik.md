## Firewall di MikroTik
Firewall di MikroTik adalah sistem penyaring lalu lintas jaringan (network traffic) yang berfungsi untuk **mengontrol akses**, **melindungi jaringan**, dan **mendeteksi/menangkal ancaman** berdasarkan **aturan (rules)** yang dibuat pengguna.

Firewall ini bekerja di atas **Netfilter**, dan bisa diakses melalui menu:

```
/ip firewall
```

## **Fungsi Firewall MikroTik**

*  **Menjaga keamanan jaringan**
*  **Mengatur lalu lintas berdasarkan IP, port, protokol, dan lainnya**
*  **Memblokir serangan (DoS, brute force, scan, dll.)**
*  **Melacak koneksi aktif (connection tracking)**
*  **Mengatur QoS (Quality of Service) dengan mangle**
*  **Redirect trafik (misal ke proxy internal)**
*  **Memfilter trafik berdasarkan layer 3 dan 4 (IP dan port)**



## **Menu Firewall di MikroTik**

### **Filter Rules**

Digunakan untuk **mengizinkan (accept), memblokir (drop), atau memproses trafik** berdasarkan kondisi tertentu.

Contoh rule:

```bash
/ip firewall filter add chain=input protocol=tcp dst-port=22 action=drop src-address=192.168.1.7
```

### **NAT (Network Address Translation)**

Digunakan untuk **mengubah alamat IP sumber atau tujuan**, umumnya untuk internet sharing dan port forwarding.

Contoh rule:

```bash
/ip firewall nat add chain=srcnat out-interface=ether1 action=masquerade
```

### **Mangle**

Digunakan untuk **menandai** trafik (packet, connection, routing, dll.) untuk keperluan routing policy, QoS, atau statistik.

Contoh:

```bash
/ip firewall mangle add chain=prerouting src-address=192.168.1.0/24 action=mark-routing new-routing-mark=via-wifi
```

### **Raw**

Berfungsi untuk memproses trafik **sebelum masuk ke connection tracking** — digunakan untuk optimalisasi atau blocking awal (misal blok DoS awal).

Contoh:

```bash
/ip firewall raw add chain=prerouting src-address=1.2.3.4 action=drop
```

### **Address Lists**

Digunakan untuk membuat daftar IP (manual atau otomatis) yang bisa digunakan di semua rule.

Contoh menambahkan:

```bash
/ip firewall address-list add list=blacklist address=192.168.1.100
```

## **Chain di Firewall MikroTik**

### Chain `input`

* Mengatur **trafik yang ditujukan ke router itu sendiri**
* Misal: akses SSH, Winbox, ping ke router

### Chain `forward`

* Mengatur **trafik yang melewati router**, dari satu interface ke interface lain
* Misal: trafik dari LAN ke Internet

### Chain `output`

* Mengatur **trafik yang keluar dari router itu sendiri**
* Misal: update DNS, NTP, check software update

## **Struktur rule**
Di MikroTik, urutan rule firewall itu sangat penting, karena dia berjalan dari atas ke bawah dan akan berhenti pada rule pertama yang cocok (first match wins).
```
1. rule: accept kondisi tertentu
2. rule: drop semua sisanya
```
Atau sebaliknya.

## **Action pada Firewall Rules**

| Action                    | Fungsi                                              |
| ------------------------- | --------------------------------------------------- |
| `accept`                  | Mengizinkan paket lewat                             |
| `drop`                    | Menolak paket tanpa memberi respon                  |
| `reject`                  | Menolak paket dan memberi respon (misal ICMP error) |
| `log`                     | Mencatat paket ke log MikroTik                      |
| `add-src-to-address-list` | Menambahkan IP sumber ke Address List               |
| `tarpit`                  | Memerangkap koneksi TCP (untuk slow down attacker)  |


## **Connection Tracking**

Fitur `connection tracking` memungkinkan MikroTik mengetahui **status koneksi** (new, established, related, invalid).

Contoh penggunaan di filter:

```bash
/ip firewall filter add chain=forward connection-state=established action=accept
/ip firewall filter add chain=forward connection-state=related action=accept
/ip firewall filter add chain=forward connection-state=invalid action=drop
```
## **Perintah CLI Penting**

```bash
/ip firewall filter print                     ← Lihat semua rule filter
/ip firewall filter add ...                   ← Tambah rule baru
/ip firewall nat print                        ← Lihat semua NAT
/ip firewall address-list print               ← Lihat daftar IP list
/ip firewall mangle print                     ← Lihat mangle rule
/ip firewall connection print                 ← Lihat koneksi aktif
```
