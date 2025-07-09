# Apa itu IPSec?
IPSec (Internet Protocol Security) adalah suite protokol untuk mengamankan komunikasi IP dengan:
- Authentication (Memastikan identitas pengirim)
- Integrity - (Data tidak berubah selama transit)
- Confidentiality - (Data terenkripsi)
- Anti-replay - (Mencegah serangan replay)

## IPSec Components:
- AH (Authentication Header) - Authentikasi tanpa enkripsi
- ESP (Encapsulating Security Payload) - Enkripsi + authentikasi
- IKE (Internet Key Exchange) - Negosiasi security parameters


## IPSec ARCHITECTURE
IPSec Modes:
- Transport Mode (Hanya payload yang dienkripsi)
- Tunnel Mode (Seluruh IP packet dienkripsi `untuk site-to-site`)

## IPSec Phases:

### Phase 1 (IKE):

- Establish secure channel untuk negosiasi
- Mutual authentication
- Create ISAKMP SA (Security Association)

### Phase 2 (IPSec):

- Negosiasi IPSec parameters
- Create IPSec SA untuk data transfer
- Exchange traffic

## Topology

 ![Screenshot 2025-07-05 004829](https://github.com/user-attachments/assets/e401625b-f3ef-4e71-a360-141d2fa5dbe7)

## Konfigurasi
1. Profile configuration
   
   ![Screenshot 2025-07-04 220833](https://github.com/user-attachments/assets/5624106c-e615-457a-b5ed-0e0af995ee8e)

2. Create proposal

   ![Screenshot 2025-07-04 220925](https://github.com/user-attachments/assets/021b1e73-4152-4c28-86db-b3d32aeb5b92)

3. Create peers

   ![Screenshot 2025-07-04 221130](https://github.com/user-attachments/assets/fbfa6ea9-5c83-4063-b356-6d0dd32a3dd9)

4. Create identity

   ![Screenshot 2025-07-04 221354](https://github.com/user-attachments/assets/c9a45197-db11-45ed-8467-f2695d4e430d)

5. Create policies

   ![Screenshot 2025-07-04 221605](https://github.com/user-attachments/assets/53d2dd44-50d1-4b3c-acc8-e771873647bd)
   ![Screenshot 2025-07-04 221617](https://github.com/user-attachments/assets/1b166d59-0073-4eae-af3e-410d8a1c4a7a)

`NOTE: Lakukan hal sama pada router target`

# Apa Itu L2TP over IPSec?
L2TP (Layer 2 Tunneling Protocol) adalah protokol tunneling yang bekerja di layer 2 dan biasa digunakan untuk membentuk koneksi VPN.
Karena L2TP tidak menyediakan enkripsi, maka dikombinasikan dengan IPSec, membentuk:

- L2TP over IPSec = Tunneling + Enkripsi
  
  Cocok untuk VPN Client seperti Windows, Android, iOS.

### Topology
  
![Screenshot 2025-07-04 215932](https://github.com/user-attachments/assets/1a867ebf-e6f2-44da-990e-e1772c0e9d35)

## Konfigurasi
### L2TP Server

![Screenshot 2025-07-04 231139](https://github.com/user-attachments/assets/efb94800-79d5-4c81-a7de-0031e69438bd)

### Create PPP Secret

![Screenshot 2025-07-04 231322](https://github.com/user-attachments/assets/44988074-a35d-4a1d-9e04-f85ce7b3d47f)

### Routing

![Screenshot 2025-07-04 232827](https://github.com/user-attachments/assets/fbea6062-e6ca-464f-b3fb-340b37058dab)

### L2TP Client

![Screenshot 2025-07-04 231406](https://github.com/user-attachments/assets/e1c1b315-febb-453c-bf13-2731d80c449f)

![Screenshot 2025-07-04 232844](https://github.com/user-attachments/assets/03d5a55e-1b39-4c19-9ae3-b506dae3029b)

### Routing

![Screenshot 2025-07-04 232803](https://github.com/user-attachments/assets/31ae4b30-5869-4dd6-bc18-d8f03f4d1d63)

