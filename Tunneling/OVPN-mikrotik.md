# Apa itu OVPN?
OpenVPN adalah sebuah protokol dan software open-source yang digunakan untuk membuat koneksi jaringan pribadi virtual (VPN) secara aman melalui internet. Dengan menggunakan enkripsi TLS/SSL, OpenVPN memastikan bahwa data yang dikirim antara client dan server tidak bisa diintip atau dimodifikasi oleh pihak ketiga. OpenVPN mendukung berbagai metode autentikasi seperti username-password dan sertifikat digital, serta bisa dikonfigurasi untuk menggunakan port TCP atau UDP, menjadikannya fleksibel untuk berbagai kebutuhan, baik remote access VPN maupun site-to-site VPN. Karena bersifat open-source dan sangat aman, OpenVPN banyak digunakan di lingkungan perusahaan maupun pribadi.

## Topologi

![Screenshot 2025-07-04 215932](https://github.com/user-attachments/assets/60592bf6-cbe4-4513-a5e4-46198a7dcb15)

## Konfigurasi
### Buat certificate
`Root CA`
![image](https://github.com/user-attachments/assets/f00231c6-e02c-4565-b467-a489f1696e7a)

`Server certificate`
![image](https://github.com/user-attachments/assets/e0d31d56-9f23-4a3e-832c-c0c310dcae88)

`Client certificate`
![image](https://github.com/user-attachments/assets/7c9f75c3-05f5-4df0-9761-536a8eb61972)

`Digital Signature`

diterminal
```yaml
[admin@R1] > certificate sign CA-Tamplate ca-crl-host=192.168.137.129 name=myCA
  progress: done
[admin@R1] > certificate sign Server-Tamplate ca=myCA name=server
  progress: done
[admin@R1] > certificate sign Client-Tamplate ca=myCA name=client
  progress: done
[admin@R1] > certificate set server trusted=yes
[admin@R1] > certificate set client trusted=yes
[admin@R1] > 
```

`Import certificate client`
```yaml
[admin@R1] > certificate export-certificate myCA export-passphrase=kunciajaib
[admin@R1] > certificate export-certificate client export-passphrase=kunciajaib
```

`Download certificate client`
![image](https://github.com/user-attachments/assets/f7b1fb17-9764-46b2-b73e-8de1952e43cd)

### Setup OVPN Server
![image](https://github.com/user-attachments/assets/965c3df6-b130-4d54-ad1d-d4c887fb36eb)

`Secrets`

![image](https://github.com/user-attachments/assets/64c33f1e-0b6b-4f6b-b64d-9d4b82db0c0f)

### Dari sisi Client
`Uploade file certificate`

![image](https://github.com/user-attachments/assets/8e1c0a55-b520-40be-b8c8-d123e4f306d3)

`Import CA`

![image](https://github.com/user-attachments/assets/41ca5d25-c7dd-4bb2-90f0-07d03d3003d0)
![image](https://github.com/user-attachments/assets/a3008f25-c76a-4689-a6f0-e3a1005f693e)

`Import certificate client `

![image](https://github.com/user-attachments/assets/a0d85eb0-5b90-4215-9cd2-7256b7279b16)
![image](https://github.com/user-attachments/assets/889c086c-9724-427e-ab39-09717402a0cc)

### Setup OVPN Client
![image](https://github.com/user-attachments/assets/4de3a33f-0e71-49dd-b715-0e1164f5fea0)

### Routing
`Router1`

![image](https://github.com/user-attachments/assets/b46c737c-5a1a-4d7d-9db3-818ad9bde1fa)

`Router2`

![image](https://github.com/user-attachments/assets/2e753b98-6c95-43f8-af3a-006e63a2eb1c)



















