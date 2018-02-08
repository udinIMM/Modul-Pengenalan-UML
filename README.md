# Modul-Pengenalan-UML
## Apakah UML itu?
UML (User Mode Linux) adalah sebuah virtual sistem dari linux yang memungkinkan kita untuk membuat simulasi jaringan virtual yang biasa terdiri dari host, router, switch.

## Instalasi UML
### 1. Untuk Windows 
  * **Download Putty**
    Silahkan ambil di drive http://bit.ly/JARKOM2018 . Atau download dari webnya -> http://www.putty.org/
  * **Download Xming**
    Silahkan ambil di drive http://bit.ly/JARKOM2018 . Atau download dari link berikut -> https://sourceforge.net/projects/xming/
  * Jalankan Xming, kemudian jalankan putty yang sudah kalian install
    ![PuTTY](/images/001.PNG)
  * Isikan **Host Name** dengan IP sesuai pembagian masing-masing kelas :
    **Kelas A = kelompok A1 s.d. A12 (10.151.36.203) & kelompok A13 s.d. A15 (10.151.36.201)
    Kelas B = kelompok B1 s.d. B17 (10.151.36.202)
    Kelas C = kelompok C1 s.d. C18 (10.151.36.204)
    Kelas D = kelompok D1 s.d. D19 (10.151.36.201)
    Kelas E = kelompok E1 s.d. E12 (10.151.36.205) & kelompok E13 s.d. E15 (10.151.36.201)**
    ![PuTTY IP](/images/002.PNG)
  * Kemudian pilih tab **SSH** dibagian **Category** dan pilih **X11**, lalu centang **Enable X11 forwarding**
    ![PuTTY X11](/images/003.PNG)
  * Kemudian pilih Open dan akan muncul tampilan seperti ini
    ![PuTTY login](/images/004.PNG)
  * Login dengan username **[nama kelompok]** dan password **praktikum**.
    Contoh: **Username -> d1.
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Password -> praktikum**
    Jika berhasil akan muncul seperti gambar dibawah ini
    ![PuTTY d1](/images/005.PNG)
### 2. Untuk Linux
  * Buka terminal, ketikkan **ssh -X [nama_kelompok]@[ip_sesuai_kelas]
    Contoh: ssh -X d1@10.151.36.201**
  * Kemudian masukkan password kelompok kalian
    **Contoh: Password -> praktikum**
  * Pastikan allow connection dengan mengetikkan Yes
  * Login dengan **[nama_kelompok]** dan password **praktikum
    Contoh: Username -> d1
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Password -> praktikum**
## Membuat Topologi Jaringan yang Akan Digunakan