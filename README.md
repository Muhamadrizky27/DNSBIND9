# Instalasi dan konfigurasi DNS BIND9 Ubuntu server
### Kelompok A2 (DNS BIND9)
- Muhamad Rizky - 233040042
- Aditya Eka Heriyawan - 233040068
- Rafi Asmaul Rizal - 233040073
  
## 1. Persiapan

Persiapan
Pastikan sistem Anda sudah diperbarui:
  
    sudo apt update && sudo apt upgrade -y


## 2. Install BIND9
Instal paket BIND9:

    sudo apt install bind9 bind9utils bind9-doc -y


## 3. Konfigurasi DNS Server
### 1. Edit File Konfigurasi: Buka file konfigurasi utama BIND:

    sudo nano /etc/bind/named.conf.local

Tambahkan konfigurasi berikut di bagian akhir file untuk mendefinisikan zona mabar.dev:

    zone "mabar.local" {
    type master;
    file "/etc/bind/db.mabar.local";


#### 2. Buat File Zona: Salin file contoh zona default ke file baru:

    sudo cp /etc/bind/db.local /etc/bind/db.mabar.local

edit file baru tersebut

    sudo nano /etc/bind/db.mabar.local

Ubah konten file menjadi seperti berikut:

    $TTL    604800
    @       IN      SOA     ns1.mabar.local. admin.mabar.local. (
                     2024121601 ; Serial (diubah setiap kali file ini diperbarui)
                     604800     ; Refresh
                     86400      ; Retry
                     2419200    ; Expire
                     604800 )   ; Negative Cache TTL

    ; Nameserver
    @       IN      NS      ns1.mabar.local.
    
    ; A Records
    ns1     IN      A       192.168.10.176
    www     IN      A       192.168.10.176
    
    ; MX Records
    @       IN      MX      10 mail.mabar.local.
    mail    IN      A       192.168.10.176

#### 3. Verifikasi Konfigurasi: Setelah melakukan perubahan, Anda perlu memeriksa konfigurasi BIND untuk memastikan tidak ada kesalahan:
     sudo named-checkconf

Anda juga harus memeriksa file zona:

     sudo named-checkzone mabar.local /etc/bind/db.mabar.local


#### 4. Restart BIND
Setelah semua konfigurasi dilakukan, restart layanan BIND untuk menerapkan perubahan:

    sudo systemctl restart bind9

#### 5. Uji Konfigurasi

Dengan ping

    ping mabar.local

  Dengan nslookup:

    nslookup mabar.local 

## Dokumentasi

![gambar](mabar.local.rev2)


