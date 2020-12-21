# cobacoba

# Jarkom_Modul5_Lapres_T06
<b> Laporan Resmi Praktikum Jaringan Komputer </b> <br>
Modul 5 - FIREWALL <br>
Kelompok T06
1. Hana Ghaliyah Azhar  (05311840000032)
2. Azmi                 (05311840000047)

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## SOAL
<b>(A)</b> Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Bibah seperti dibawah ini : <br>
![Topologi Modul 5](https://user-images.githubusercontent.com/61286109/102757895-8d583d80-43a4-11eb-971a-50f803ca820a.PNG) <br>
Keterangan : 
- <b>SURABAYA</b> diberikan <b>IP TUNTAP</b>
- <b>MALANG</b> merupakan <b>DNS Server</b> diberikan <b>IP DMZ</b>
- <b>MOJOKERTO</b> merupakan <b>DHCP Server</b> diberikan <b>IP DMZ</b>
- <b>MADIUN</b> dan <b>PROBOLINGGO</b> merupakan <b>WEB Server</b>
- <b>Setiap Server</b> diberikan memory sebesar <b>128M</b>
- <b>Client dan Router</b> diberikan memori sebesar <b>96M</b>
- Jumlah host pada subnet <b>SIDOARJO 200 Host</b>
- Jumlah host pada subnet <b>GRESIK 210 Host</b> 
<br>
<b>(B)</b> karena kalian telah mempelajari Subnetting dan Routing, Bibah meminta kalian untuk membuat topologi tersebut menggunakan teknik <b>CIDR</b> atau <b>VLSM</b>. Setelah melakukan subnetting, <b>(C)</b> kalian juga diharuskan melakukan routing agar setiap perangkat pada jaringan tersebut dapat terhubung. 
<br>
<br>
<b>(D)</b> Tugas berikutnya adalah memberikan ip pada subnet <b>SIDOARJO</b> dan <b>GRESIK</b> secara dinamis menggunakan bantuan DHCP SERVER (Selain subnet tersebut menggunakan ip static). Kemudian kalian mengingat bahwa kalian harus setting DHCP RELAY pada router yang menghubungkannya, seperti yang kalian telah pelajari di masa lalu. 
<br>
<br>
<b>(1)</b> Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi <b>SURABAYA</b> menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE. 
<br>
<br>
<b>(2)</b> Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada <b>SURABAYA</b> demi menjaga keamanan. <br> <br>
<b>(3)</b> Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan <b>iptables pada masing masing server</b>, selebihnya akan di DROP. 
<br> <br>
kemudian kalian diminta untuk membatasi akses ke <b>MALANG</b> yang berasal dari SUBNET SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut: <br>
<b>(4)</b> Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat. <br>
<b>(5)</b> Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya. <br>
Selain itu paket akan di REJECT. 
<br> <br>
Karena kita memiliki 2 buah <b>WEB Server</b>, <b>(6)</b> Bibah ingin <b>SURABAYA</b> disetting sehingga setiap request dari client yang mengakses <b>DNS Server</b> akan didistribusikan <b>secara bergantian</b> pada <b>PROBOLINGGO</b> port 80 dan <b>MADIUN</b> port 80. 
<br> <br>
<b>(7)</b> Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop. <br>
Bibah berterima kasih kepada kalian karena telah mau membantunya. Bibah juga mengingatkan agar semua aturan iptables harus disimpan pada sistem atau paling tidak kalian menyediakan script sebagai backup.

## PENYELESAIAN <br>
### Menggunakan Metode VLSM
#### Subnetting (Pembagian IP) <br>
- Menentukan jumlah subnet yang ada pada topologi. <br>
![Subnet Topologi Modul 5](https://user-images.githubusercontent.com/61286109/102758704-b927f300-43a5-11eb-9548-0c1305a6ba52.png) <br>
- Menentukan jumlah alamat IP yang dibutuhkan oleh tiap subnet dan lakukan labelling netmask berdasarkan jumlah IP yang dibutuhkan. <br>
![Labelling Netmask](https://user-images.githubusercontent.com/61286109/102759054-2d629680-43a6-11eb-9880-0a931f9a8d5f.PNG) <br>
Berdasarkan total IP dan netmask yang dibutuhkan, maka kita dapat menggunakan netmask /23 untuk memberikan pengalamatan IP pada subnet.
- Hitung pembagian IP dan Lakukan subnetting dengan menggunakan pohon tersebut untuk pembagian IP sesuai dengan kebutuhan masing-masing subnet yang ada. <br>
