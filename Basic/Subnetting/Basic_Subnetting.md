# Simulasi Jaringan VLSM dengan Cisco Packet Tracer
Halo semuanya! Di artikel ini aku mau sharing proyek simulasi jaringan pakai VLSM (Variable Length Subnet Masking) yang aku kerjain di Cisco Packet Tracer. Topologinya simpel tapi cukup menggambarkan penerapan konsep subnetting secara efisien.

## Topologi Jaringan

<img width="936" height="625" alt="image" src="https://github.com/user-attachments/assets/e2f8aaad-73b8-4dcd-857c-a4bd7e408043" />

Disini kita mempunyai base network 192.168.14.0/24 dan akan kita bagi di tiga gedung (A, B, dan C). Dimana gedung A mempunyai 5 client, gedung B mempunyai 20 client, dan gedung C mempunyai 59 client. Untuk menguhubungkan antar gedung kita menggunakan 3 router dengan subnet point-to-point (masing-masing /30).

## Menghitung Subnet
Untuk menghitungnya kita mulai dari gedung yang mempunyai kebutuhan paling banyak sampai ke yang paling sedikit.

<img width="1200" height="359" alt="image" src="https://github.com/user-attachments/assets/f2714c34-ab62-498c-a296-2e2253e841fc" />

## Konfigurasi Router
Disini aku menggunakan routing static biar simpel dan jelas.
**Router 1**
```yaml
Router>ena
Router#conf t
Router(config)#int g0/0 
Router(config-if)#ip address 192.168.14.97 255.255.255.248
Router(config-if)#no sh
Router(config-if)#int s0/0/0
Router(config-if)#ip address 192.168.14.105 255.255.255.252
Router(config-if)#clock rate 64000
Router(config-if)#no sh
Router(config-if)#int s0/0/1
Router(config-if)#ip address 192.168.14.109 255.255.255.252
Router(config-if)#clock rate 64000
Router(config-if)#no sh
Router(config)#ip route 192.168.14.0 255.255.255.248 192.168.14.110
Router(config)#ip route 192.168.14.112 255.255.255.252 192.168.14.110
Router(config)#ip route 192.168.14.64 255.255.255.224 192.168.14.106
Router(config)#do wr
Building configuration…
[OK]
```

**Router 2**
```yaml
Router>ena
Router#conf t
Router(config)#int g0/0 
Router(config-if)#ip address 192.168.14.65 255.255.255.224
Router(config-if)#no sh
Router(config-if)#int s0/0/0
Router(config-if)#ip address 192.168.14.106 255.255.255.252
Router(config-if)#clock rate 64000
Router(config-if)#no sh
Router(config-if)#int s0/0/1
Router(config-if)#ip address 192.168.14.113 255.255.255.252
Router(config-if)#clock rate 64000
Router(config-if)#no sh
Router(config)#ip route 192.168.14.96 255.255.255.248 192.168.14.105
Router(config)#ip route 192.168.14.108 255.255.255.252 192.168.14.105
Router(config)#ip route 192.168.14.0 255.255.255.248 192.168.14.114
Router(config)#do wr
Building configuration…
[OK]
```

**Router 3**
```yaml
Router>ena
Router#conf t
Router(config)#int g0/0 
Router(config-if)#ip address 192.168.14.1 255.255.255.192
Router(config-if)#no sh
Router(config-if)#int s0/0/0
Router(config-if)#ip address 192.168.14.114 255.255.255.252
Router(config-if)#clock rate 64000
Router(config-if)#no sh
Router(config-if)#int s0/0/1
Router(config-if)#ip address 192.168.14.110 255.255.255.252
Router(config-if)#clock rate 64000
Router(config-if)#no sh
Router(config)#ip route 192.168.14.64 255.255.255.224 192.168.14.113
Router(config)#ip route 192.168.14.104 255.255.255.252 192.168.14.113
Router(config)#ip route 192.168.14.96 255.255.255.248 192.168.14.109
Router(config)#do wr
Building configuration…
[OK]
```

## Cek koneksi
**Gedung A**
Sebelum nge-ping kita setup IP PC terlebih dahulu dengan IP static:

<img width="1040" height="792" alt="image" src="https://github.com/user-attachments/assets/05141ee0-e1f7-49e1-8cab-86d3bf6890e9" />

Ping ke client gedung B dan C:

<img width="1042" height="967" alt="image" src="https://github.com/user-attachments/assets/f0251ffe-56af-4a46-9486-a08c9421a10f" />

**Gedung B**
Setup IP PC:

<img width="1046" height="967" alt="image" src="https://github.com/user-attachments/assets/0d955d06-21c7-4e0b-8739-5c5452c1d518" />

Nge-ping Gedung A dan C:

<img width="1034" height="969" alt="image" src="https://github.com/user-attachments/assets/a153d9cc-abd5-4f99-b7dc-6c2bc608513c" />

**Gedung C**
Setup IP PC:

<img width="1043" height="971" alt="image" src="https://github.com/user-attachments/assets/cab3ee5e-d215-441a-988c-193a92adbf98" />


Nge-ping Gedung A dan B:

<img width="1047" height="966" alt="image" src="https://github.com/user-attachments/assets/d8ccedd7-73c9-49f8-89e5-340f487588ce" />


`**File Proyek : vlsm.pkt**`
