# Jaringan Komputer C-09 (2021)
Laporan Resmi Jaringan Komputer Modul 5 - Firewall

NRP              | Nama
-----------------|-----------
05111940000014   | Ega Prabu Pamungkas
05111940000178   | Muhammad Rizqullah Akbar
05111940000227   | Rayhan Daffa Alhafish

## Topologi 
![img](https://github.com/rayhandaffa/Jarkom-Modul-5-C09-2021/blob/main/img/topologi-bernomer.jpeg) <br>

## Pembagian Subnet (CIDR)
Subnetting menggunakan metode CIDR dengan jumlah subnet 8 buah. Hasil pembagian subnet sebagai berikut : <br> 
| Subnet | Jumlah IP | Netmask |	
|:------:|:---------:|:-------:|	
|   A1   |    3   |   /29   |	
|   A2   |    101    |   /25   |	
|   A3   |     701     |   /22   |	
|   A4   |      2  |   /30   |	
|   A5   |     2    |   /30   |	
|   A6   |    301   |   /23   |	
|   A7   |     201     |   /24   |	
|   A8   |    3    |   /29  |	
|  Total   |    1314    |   /21  | <br>

Berdasarkan tabel di atas didaptkan netmask /21 sebagai netmask terbesar yang akan digunakan untuk pembagian IP.

## Pembagian IP 
Berikut adalah pembagian IP yang diilustrasikan dengan menggunakan tree. Pada tree berikut dapat diketahui netrork ID yang dapat digunakan pada masing-masing subnet. <br> 
![img](https://github.com/rayhandaffa/Jarkom-Modul-5-C09-2021/blob/main/img/tree_ip.jpeg) <br> 

Setelah mendapatkan tree pada masing-masing subnet, selanjutnya berikut adalah tabel pembagian tabel IP untuk subnet A1 hingga A8 : <br> 
|  A1 | Network ID        | 192.188.7.128    |	
|:---:|-------------------|------------------|	
|     | Netmask           | 255.255.255.248  |	
|     | Broadcast Address | 192.188.7.135    |	
|  A2 | Network ID        | 192.188.7.0      |	
|     | Netmask           | 255.255.255.128  |	
|     | Broadcast Address | 192.188.7.127       |	
|  A3 | Network ID        | 192.188.0.0        |	
|     | Netmask           | 255.255.252.0    |	
|     | Broadcast Address | 192.188.3.255     |	
|  A4 | Network ID        | 192.188.7.144       |	
|     | Netmask           | 255.255.252.0    |	
|     | Broadcast Address | 192.188.7.147      |	
|  A5 | Network ID        | 192.188.7.148        |	
|     | Netmask           | 255.255.255.252  |	
|     | Broadcast Address | 192.188.7.151       |	
|  A6 | Network ID        | 192.188.4.0        |	
|     | Netmask           | 255.255.254.0    |	
|     | Broadcast Address | 192.188.5.255     |	
|  A7 | Network ID        | 192.188.6.0        |	
|     | Netmask           | 255.255.255.0    |	
|     | Broadcast Address | 192.188.6.255      |	
|  A8 | Network ID        | 192.188.7.136       |	
|     | Netmask           | 255.255.255.248  |	
|     | Broadcast Address | 192.188.7.143      |

## DHCP Server dan DHCP Relay
Hal yang pertama dilakukan adalah meng-assign IP address ke masing-masing node pada topologi selain Client. Setelah itu lakukan routing dari foosha ke seluruh subnet pada topologi. Lalu, untuk DHCP relay, arahkan semua request ke IP Jipangu sebagai DHCP Server pada topologi. Selanjutnya, lakukanlah pengaturan IP Client pada node Jipangu. <br> 

```subnet 192.188.7.128 netmask 255.255.255.248 {
}

subnet 192.188.7.0 netmask 255.255.255.128 {
        range 192.188.7.2 192.188.7.126;
        option routers 192.188.7.1;
        option broadcast-address 192.188.7.128;
        option domain-name-servers 192.188.7.130;
        default-lease-time 360;
        max-lease-time 7200;
}

subnet 192.188.0.0 netmask 255.255.252.0 {
        range 192.188.0.2 192.188.3.254;
        option routers 192.188.0.1;
        option broadcast-address 192.188.3.255;
        option domain-name-servers 192.188.7.130;
        default-lease-time 360;
        max-lease-time 7200;
}

subnet 192.188.4.0 netmask 255.255.254.0 {
        range 192.188.4.2 192.188.5.254;
        option routers 192.188.4.1;
        option broadcast-address 192.188.5.255;
        option domain-name-servers 192.188.7.130;
        default-lease-time 360;
        max-lease-time 7200;
}

subnet 192.188.6.0 netmask 255.255.255.0 {
        range 192.188.6.2 192.188.6.254;
        option routers 192.188.6.1;
        option broadcast-address 192.188.6.255;
        option domain-name-servers 192.188.7.130;
        default-lease-time 360;
        max-lease-time 7200;
}
```
Jangan lupa untuk menghidupkan DNS Server dan arahkan ke IP Doriki agar dapat tersambung pada internet.

## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

### Jawaban
Konfigurasi interface pada Foosha sebagai berikut.

![ifaceFoosha](/img/interfaces-foosha.jpeg)

```bash
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source "$eth0_ip"
```

Keterangan:
- -t nat: Menggunakan tabel NAT karena akan mengubah alamat asal dari paket.

- -A POSTROUTING: Menggunakan chain POSTROUTING karena mengubah asal paket setelah routing

- -o eth0: Paket keluar dari eth0 Foosha

- -j SNAT: Menggunakan target SNAT untuk mengubah source atau alamat asal dari paket

- --to-source: Mendefinisikan IP source, di mana digunakan eth0 Foosha

### Test 3

![test1](/img/1-pingkeluar-cipher.jpeg)

## Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

### Jawaban
Konfigurasi `iptables` pada Jipangu dan Doriki.

```bash
iptables -A INPUT ! -s 192.188.0.0/21 -p tcp --dport 80 -j DROP
```

Keterangan:
- -A INPUT: Menggunakan chain INPUT

- -p tcp: Protokol yang digunakan, yaitu tcp

- --dport 80: Port yang digunakan, yaitu 80

- -j DROP: Paket di-drop

### Test

![test2.1](/img/2-1-nmap-doriki.jpeg)

![test2.2](/img/2-2-nmap-jipangu.jpeg)

## Soal 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

### Jawaban

```bash
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

Keterangan: <br>
- -A INPUT: Menggunakan chain INPUT

- -p icmp: Mendefinisikan protokol yang digunakan, yaitu ICMP (ping)

- -m connlimit: Menggunakan rule connection limit

- --connlimit-above 3: Limit yang ditangkap paket adalah di atas 3

- --connlimit-mask 0 : Hanya memperbolehkan 3 koneksi setiap subnet dalam satu waktu

- -j DROP: Paket di-drop

### Test

![test3](/img/3-ping4client.jpeg)

## Soal 4
Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis. <br> 

### Jawaban 
```iptables -A INPUT -s 192.188.7.0/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.188.0.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.188.7.0/25 -j REJECT
iptables -A INPUT -s 192.188.0.0/22 -j REJECT
```
Barisan kode di atas digunakan untuk memberi akses ke Doriki, namun hanya untuk jam 07:00 sampai 15:00 pada hari Senin sampai Kamis, selain dari waktu itu akan di reject. 192.188.7.0/25 disini merupakan NID dari subnet Blueno dan 192.188.4.0/22 merupakan NID dari subnet Cipher.

### Test 
Untuk dapat mengetesnya dengan menggunakan `date -s '2021-10-23 21:00'. Memakai syntax seperti itu dikarenakan agar sesuai dan akses tidak di batasi. Selanjutnya dapat meng-ping ke google dari salah satu klien, disini kami menggunakan CIPHER. <br> 

![img](https://github.com/rayhandaffa/Jarkom-Modul-5-C09-2021/blob/main/img/4-pingdaricipher.jpeg)

## Soal 5 
Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya. Selain itu di reject.

### Jawaban 
Nomor 5 ini tidak beda jauh dengan nomor 4, tambahkan barisan kode berikut ini : <br> 
```iptables -A INPUT -s 192.188.4.0/23 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 192.188.6.0/24 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 192.188.4.0/23 -j REJECT
iptables -A INPUT -s 192.188.6.0/24 -j REJECT
```
Sama dengan nomor 4, barisan kode di atas menggambarkan dan memberi akses dari jam 00:00 sampai jam 06:59. 192.188.4.0/23 disini merupakan NID dari subnet Elena dan 10.19.1.0/24 merupakan NID dari subnet Fukurou. 

## Testing 
Untuk dapat mengetesnya dengan menggunakan `date -s '2021-10-23 21:00'. Memakai syntax seperti itu dikarenakan agar sesuai dan akses tidak di batasi. Selanjutnya dapat meng-ping ke google dari salah satu klien, disini kami menggunakan FUKUROU. <br> 

![img](https://github.com/rayhandaffa/Jarkom-Modul-5-C09-2021/blob/main/img/5-pingdarifukurou.jpeg) 

## Soal 6 
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate

### Jawaban 
```iptables -t nat -A PREROUTING -d 192.188.7.130 -p tcp -m statistic --mode random --probability 0.5 -j DNAT --to-destination 192.188.7.138
iptables -t nat -A PREROUTING -d 192.188.7.130 -p tcp -j DNAT --to-destination 192.188.7.139
iptables -t nat -A POSTROUTING -s 192.188.7.138 -p tcp -j SNAT --to-source 192.188.7.130
ptables -t nat -A POSTROUTING -s 192.188.7.139 -p tcp -j SNAT --to-source 192.188.7.130
```
### Testing 
1. Pada Guanhao, Jorge, Maingate dan Elena install: `apt-get install netcat -y`
2. Pada Jorge ketikkan perintah: `nc -l -p 80`
3. Pada Maingate ketikkan perintah: `nc -l -p 80`
4. Pada client Elena ketikkan perintah: `nc 192.188.7.130 80`
5. Ketikkan sembarang pada client Elena, jika sudah hasilnya akan muncul disalah satu webserver
6. CTRL+Z pada client Elena. kemudian ketikkan perintah: `nc 192.188.7.130 80` lagi pada Elena.
7. Ketikkan sembarang lagi, hasilnya akan keluar di server kedua
