# Modul-Pengenalan-UML
## Apakah UML itu?
UML (User Mode Linux) adalah sebuah virtual sistem dari linux yang memungkinkan kita untuk membuat simulasi jaringan virtual yang biasa terdiri dari host, router, switch.

## Instalasi UML
### 1. Untuk Windows 
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
### 2. Untuk Linux
  - Buka terminal, ketikkan **ssh -X [nama_kelompok]@[ip_sesuai_kelas]<br>
    Contoh: ssh -X d1@10.151.36.201**
  - Kemudian masukkan password kelompok kalian<br>
    **Contoh: Password -> praktikum**
  - Pastikan allow connection dengan mengetikkan Yes
  - Login dengan **[nama_kelompok]** dan password **praktikum<br>
    Contoh: Username -> d1<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Password -> praktikum**
## Membuat Topologi Jaringan yang Akan Digunakan
![Topologi CPT](/images/006a.PNG)
1.	Setelah login, buat file script dengan ekstensi **.sh** yang akan digunakan untuk menyimpan script membuat **router, switch, dan client**. Misalkan kita buat **topologi.sh**.<br>
2.	Ketikkan **nano topologi.sh**.<br>
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
  **Keterangan:** Jangan lupa mengubah ***'ip_tuntap_tiap_kelompok'*** terlebih dahulu dan sesuai kelompok masing-masing.<br>
5.	Kemudian jalankan script tersebut dengan perintah **bash topologi.sh**.
  ![GEBANG login](/images/008.PNG)
6.	Setelah muncul seperti gambar diatas, login di masing-masing router dan client menggunakan **username = root dan password = praktikum**.
  ![GEBANG root](/images/009.PNG)
7.	Di router **GEBANG** lakukan setting sysctl dengan mengetik perintah **nano /etc/sysctl.conf**.<br>
8.	Hilangkan tanda pagar (#) pada bagian **net.ipv4.ip_forward=1**.
  ![GEBANG sysctl](/images/010a.PNG)
  Lalu ketikkan **sysctl –p** untuk mengaktifkan perubahan yang ada.<br>
9.	Setting IP di setiap router dan client dengan mengetikkan **nano /etc/network/interfaces**. Lalu seting IPnya sebagai berikut:<br>
  **Setting IP pada GEBANG (Sebagai Router)**.
  ```shell
  auto eth0
  iface eth0 inet static
  address 'ip_eth0_GEBANG_tiap_kelompok'
  netmask 255.255.255.252
  gateway 'ip_tuntap_tiap_kelompok'

  auto eth1
  iface eth1 inet static
  address 'ip_eth1_GEBANG_tiap_kelompok'
  netmask 255.255.255.248

  auto eth2
  iface eth2 inet static
  address 192.168.0.1
  netmask 255.255.255.0
  ```
  **Setting IP pada NGAGEL (Sebagai client)**.
  ```shell
  auto eth0
  iface eth0 inet static
  address 192.168.0.2
  netmask 255.255.255.0
  gateway 192.168.0.1
  ```
  **Setting IP pada NGINDEN (Sebagai client)**.
  ```shell
  auto eth0
  iface eth0 inet static
  address 192.168.0.3
  netmask 255.255.255.0
  gateway 192.168.0.1
  ```
  **Setting IP pada KLAMPIS (Sebagai DNS Server)**.
  ```shell
  auto eth0
  iface eth0 inet static
  address 'ip_KLAMPIS_tiap_kelompok'
  netmask 255.255.255.248
  gateway 'ip_eth1_GEBANG_tiap_kelompok'
  ```
  **Setting IP pada PUCANG (Sebagai Web Server)**.
  ```shell
  auto eth0
  iface eth0 inet static
  address 'ip_PUCANG_tiap_kelompok'
  netmask 255.255.255.248
  gateway 'ip_eth1_GEBANG_tiap_kelompok'
  ```
  **Keterangan**:
    - **Ip_eth0_GEBANG_tiap_kelompok** = NID_tuntap_tiap_kelompok + 2
    - **Ip_tuntap_tiap_kelompo**k = NID_tuntap_tiap_kelompok + 1
    - **Ip_eth1_GEBANG_tiap_kelompok** = NID_DMZ_tiap_kelompok + 1
    - **Ip_KLAMPIS_tiap_kelompok** = NID_DMZ_tiap_kelompok + 2
    - **Ip_PUCANG_tiap_kelompok** = NID_DMZ_tiap_kelompok + 3<br>
10.	Restart network pada setiap router dan host dengan mengetikkan **service networking restart** atau **/etc/init.d/networking restart**.<br>
11.	Coba cek IP pada setiap router dan host dengan mengetikkan **ifconfig**. Jika sudah mendapatkan IP seperti gambar dibawah, setting IP yang kalian lakukan benar.
  ![ifconfig](/images/011a.PNG)
12.	Topologi yang kalian buat sudah bisa berjalan secara lokal, tetapi kalian belum bisa mengakses jaringan keluar. Ketikkan **iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16** pada router **GEBANG**.
  ![iptables](/images/012.PNG)
13.	Coba test di semua router dan client dengan melakukan **ping its.ac.id** atau **ping 10.151.36.1** dari masing-masing host untuk mengecek apakah pengaturan anda benar atau tidak
  ![iptables](/images/013.PNG)
14.	Export proxy di uml kalian terlebih dahulu dengan syntax seperti dibawah ini:
  `export http_proxy=”http://`<span style="color:red;">[emailitsanda]`%40`mhs.if.its.ac.id`:`password`@`proxy.its.ac.id`:`8080</span>`”;`
  export https_proxy=”http://[emailitsanda]%40mhs.if.its.ac.id:password@proxy.its.ac.id:8080”;
  export ftp_proxy=”http://[emailitsanda]%40mhs.if.its.ac.id:password@proxy.its.ac.id:8080”;