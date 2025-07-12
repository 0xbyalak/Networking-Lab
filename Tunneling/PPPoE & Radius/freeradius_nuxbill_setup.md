# 📡 Dokumentasi Integrasi FreeRADIUS + PHPNuxBill untuk ISP

## 📘 Tujuan

Membangun sistem AAA (Authentication, Authorization, Accounting) berbasis **FreeRADIUS** yang terintegrasi dengan sistem billing berbasis **PHPNuxBill**, untuk digunakan di jaringan **ISP kecil/menengah** seperti RT/RW Net, Hotspot, atau PPPoE MikroTik.

---

## 🧩 Komponen yang Digunakan

| Komponen         | Keterangan                         |
| ---------------- | ---------------------------------- |
| FreeRADIUS       | AAA server utama                   |
| PHPNuxBill       | GUI Billing & Manajemen User       |
| MariaDB / MySQL  | Backend database user + accounting |
| MikroTik Router  | NAS / PPPoE/Hotspot server         |
| Debian 11/Ubuntu | OS server untuk instalasi          |

---

## ⚙️ Instalasi FreeRADIUS

```bash
sudo apt update
sudo apt install freeradius freeradius-mysql -y
```

Aktifkan SQL module:

```bash
sudo ln -s /etc/freeradius/3.0/mods-available/sql /etc/freeradius/3.0/mods-enabled/
```

Edit `/etc/freeradius/3.0/mods-enabled/sql`:

- Ganti driver ke `mysql`
- Masukkan kredensial DB PHPNuxBill:

```conf
server = "localhost"
login = "radius"
password = "radiuspass"
database = "nuxbill"
```

Uji konfigurasi:

```bash
freeradius -X
```

---

## 📦 Instalasi PHPNuxBill

### 1. Install Dependency:

```bash
sudo apt install apache2 mariadb-server php php-mysql php-curl php-mbstring php-gd php-xml unzip git -y
```

### 2. Download PHPNuxBill:

```bash
cd /var/www/html
sudo git clone https://github.com/phpnuxbill/NuxBill.git nuxbill
sudo chown -R www-data:www-data nuxbill
```

### 3. Setup Database:

```sql
CREATE DATABASE nuxbill;
GRANT ALL ON nuxbill.* TO 'radius'@'localhost' IDENTIFIED BY 'radiuspass';
FLUSH PRIVILEGES;
```

Import skema:

```bash
cd /var/www/html/nuxbill
mysql -u radius -p nuxbill < database/nuxbill.sql
```

### 4. Setup Web:

- Akses via browser: `http://<IP-SERVER>/nuxbill`
- Ikuti wizard instalasi

---

## 📡 Setup MikroTik (NAS)

Tambahkan NAS di `/etc/freeradius/3.0/clients.conf`:

```conf
client mikrotik {
    ipaddr = 192.168.88.1
    secret = radius123
    shortname = rt01
    nastype = other
}
```

### MikroTik Configuration (PPPoE)

```bash
/ip radius add address=192.168.1.2 secret=radius123 service=ppp
/ppp aaa set use-radius=yes accounting=yes interim-update=30s
```

---

## 🧾 Struktur Database FreeRADIUS (yang diakses PHPNuxBill)

| Tabel        | Fungsi                   |
| ------------ | ------------------------ |
| radcheck     | Username, Password       |
| radreply     | Bandwidth, IP, Timeout   |
| radacct      | Accounting login/logout  |
| radusergroup | Group paket user         |
| nas          | NAS terdaftar (MikroTik) |

Contoh radreply:

```sql
INSERT INTO radreply (username, attribute, op, value)
VALUES ('budi', 'Mikrotik-Rate-Limit', '=', '2M/512k');
```

---

## 🎛️ Fitur Integrasi yang Aktif

- ✅ Manajemen user via GUI
- ✅ Login PPPoE / Hotspot via FreeRADIUS
- ✅ Bandwidth control via Mikrotik-Rate-Limit
- ✅ Auto Disconnect via timeout / expired paket
- ✅ Monitoring user aktif / histori pemakaian
- ✅ Cetak invoice PDF / histori pembayaran

---

## 🚨 Tips & Best Practice

| Tips                          | Penjelasan                                    |
| ----------------------------- | --------------------------------------------- |
| Backup database rutin         | Backup MySQL harian                           |
| Gunakan secret unik per NAS   | Untuk keamanan                                |
| Monitor dengan Grafana/Zabbix | Untuk pantau pengguna aktif & performa server |
| Pisahkan server GUI & RADIUS  | Untuk deployment skala ISP menengah           |

---

## ✅ Penutup

Dengan integrasi ini, kamu bisa membangun sistem ISP yang efisien dan profesional, dengan sistem login berbasis RADIUS dan billing otomatis yang terpusat.

Kalau ingin lanjut ke implementasi kuota, auto-disable, atau notifikasi Telegram, tinggal perluas modul PHPNuxBill dan tambahkan automation di server.

---

📂 Author: MentorGPT - OpenAI x ISP Stack

