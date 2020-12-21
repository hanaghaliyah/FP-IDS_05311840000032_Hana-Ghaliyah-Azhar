# cobacoba

# Jarkom_Modul5_Lapres_T06
<b> Laporan Resmi Praktikum Jaringan Komputer </b> <br>
Modul 5 - FIREWALL <br>
Kelompok T06
1. Hana Ghaliyah Azhar  (05311840000032)
2. Azmi                 (05311840000047)

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## SOAL
(A) Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Bibah seperti dibawah ini : <br>
![Topologi Modul 5](https://user-images.githubusercontent.com/61286109/102757895-8d583d80-43a4-11eb-971a-50f803ca820a.PNG) <br>
Keterangan : 
- SURABAYA diberikan IP TUNTAP
- MALANG merupakan DNS Server diberikan IP DMZ
- MOJOKERTO merupakan DHCP Server diberikan IP DMZ
- MADIUN dan PROBOLINGGO merupakan WEB Server
- Setiap Server diberikan memory sebesar 128M
- Client dan Router diberikan memori sebesar 96M
- Jumlah host pada subnet SIDOARJO 200 Host
- Jumlah host pada subnet GRESIK 210 Host
(B) karena kalian telah mempelajari Subnetting dan Routing, Bibah meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM. Setelah melakukan subnetting, (C) kalian juga diharuskan melakukan routing agar setiap perangkat pada jaringan tersebut dapat terhubung. <br>
(D) Tugas berikutnya adalah memberikan ip pada subnet SIDOARJO dan GRESIK secara dinamis menggunakan bantuan DHCP SERVER (Selain subnet tersebut menggunakan ip static). Kemudian kalian mengingat bahwa kalian harus setting DHCP RELAY pada router yang menghubungkannya, seperti yang kalian telah pelajari di masa lalu. <br>
(1) Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE. <br>
(2) Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada SURABAYA demi menjaga keamanan. <br>
(3) Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server, selebihnya akan di DROP. <br>
kemudian kalian diminta untuk membatasi akses ke MALANG yang berasal dari SUBNET SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut: <br>
- (4) Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat.
- (5) Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya.
Selain itu paket akan di REJECT. <br>
Karena kita memiliki 2 buah WEB Server, (6) Bibah ingin SURABAYA disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80. <br>
(7) Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop. <br>
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
