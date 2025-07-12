## Apa Itu PPPoE?

**PPPoE (Point-to-Point Protocol over Ethernet)** adalah protokol yang memungkinkan koneksi point-to-point melalui media Ethernet. Protokol ini sering digunakan oleh ISP untuk mengautentikasi pelanggan menggunakan username dan password, serta memberikan IP address secara dinamis atau statis.

## Manfaat PPPoE

- Mengelola koneksi per pelanggan
- Otentikasi pengguna via username/password
- Bisa integrasi dengan RADIUS server
- Bisa diberi limitasi bandwidth per user
- Alokasi IP otomatis melalui profile

## Topology

<img width="1919" height="919" alt="Screenshot 2025-07-11 184636" src="https://github.com/user-attachments/assets/9a15fb8f-f82c-4392-a401-c13ff781912a" />

## Alokasi IP

| Perangkat | Interface      | Peran             | IP Address / Keterangan     |
|-----------|----------------|-------------------|-----------------------------|
| Router1   | ether1         | Internet          | DHCP / Static dari ISP      |
| Router1   | ether2         | PPPoE Server      | IP Pool 172.16.0.0/29       |
| Router2   | ether1         | PPPoE Client      | PPPoE Dial ke Router1       |
| Router2   | ether2         | LAN to PC         | 192.168.1.1/24              |
| PC        | -              | Client LAN        | IP DHCP                     |

## Konfigurasi PPPoE Server (Router1)

### Create IP Pool
di `IP > Pool > +`

<img width="1919" height="1003" alt="image" src="https://github.com/user-attachments/assets/67094768-2275-4e0c-ab47-ac315525781d" />

### Create PPP Profile
di `PPP > Profiles > +`

<img width="1919" height="1004" alt="image" src="https://github.com/user-attachments/assets/8c8b03ff-420e-46aa-919e-8bd06bc56d6b" />

**Rate limit**

<img width="1919" height="1001" alt="image" src="https://github.com/user-attachments/assets/e4fc1753-dc65-447c-b88f-8d93290b4091" />

### Tambah PPPoE Server pada interface ether2

<img width="1919" height="1004" alt="image" src="https://github.com/user-attachments/assets/a1ccb431-b923-4e6d-b363-6fd1cfb2d7db" />

### Tambah PPPoE User

<img width="1919" height="1001" alt="image" src="https://github.com/user-attachments/assets/ad5421d7-8d61-4e78-b060-2e7088a12096" />

## Konfigurasi PPPoE Client (Router2)

### Tambah PPPoE Client di ether1
di `PPP > + > PPPoE Client`

<img width="1918" height="1003" alt="image" src="https://github.com/user-attachments/assets/23e3293f-1dec-45cf-8e3c-84e1558ee709" />

**Dial Out**

<img width="1919" height="1004" alt="image" src="https://github.com/user-attachments/assets/81c5992f-d87c-47d0-b9bd-53bdbf57192d" />

## Konfigurasi LAN di Router2
**Create Ip Address**

<img width="1917" height="1004" alt="image" src="https://github.com/user-attachments/assets/12d0ace0-a1b0-4c1c-8b03-79aa665dff4c" />

**NAT**

<img width="1918" height="1005" alt="image" src="https://github.com/user-attachments/assets/3c51ce8c-babb-4e7d-b329-ae3d980898e6" />

<img width="1918" height="997" alt="image" src="https://github.com/user-attachments/assets/e5994dd8-dcb6-4ccf-bf4f-bcef201d0ff7" />

**DHCP Server**

<img width="1919" height="1000" alt="image" src="https://github.com/user-attachments/assets/f4a87020-0777-40b1-9a8d-249aa957f217" />

`**Next aja terus sampe success**`

## Tes koneksi

<img width="970" height="605" alt="image" src="https://github.com/user-attachments/assets/aa3b6f30-b992-41f8-86d0-e08cbf2b1231" />
