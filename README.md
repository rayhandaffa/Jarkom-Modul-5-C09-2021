# Jaringan Komputer C-09 (2021)
Laporan Resmi Jaringan Komputer Modul 5 - Firewall

NRP              | Nama
-----------------|-----------
05111940000014   | Ega Prabu Pamungkas
05111940000178   | Muhammad Rizqullah Akbar
05111940000227   | Rayhan Daffa Alhafish

# Topologi 
![img](https://github.com/rayhandaffa/Jarkom-Modul-5-C09-2021/blob/main/img/topologi.jpeg) <br>

# Pembagian Subnet (CIDR)
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

# Pembagian IP 
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

## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

### Jawaban
Konfigurasi interface pada Foosha sebagai berikut.

![ifaceFoosha](/img/interfaces-foosha.jpeg)

```bash
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source "$eth0_ip"
```

Keterangan:
-t nat: Menggunakan tabel NAT karena akan mengubah alamat asal dari paket.

-A POSTROUTING: Menggunakan chain POSTROUTING karena mengubah asal paket setelah routing

-o eth0: Paket keluar dari eth0 Foosha

-j SNAT: Menggunakan target SNAT untuk mengubah source atau alamat asal dari paket

--to-source: Mendefinisikan IP source, di mana digunakan eth0 Foosha

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
-A INPUT: Menggunakan chain INPUT

-p tcp: Protokol yang digunakan, yaitu tcp

--dport 80: Port yang digunakan, yaitu 80

-j DROP: Paket di-drop

### Test

![test2.1](/img/2-1-nmap-doriki.jpeg)

![test2.2](/img/2-2-nmap-jipangu.jpeg)

## Soal 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

### Jawaban

```bash
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

Keterangan:
A INPUT: Menggunakan chain INPUT

-p icmp: Mendefinisikan protokol yang digunakan, yaitu ICMP (ping)

-m connlimit: Menggunakan rule connection limit

--connlimit-above 3: Limit yang ditangkap paket adalah di atas 3

--connlimit-mask 0 : Hanya memperbolehkan 3 koneksi setiap subnet dalam satu waktu

-j DROP: Paket di-drop

### Test

![test3](/img/3-ping4client.jpeg)