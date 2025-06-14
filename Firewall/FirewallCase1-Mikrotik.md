# Study case 1

## **Topologi sederhana**

![Screenshot 2025-06-15 011447](https://github.com/user-attachments/assets/b69e8f04-17d2-4f25-a396-64b32ffe9922)

## **Goal**

-  NAT diaktifkan agar client bisa akses internet
-  Hanya `192.168.10.2` yang boleh akses router
-  `Komputer lainnya` diblok aksesnya ke router
-  `192.168.10.3` tidak boleh akses `github.com`

## Konfigurasi
### NAT

![Screenshot 2025-06-15 012547](https://github.com/user-attachments/assets/713797b2-2b3c-441e-b43c-6bbaddd512ab)

**Tab General**
- Chain = `srcnat` → rule ini bekerja saat trafik keluar dari jaringan (ke internet)
- Out. Interface: wlan1 → rule NAT ini akan aktif jika keluar melalui wlan1 (yang terhubung ke internet)

**Tab Action**
- Action: masquerade → ini adalah jenis NAT yang otomatis menyamarkan IP lokal menjadi IP dari interface keluar (wlan1)

### Filter rule
> Accept few, Drop any
>
> Drop github.com from `192.168.10.3`

**Ijinkan `192.168.10.2` akses router**

![Screenshot 2025-06-15 013618](https://github.com/user-attachments/assets/66a35fd1-b5a6-4bef-b398-8fb862695a95)

**Blokir semua yang mau akses router**

![Screenshot 2025-06-15 013959](https://github.com/user-attachments/assets/84ae6261-e51a-4e59-b8ef-5112b04cd67a)

**Blokir koneksi ke `github.com` dari 192.168.10.3**

![Screenshot 2025-06-15 014931](https://github.com/user-attachments/assets/1fad2394-3cb0-4090-bbd4-4af9c4441ac3)
> Cek IP github.com

![Screenshot 2025-06-15 015121](https://github.com/user-attachments/assets/f065166f-4cc7-41b0-9604-39516a7c891f)
> Dst. address IP github.com

**Matikan login dengan Mac Address**

![Screenshot 2025-06-15 020830](https://github.com/user-attachments/assets/f094a56a-ca34-4fdb-83d4-c92a6d90176b)
> Masuk ke menu `Tool` pilih `Mac server`
> 
> Disable `Mac telnet server` & `Mac Winbox server`
