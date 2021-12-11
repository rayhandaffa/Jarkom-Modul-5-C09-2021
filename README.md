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
