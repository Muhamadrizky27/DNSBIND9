# Instalasi dan konfigurasi DNS BIND9 Ubuntu server
### Kelompok A2 (DNS BIND9)
- Muhamad Rizky - 233040043
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

    zone "mabar.dev" {
    type master;
    file "/etc/bind/db.mabar.dev";


#### 2. Buat File Zona: Salin file contoh zona default ke file baru:

    sudo cp /etc/bind/db.local /etc/bind/db.mabar.dev

edit file baru tersebut

    sudo nano /etc/bind/db.mabar.dev

Ubah konten file menjadi seperti berikut:

    ;
    ; BIND data file for mabar.dev
    ;
    $TTL    604800
    @       IN      SOA     ns.mabar.dev. admin.mabar.dev. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      ns.mabar.dev.
    ns      IN      A       192.168.1.26
    @       IN      A       192.168.1.26
    www     IN      A       192.168.1.26

#### 3. Verifikasi Konfigurasi: Setelah melakukan perubahan, Anda perlu memeriksa konfigurasi BIND untuk memastikan tidak ada kesalahan:
     sudo named-checkconf

Anda juga harus memeriksa file zona:

     sudo named-checkzone mabar.dev /etc/bind/db.mabar.dev



> sudo named-checkzone mabar.dev /etc/bind/db.mabar.dev

#### 4. Restart BIND
Setelah semua konfigurasi dilakukan, restart layanan BIND untuk menerapkan perubahan:

    sudo systemctl restart bind9

#### 5. Uji Konfigurasi
1. Uji DNS dengan dig: Untuk menguji apakah DNS berfungsi dengan baik, gunakan perintah dig dari terminal:
   
       dig @192.168.1.26 mabar.dev
   
3. Uji dengan ping: Anda juga dapat menguji dengan ping:
   
        ping mabar.dev




