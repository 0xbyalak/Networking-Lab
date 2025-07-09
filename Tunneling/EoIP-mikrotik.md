## Apa itu EoIP?

**EoIP (Ethernet over IP)** adalah protokol tunneling yang dikembangkan oleh MikroTik untuk mengenkapsulasi frame Layer 2 (Ethernet) ke dalam paket IP, sehingga memungkinkan koneksi Layer 2 (bridge) antar dua lokasi berbeda melalui jaringan IP seperti Internet.

> Tujuan utama EoIP:
> - Menghubungkan dua jaringan LAN secara Layer 2 seolah-olah berada di jaringan yang sama.
> - Cocok untuk bridging VLAN, Hotspot roaming, atau site-to-site dengan kebutuhan broadcast Layer 2.


## Karakteristik EoIP

- Protokol eksklusif MikroTik (hanya bisa antar MikroTik)
- Bisa dibridge dengan interface lokal
- Mendukung VLAN dan broadcast traffic
- Tidak terenkripsi (disarankan digabung dengan IPsec)

---

## Opsi Konfigurasi EoIP

| Opsi | Penjelasan |
|------|------------|
| `Remote Address` | IP publik dari router lawan (peer) |
| `Tunnel ID` | Angka identifikasi tunnel (harus sama di kedua sisi) |
| `Local Address` | IP punlik router pengirim |
| `Keepalive` | Timeout dan interval untuk deteksi status tunnel |
| `MTU` | Ukuran maksimum frame yang bisa lewat (default 1500 - overhead) |

---

## Step-by-Step Konfigurasi EoIP Tunnel

### Topology

![image](https://github.com/user-attachments/assets/5955d5ba-7e63-4443-807e-0f170c9f41d2)

---

### 1. Buat EoIP di **Router 1**
di `Interfaces > EoIP Tunnel > +`

![image](https://github.com/user-attachments/assets/461c2dec-4cc8-4228-b90c-9f6623f3a075)

---

### 2. Buat EoIP di **Router 2**
di `Interfaces > EoIP Tunnel > +`

![image](https://github.com/user-attachments/assets/11b22cfa-bee3-4469-908d-9518dd7ab73c)

---

### 3. Bridge Interface Lokal di **Router 1**
**buat interface bridge**

di `Bridge > +`

![image](https://github.com/user-attachments/assets/0dcae9d2-266a-481b-bb51-c58ce60a87ed)

**Masukkan interface lokal dan EoIP ke dalam bridge**

di `bridge > Ports > +`

![image](https://github.com/user-attachments/assets/6085b351-cec2-4914-ac35-bfbf5c454c8f)


---

### 4. Bridge Interface Lokal di **Router 2**
**Buat interface bridge**

di `Bridge > +`

![image](https://github.com/user-attachments/assets/0dcae9d2-266a-481b-bb51-c58ce60a87ed)

**Masukkan interface lokal dan EoIP ke dalam bridge**

di `bridge > Ports > +`

![image](https://github.com/user-attachments/assets/6085b351-cec2-4914-ac35-bfbf5c454c8f)

---

### 5. Tes koneksi dari **PC**

![image](https://github.com/user-attachments/assets/62214aec-9ea7-46ee-a8f6-bcb22829fb6b)

![image](https://github.com/user-attachments/assets/e8200fdd-878f-40cb-b3f6-18e8762ee274)

## Tips Keamanan

- Gunakan IPsec untuk mengenkripsi EoIP (karena EoIP sendiri tidak aman)
- Batasi akses port EoIP (47) di firewall
- Gunakan tunnel ID unik untuk tiap koneksi
