
## Apa Itu IPIP Tunnel?

**IPIP (IP over IP)** adalah salah satu jenis tunneling di MikroTik yang memungkinkan kita mengenkapsulasi paket IP di dalam paket IP lain. Tunnel ini bersifat **Layer 3 (network layer)** dan cocok digunakan untuk:

- Menghubungkan dua jaringan berbeda melalui internet
- Menghubungkan router dengan IP publik masing-masing
- Membuat jalur komunikasi privat antar site
- Transportasi protokol routing seperti OSPF, BGP, dsb.

---

## Opsi-Opsi pada IPIP Tunnel

| Opsi              | Penjelasan |
|-------------------|------------|
| **Name**          | Nama interface tunnel |
| **Remote Address**| IP publik router tujuan |
| **Local Address** | IP publik router kita sendiri *(bisa dikosongkan untuk otomatis)* |
| **MTU**           | Maximum Transmission Unit (default 1480) |
| **Clamp TCP MSS** | Menyesuaikan MSS secara otomatis agar menghindari fragmentasi |
| **Allow Fast Path** | Aktifkan FastTrack (jika routing tidak terlalu kompleks) |

---

## Topology

![image](https://github.com/user-attachments/assets/9124c347-ecdc-47ea-accc-dc1dab577ca9)

---

## Step-by-step Konfigurasi IPIP Tunnel

### Di Router 1
**Create Interface IPIP Tunnel**

di `Interfaces > IPIP Tunnel > +`

![image](https://github.com/user-attachments/assets/ed2d6696-1958-4496-a077-37dce4d4ff09)

**Create Ip Address IPIP Tunnel**

di `IP > Addresses > +`

![image](https://github.com/user-attachments/assets/ca0c4c53-9cab-4b0c-a983-532e45257072)

**Routing**

di `IP > Routes > +`

![image](https://github.com/user-attachments/assets/d3e0b557-24a2-4c18-9b94-b7bdc468e094)

---

### Di Router 2

**Create Interface IPIP Tunnel**

di `Interfaces > IPIP Tunnel > +`

![image](https://github.com/user-attachments/assets/5753530b-f1bc-467e-b9fb-2f2fbf4d6ff4)

**Create Ip Address IPIP Tunnel**

di `IP > Addresses > +`

![image](https://github.com/user-attachments/assets/7d95e457-18d5-4b72-b0ba-6ed625229ef7)

**Routing**

di `IP > Routes > +`

![image](https://github.com/user-attachments/assets/5ef2fc7b-8735-4433-a637-ca4093fcce2a)


---

## Tes Koneksi

dari **PC1**

![image](https://github.com/user-attachments/assets/c45a64a6-8fba-4645-a8a4-f84a50f5435e)

dari **PC2**

![image](https://github.com/user-attachments/assets/3c6196f2-7a04-4c66-a398-d953c5e7e7d4)

---

## Catatan Penting

- IPIP membutuhkan IP publik di kedua sisi.
- Jika ada NAT di tengah, pertimbangkan pakai GRE over IPsec atau L2TP/IPsec.
- Jangan lupa cek `firewall > filter` dan `IP > route`.
