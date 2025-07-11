
# Belajar VLAN dengan MikroTik (Router Internet + VLAN + Trunking)

## 1. Apa Itu VLAN?

**VLAN (Virtual Local Area Network)** adalah metode segmentasi jaringan secara logis pada perangkat fisik yang sama. VLAN memungkinkan pemisahan broadcast domain dan meningkatkan efisiensi serta keamanan jaringan.

---

## 2. Manfaat VLAN

- Memisahkan jaringan berdasarkan fungsi/divisi
- Meningkatkan keamanan jaringan
- Mengurangi broadcast
- Memudahkan pengelolaan jaringan

---

## 3. Topologi Jaringan

```
        [ Internet ]
            |
        [Router1]  ⇽⇾  Buat VLAN
            |
        trunk (ether2)
            |
        [Router2] ⇽⇾ Trunking dan Tagging
        /       \
     (ether3)  (ether4)
     [PC1]     [PC2]

Keterangan:
- Router1 = Gateway + Buat VLAN
- Router2 = Switch Trunking VLAN
- PC1 = VLAN 10
- PC2 = VLAN 20
```

---

## 4. Pembagian VLAN

| VLAN ID | Nama     | Interface | IP Subnet         |
|---------|----------|-----------|-------------------|
| 10      | VLAN-IT  | ether3    | 192.168.10.0/24   |
| 20      | VLAN-HR  | ether4    | 192.168.20.0/24   |

---

## 5. IP Addressing Plan

- **Router1 (Gateway + VLAN Creator)**:
  - ether1: DHCP (Internet)
  - ether2: trunk ke Router2
  - VLAN10: 192.168.10.1/24
  - VLAN20: 192.168.20.1/24

- **Router2 (Trunk Receiver + Access VLAN)**:
  - ether1: trunk (tagged VLAN10 dan VLAN20)
  - ether3: Access VLAN10
  - ether4: Access VLAN20

- **PC1**: 192.168.10.10/24
- **PC2**: 192.168.20.10/24

---

## 6. Konfigurasi Step-by-Step

### 6.1 Router1 - Buat VLAN + Internet

```bash
/ip dhcp-client add interface=ether1

/interface vlan
add name=vlan10 vlan-id=10 interface=ether2
add name=vlan20 vlan-id=20 interface=ether2

/ip address
add address=192.168.10.1/24 interface=vlan10
add address=192.168.20.1/24 interface=vlan20

/ip firewall nat
add chain=srcnat out-interface=ether1 action=masquerade
```

---

### 6.2 Router2 - Trunk Receiver + Access VLAN

```bash
/interface vlan
add name=vlan10 vlan-id=10 interface=ether1
add name=vlan20 vlan-id=20 interface=ether1

/interface bridge
add name=bridge10
add name=bridge20

/interface bridge port
add bridge=bridge10 interface=ether3
add bridge=bridge10 interface=vlan10
add bridge=bridge20 interface=ether4
add bridge=bridge20 interface=vlan20

/ip address
add address=192.168.10.2/24 interface=bridge10
add address=192.168.20.2/24 interface=bridge20
```

---

## 7. Konfigurasi PC

- **PC1 (VLAN10)**:
  - IP: 192.168.10.10/24
  - Gateway: 192.168.10.1

- **PC2 (VLAN20)**:
  - IP: 192.168.20.10/24
  - Gateway: 192.168.20.1

---

## 8. Pengujian

- Ping dari PC1 ke PC2 → **tidak bisa (terpisah VLAN)**
- Ping dari PC1 dan PC2 ke 8.8.8.8 → **berhasil (akses internet)**
- Ping dari PC ke gateway masing-masing → **berhasil**

---

## 9. Kesimpulan

Dokumentasi ini menunjukkan bagaimana membuat dan menghubungkan VLAN antar-router menggunakan trunking dan tagging di MikroTik. Konsep ini sangat penting dalam jaringan menengah-besar untuk efisiensi dan keamanan.

---

**Penulis**:  
byalak | VLAN MikroTik | 2025
