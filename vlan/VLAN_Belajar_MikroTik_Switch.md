
# 📘 Dokumentasi Belajar VLAN MikroTik + Switch + 2 PC

## 🧠 Apa Itu VLAN?

**VLAN (Virtual Local Area Network)** adalah teknologi jaringan Layer 2 yang digunakan untuk memisahkan jaringan secara logis dalam satu switch fisik. Dengan VLAN, kamu bisa membuat beberapa jaringan yang **terisolasi** walaupun menggunakan **perangkat fisik yang sama**.

---

## ✅ Manfaat VLAN

- Memisahkan jaringan antar departemen (HR, Finance, IT)
- Meningkatkan keamanan (tidak semua user bisa saling akses)
- Mengurangi broadcast domain
- Pengelolaan jaringan jadi lebih fleksibel dan scalable

---

## 🧭 Topologi Belajar

```
[Internet]
    |
 [Router MikroTik]
      |
    [Switch]
   /       \
[PC1]     [PC2]

PC1 (VLAN 10) → 192.168.10.0/24
PC2 (VLAN 20) → 192.168.20.0/24
Router → VLAN 10 & VLAN 20 gateway
```

---

## 🧱 Alat yang Digunakan

- 1 Router MikroTik (terhubung ke internet)
- 1 Switch manageable (Layer 2 VLAN support)
- 2 PC/laptop/VM
- GNS3 atau fisik

---

## ⚙️ Step-by-Step Konfigurasi

### 🔹 1. Buat Sub-interface VLAN di MikroTik

```bash
/interface vlan
add name=vlan10 vlan-id=10 interface=ether2
add name=vlan20 vlan-id=20 interface=ether2

/ip address
add address=192.168.10.1/24 interface=vlan10
add address=192.168.20.1/24 interface=vlan20
```

*ether2 = port ke arah switch*

---

### 🔹 2. Konfigurasi DHCP Server (Opsional)

```bash
/ip pool
add name=pool-vlan10 ranges=192.168.10.10-192.168.10.100
add name=pool-vlan20 ranges=192.168.20.10-192.168.20.100

/ip dhcp-server
add name=dhcp-vlan10 interface=vlan10 address-pool=pool-vlan10 lease-time=1h
add name=dhcp-vlan20 interface=vlan20 address-pool=pool-vlan20 lease-time=1h

/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1
add address=192.168.20.0/24 gateway=192.168.20.1
```

---

### 🔹 3. Konfigurasi Port Switch

**VLAN Access Port:**
- Port ke PC1 → VLAN 10 (Access)
- Port ke PC2 → VLAN 20 (Access)

**VLAN Trunk Port:**
- Port ke MikroTik (ether2) → Trunk (Tag VLAN 10 & 20)

Contoh konfigurasi jika menggunakan switch Cisco:

```bash
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20

interface FastEthernet0/24
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 10,20
```

---

### 🔹 4. Cek Hasil

- PC1 seharusnya dapat IP `192.168.10.x`
- PC2 seharusnya dapat IP `192.168.20.x`
- Ping dari PC1 ke PC2 harus **gagal** (karena beda VLAN)
- Ping dari kedua PC ke gateway-nya masing-masing harus **sukses**

---

## 🔒 Tips Keamanan VLAN

- Jangan gabungkan user berbeda VLAN tanpa firewall
- Gunakan switch manageable yang mendukung isolasi port
- Gunakan firewall MikroTik untuk kontrol akses antar VLAN jika dibutuhkan

---

## 📝 Penutup

Dengan setup sederhana 1 router + 1 switch + 2 PC ini, kamu sudah bisa memahami bagaimana VLAN bekerja untuk segmentasi jaringan. Ini dasar yang penting banget kalau nanti mau bikin jaringan ISP, gedung, atau kantor berskala besar.

