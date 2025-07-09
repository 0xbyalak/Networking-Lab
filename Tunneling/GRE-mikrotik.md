## Apa Itu GRE Tunnel?

**GRE (Generic Routing Encapsulation)** adalah protokol tunneling Layer 3 yang digunakan untuk mengenkapsulasi berbagai protokol jaringan dalam koneksi point-to-point. GRE digunakan untuk membentuk jalur privat antar-router melalui jaringan publik seperti internet.

GRE sangat berguna untuk:

- Menghubungkan dua jaringan melalui internet
- Mengangkut routing dinamis (OSPF, BGP)
- Membuat jalur backup antar lokasi
- Dapat digabungkan dengan IPsec untuk keamanan

---

## Opsi-Opsi GRE Tunnel di MikroTik

| Opsi              | Penjelasan |
|-------------------|------------|
| **Name**          | Nama interface tunnel |
| **Remote Address**| IP publik router tujuan |
| **Local Address** | IP publik router kita sendiri *(bisa dikosongkan untuk otomatis)* |
| **Keepalive**     | Paket untuk mengecek apakah tunnel masih hidup |
| **DSCP**          | Untuk QoS pada paket GRE |
| **Clamp TCP MSS** | Mengatur MSS agar tidak terjadi fragmentasi |
| **Allow Fast Path** | Mengaktifkan hardware acceleration (jika didukung) |

---

## Topology

![image](https://github.com/user-attachments/assets/e449ef8d-d5a2-4ac1-9646-612b40eafccb)

---

## Step-by-step Konfigurasi GRE Tunnel

### Di Router 1

**Create Interface GRE Tunnel**

di `Interfaces > GRE Tunnel > +`

![image](https://github.com/user-attachments/assets/8546ad77-1c28-450e-ba1b-f233b1855c87)

**Create Ip Tunnel**

di `IP > Addresses > +`

![image](https://github.com/user-attachments/assets/b5105cdb-0c1e-4092-a7b9-8e236f84fffc)

**Routing**

![image](https://github.com/user-attachments/assets/55f2cdd2-1472-4100-bdc6-a1d3014145cc)

---

### Di Router 2

**Create Interface GRE Tunnel**

di `Interfaces > GRE Tunnel > +`

![Screenshot 2025-07-09 220719](https://github.com/user-attachments/assets/0d03b3dd-3453-456d-bed3-39d7ab8be281)

**Create Ip Tunnel**

di `IP > Addresses > +`

![image](https://github.com/user-attachments/assets/5b3e3ac3-6538-4f76-8f01-dcb0aedaaaec)

**Routing**

![image](https://github.com/user-attachments/assets/8485625f-9fcf-4dd0-b74f-0e09749710d8)

---

## Tes Koneksi

dari **PC1**

![image](https://github.com/user-attachments/assets/e3ab6c6c-3df2-43ff-a2c6-4ca50552ddcb)

dari **PC2**

![image](https://github.com/user-attachments/assets/791ea233-7d57-4d0a-9929-e00a51481472)


---

## Catatan Tambahan

- GRE **tidak menyediakan enkripsi**. Jika butuh keamanan, gunakan GRE over IPsec.
- Jika router berada di balik NAT, GRE bisa bermasalah kecuali NAT mendukung GRE passthrough.
- Bisa digunakan untuk membawa routing dinamis seperti OSPF dan BGP antar router.
- GRE cocok untuk backbone antar site dengan kontrol penuh.
