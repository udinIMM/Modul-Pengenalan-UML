## Modul-Pengenalan-UML
### Apakah UML itu?
UML (User Mode Linux) adalah sebuah virtual sistem dari linux yang memungkinkan kita untuk membuat simulasi jaringan virtual yang biasa terdiri dari host, router, switch.

### Instalasi UML
#### 1. Untuk Windows 
  - **Download Putty**<br>
    Silahkan ambil di drive http://bit.ly/JARKOM2018 . Atau download dari webnya -> http://www.putty.org/
  - **Download Xming**<br>
    Silahkan ambil di drive http://bit.ly/JARKOM2018 . Atau download dari link berikut -> https://sourceforge.net/projects/xming/
  - Jalankan Xming, kemudian jalankan putty yang sudah kalian install
    ![PuTTY](/images/001.PNG)
  - Isikan **Host Name** dengan IP sesuai pembagian masing-masing kelas :<br>
    **Kelas A = kelompok A1 s.d. A12 (10.151.36.203) & kelompok A13 s.d. A15 (10.151.36.201)<br>
    Kelas B = kelompok B1 s.d. B17 (10.151.36.202)<br>
    Kelas C = kelompok C1 s.d. C17 (10.151.36.204)<br>
    Kelas D = kelompok D1 s.d. D19 (10.151.36.201)<br>
    Kelas E = kelompok E1 s.d. E12 (10.151.36.205) & kelompok E13 s.d. E17 (10.151.36.201)**
    ![PuTTY IP](/images/002.PNG)
  - Kemudian pilih tab **SSH** dibagian **Category** dan pilih **X11**, lalu centang **Enable X11 forwarding**
    ![PuTTY X11](/images/003.PNG)
  - Kemudian pilih Open dan akan muncul tampilan seperti ini
    ![PuTTY login](/images/004.PNG)
  - Login dengan username **[nama kelompok]** dan password **praktikum**.<br>
    Contoh: **Username -> d1.<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Password -> praktikum**<br>
    Jika berhasil akan muncul seperti gambar dibawah ini
    ![PuTTY d1](/images/005.PNG)
#### 2. Untuk Linux
  - Buka terminal, ketikkan **ssh -X [nama_kelompok]@[ip_sesuai_kelas]<br>
    Contoh: ssh -X d1@10.151.36.201**
  - Kemudian masukkan password kelompok kalian<br>
    **Contoh: Password -> praktikum**
  - Pastikan allow connection dengan mengetikkan Yes
  - Login dengan **[nama_kelompok]** dan password **praktikum<br>
    Contoh: Username -> d1<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Password -> praktikum**
### Membuat Topologi Jaringan yang Akan Digunakan
![Topologi CPT](/images/006a.PNG)
1.	Setelah login, buat file script dengan ekstensi **.sh** yang akan digunakan untuk menyimpan script membuat **router, switch, dan client**. Misalkan kita buat **topologi.sh**<br>
2.	Ketikkan **nano topologi.sh**<br>
3.	Sintaks yang digunakan adalah sebagai berikut:<br>
  **a.	Membuat switch:**<br>
    `uml_switch –unix `**namaswitch**` > /dev/null < /dev/null &`<br>
  **b.	Membuat router dan client:**<br>
    `xterm –T `**namadevice**` –e linux ubd0=`**namadevice**`,jarkom umid=`**namadevice**` eth0=daemon,,,`**namaswitch**` mem=96M &`<br>
    <br>
    Keterangan:
      -	Sintaks untuk membuat router dan klien hampir sama, yang membedakan adalah jumlah eth nya, eth pada router biasanya lebih dari 1.
      - **Jarkom** adalah iso UML yang digunakan.
      - Pembuatan jumlah router, switch, client dan banyaknya eth disesuaikan dengan topologi yang diminta.<br>
4.	Untuk topologi sesuai gambar, maka sintaks untuk file **topologi.sh** adalah
  ![Topologi.sh](/images/007.PNG)
  ```shell
  #switch
  uml_switch -unix switch1 > /dev/null < /dev/null &
  uml_switch -unix switch2 > /dev/null < /dev/null &

  #router
  xterm -T GEBANG -e linux ubd0=GEBANG,jarkom umid=GEBANG eth0=tuntap,,,'ip_tuntap_tiap_kelompok' eth1=daemon,,,switch1 eth2=daemon,,,switch2 mem=96M &
  #dns + web server
  xterm -T KLAMPIS -e linux ubd0=KLAMPIS,jarkom umid=KLAMPIS eth0=daemon,,,switch1 mem=96M &
  xterm -T PUCANG -e linux ubd0=PUCANG,jarkom umid=PUCANG eth0=daemon,,,switch1 mem=96M &
  #client
  xterm -T NGAGEL -e linux ubd0=NGAGEL,jarkom umid=NGAGEL eth0=daemon,,,switch2 mem=96M &
  xterm -T NGINDEN -e linux ubd0=NGINDEN,jarkom umid=NGINDEN eth0=daemon,,,switch2 mem=96M &
  ```
5.	Kemudian jalankan script tersebut dengan perintah **bash topologi.sh**
  ![GEBANG login](/images/008.PNG)