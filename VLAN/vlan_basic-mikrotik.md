## Apa Itu VLAN?

**VLAN (Virtual Local Area Network)** adalah metode segmentasi jaringan secara logis pada perangkat fisik yang sama. VLAN memungkinkan pemisahan broadcast domain dan meningkatkan efisiensi serta keamanan jaringan.

## Manfaat VLAN

- Memisahkan jaringan berdasarkan fungsi/divisi
- Meningkatkan keamanan jaringan
- Mengurangi broadcast
- Memudahkan pengelolaan jaringan

## Topologi Jaringan

<img width="1919" height="917" alt="image" src="https://github.com/user-attachments/assets/eb583c8c-52f1-4aa3-946e-ea273ab9602d" />


Keterangan:
- Router1 = Gateway + Buat VLAN
- Router2 = Switch Trunking VLAN
- PC1 = VLAN 100
- PC2 = VLAN 200

## IP Addressing Plan

- **Router1 (Gateway + VLAN Creator)**:
  - ether1: DHCP (Internet)
  - ether2: trunk ke Router2
  - VLAN100: 192.168.10.1/24 (DHCP Server)
  - VLAN200: 192.168.20.1/24 (DHCP Server)

- **Router2 (Trunk Receiver + Access VLAN)**:
  - ether1: trunk (tagged VLAN100 dan VLAN200)
  - ether2: Access VLAN100
  - ether3: Access VLAN200

- **PC1**: DHCP Client
- **PC2**: DHCP Client

## Konfigurasi Step-by-Step

### Router1
**1. Create VLAN**

di `Interfaces > VLAN > +`

<img width="1919" height="1002" alt="image" src="https://github.com/user-attachments/assets/cd3f0be6-48a4-40a0-af41-3b1aa17b79ea" />

`Apply + OK`

<img width="1919" height="1003" alt="image" src="https://github.com/user-attachments/assets/532d675d-a84d-4387-a420-69cebf68b46d" />

`Apply + OK`

**2. IP Address VLAN**

di `IP > Addresses > +`

<img width="1919" height="1002" alt="image" src="https://github.com/user-attachments/assets/6a50e5d0-a5a0-45fb-953d-1b8eb62bee84" />

`Apply + OK`

<img width="1919" height="1002" alt="image" src="https://github.com/user-attachments/assets/c53c482a-957e-49f3-aae3-f5204b2a80f3" />

`Apply + OK`

**3. DHCP Server**

di `IP > DHCP Server > DHCP Setup`

<img width="1919" height="1004" alt="image" src="https://github.com/user-attachments/assets/4de90bce-39f4-4e1c-8a9c-aff4a31fc21e" />

`Next aja terus sampai success`

<img width="1919" height="1002" alt="image" src="https://github.com/user-attachments/assets/b0c5b80e-941c-4a40-8253-8dbe349ae503" />

`Next aja terus sampai success`

### Router2 - Trunk Receiver + Access VLAN
**1. Create Vlan Interface**

di `Interfaces > VLAN > +`

<img width="1919" height="998" alt="image" src="https://github.com/user-attachments/assets/6aad9c11-0ac4-4fdd-8454-27274118de2f" />

<img width="1919" height="1002" alt="image" src="https://github.com/user-attachments/assets/23444362-810c-4636-b7f9-a1188d85bb5b" />


**2. VLAN DHCP**

di `IP > DHCP Client > +`

<img width="1919" height="1001" alt="image" src="https://github.com/user-attachments/assets/43395be1-0b34-4f7b-9771-46f2d7dac270" />

`Apply + OK`

<img width="1919" height="1004" alt="image" src="https://github.com/user-attachments/assets/991b80ee-2927-4da3-af72-e3de6020f39a" />

`Apply + OK`

**3. Create Bridge**

di `Bridge > +`

<img width="1919" height="1001" alt="image" src="https://github.com/user-attachments/assets/344501fa-dba5-465c-b14c-11569cf719bb" />

<img width="1916" height="998" alt="image" src="https://github.com/user-attachments/assets/ccdae141-0d10-4b6f-baf6-0dcb7b290851" />

**4. Insert Port to Bridge**

di `Bridge > Ports > +`

<img width="1917" height="996" alt="image" src="https://github.com/user-attachments/assets/aa70c275-18e8-4467-bd35-96a7ca9683f5" />

**lakukan juga:**
- Ether2 to Bridge100
- vlan200 to Bridge200
- Ether3 to Bridge200


## Konfigurasi PC & Tes koneksi

**PC1 (VLAN100)**:

 <img width="820" height="578" alt="image" src="https://github.com/user-attachments/assets/fe34d3d8-b8c0-44d2-8ee5-b54dcf24d2f9" />

**PC2 (VLAN20)**:

 <img width="820" height="578" alt="image" src="https://github.com/user-attachments/assets/81ec1d2c-ee51-43ae-b24b-6134d7240720" />
