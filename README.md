![Screenshot (292)](https://github.com/user-attachments/assets/24694eb8-ae9e-481a-bdf9-a12b62901026)# Jarkom-2-IT06-2024
Laporan Resmi Jarkom Modul 2
Buat Praktikan Jarkom IT06 yang masih pemula

| Nama | NRP |
| ---- | ---- |
| Callista Meyra Azizah | 5027231060 |
| Renjamin Khawarizmi Habibi | 5027231078 |

IP Prefix untuk IT06

| IT06 | 192.236 |
|----|----|

# TOPOLOGI 
![Screenshot (289)](https://github.com/user-attachments/assets/9df6bf71-7e65-40dc-aa4e-315c62431453)


# CONFIG NODE
- Nusantara
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
    address 192.236.1.1
    netmask 255.255.255.0

auto eth2
iface eth2 inet static
    address 192.236.2.1
    netmask 255.255.255.0

auto eth3
iface eth3 inet static
    address 192.236.3.1
    netmask 255.255.255.0

auto eth4
iface eth4 inet static
    address 192.236.4.1
    netmask 255.255.255.0
```

- Solok
```
auto eth0
iface eth0 inet static
    address 192.236.3.2
    netmask 255.255.255.0
    gateway 192.236.3.1
```

- Sanjaya
```
auto eth0
iface eth0 inet static
   address 192.236.1.2
   netmask 255.255.255.0
   gateway 192.236.1.1
```

- Sriwijaya
```
auto eth0
iface eth0 inet static
    address 192.236.1.3
    netmask 255.255.255.0
    gateway 192.236.1.1
```

- Tanjungkulai
```
auto eth0
iface eth0 inet static
	address 192.236.1.4
	netmask 255.255.255.0
	gateway 192.236.1.1
```

- Bedahulu
```
auto eth0
iface eth0 inet static
    address 192.236.1.5
    netmask 255.255.255.0
    gateway 192.236.1.1
```

- Jayanegara
```
auto eth0
iface eth0 inet static
    address 192.236.2.5
    netmask 255.255.255.0
    gateway 192.236.2.1
```

- Kotalingga
```
auto eth0
iface eth0 inet static
    address 192.236.2.4
    netmask 255.255.255.0
    gateway 192.236.2.1
```

- Anusapati
```
auto eth0
iface eth0 inet static
    address 192.236.2.3
    netmask 255.255.255.0
    gateway 192.236.2.1
```

- Majapahit
```
auto eth0
iface eth0 inet static
    address 192.236.2.2
    netmask 255.255.255.0
    gateway 192.236.2.1
```

# NO 1
Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave. 

- Dapatkan nameserver dulu lewat Nusantara
```
cat /etc/resolv.conf
```
Darisana didapatkan bahwa nameservernya adalah 192.168.122.1
- Gunakan nano /root/.bashrc pada node selain Nusantara dan masukkan berikut di bagian paling bawah
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
```
- Untuk Sriwijaya tambahkan script berikut
```
apt-get update
apt-get install bind9 -y
```
- Untuk Nusantara gunakan command berikut
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.236.0.0/16
```
- Reload seluruh node, lalu lakukan tes PING menggunakan setiap node untuk memastikan jaringannya aman
Dokumentasi:
![Screenshot (280)](https://github.com/user-attachments/assets/e0667c0b-18d4-4c7d-adb1-52b7cc5cb4e6)

![Screenshot (281)](https://github.com/user-attachments/assets/106c36eb-0268-4180-a076-cdc0bac49f96)

# NO 2
Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke **Solok** dengan alamat **sudarsana.xxxx.com** dengan alias **www.sudarsana.xxxx.com**, dimana **xxxx merupakan kode kelompok**. Contoh: sudarsana.it01.com.
- Gunakan command
```
nano /etc/resolv.conf
```
Dan tambahkan ip milik sriwijaya
```
nameserver 192.236.1.3
```
- Jalankan script berikut untuk menambahkan script kedalam /root/.bashrc
```
nano /root/.bashrc
```
```
nano /root/.bashrc
```
```
#!/bin/bash

echo 'zone "sudarsana.it06.com" {
	type master;
	file "/etc/bind/jarkom/sudarsana.it06.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it06.it06.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it06.com. sudarsana.it06.com. (
                        2024100121      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it06.com.
@       IN      A        192.236.3.2    ; IP Solok
www     IN      CNAME   sudarsana.it06.com.' > /etc/bind/jarkom/sudarsana.it06.com

service bind9 restart
```
- Save, lalu jalankan script dengan
```
chmod +x ./sriwijaya2
```
- Coba PING ke web tersebut lewat Sriwijaya dengan command
```
ping sudarsana.it06.com
```
- Dokumentasi:
![Screenshot (283)](https://github.com/user-attachments/assets/9662dcb1-65c2-4013-8110-b5d428d14b85)


# NO 3
Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu **pasopati.xxxx.com** dengan **alias www.pasopati.xxxx.com** yang mengarah ke **Kotalingga**.
- Jalankan script berikut untuk menambahkan script kedalam /root/.bashrc
```
nano /root/.bashrc
```
Lalu, masukkan script berikut kedalamnya
```
#!/bin/bash

echo 'zone "pasopati.it06.com" {
	type master;
	file "/etc/bind/jarkom/pasopati.it06.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/pasopati.it06.it06.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it06.com. pasopati.it06.com. (
                        2024100121      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it06.com.
@       IN      A       10.81.3.2     ; IP Kotalingga
www     IN      CNAME   pasopati.it06.com.' > /etc/bind/jarkom/pasopati.it06.com

service bind9 restart
```
- Save, lalu jalankan script dengan
```
chmod +x ./sriwijaya3
```
- Coba PING ke web tersebut lewat Sriwijaya dengan command
```
ping pasopati.it06.com
```
- Dokumentasi:
![Screenshot (286)](https://github.com/user-attachments/assets/4e041d90-97d1-47f6-8b7e-d0d41d3e471b)

# NO 4
Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke **Tanjungkulai** dan domain yang ingin digunakan adalah **rujapala.xxxx.com** dengan alias **www.rujapala.xxxx.com**.
Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke **Solok** dengan alamat **sudarsana.xxxx.com** dengan alias **www.sudarsana.xxxx.com**, dimana **xxxx merupakan kode kelompok**. Contoh: sudarsana.it01.com.
- Jalankan script berikut untuk menambahkan script kedalam /root/.bashrc
```
nano /root/.bashrc
```
Lalu, masukkan script berikut kedalamnya
```
#!/bin/bash

echo 'zone "rujapala.it06.com" {
	type master;
	file "/etc/bind/jarkom/rujapala.it06.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/rujapala.it06.it06.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it06.com. rujapala.it06.com. (
                        2024100121      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it06.com.
@       IN      A       192.236.1.4     ; IP Tanjungkula
www     IN      CNAME   rujapala.it06.com.' > /etc/bind/jarkom/rujapala.it06.com

service bind9 restart
```
- Save, lalu jalankan script dengan
```
chmod +x ./sriwijaya3
```
- Coba PING ke web tersebut lewat Sriwijaya dengan command
```
ping rujapala.it06.com
```
- Dokumentasi:
![Screenshot (287)](https://github.com/user-attachments/assets/eff44ad6-be70-4165-b768-cc7cb2b95419)

# NO 5
Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Nusantara.

- Disini, kita akan mencoba ping ke server **sudarsana.it06.com , pasopati.it06.com , rujapala.it06.com** dalam **SANJAYA, JAYANEGARA, dan ANUSAPATI** (client). Gunakan command berikut untuk mencobanya

```
ping -c 2 sudarsana.it06.com
```
```
ping -c 2 pasopati.it06.com
```
```
ping -c 2 rujapala.it06.com
```

- Dokumentasi:
  **SANJAYA**

  ![Screenshot (290)](https://github.com/user-attachments/assets/71cea925-9d78-45a0-83b7-144abb22a61a)

  **JAYANEGARA**

  ![Screenshot (291)](https://github.com/user-attachments/assets/4be15933-746f-4a05-a624-f654503ffad6)

 **ANUSAPATI**

  ![Screenshot (292)](https://github.com/user-attachments/assets/f87ec815-f803-40db-871a-06c35b9e98a9)


# NO 6
Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain pasopati.xxxx.com melalui alamat IP Kotalingga (Notes: menggunakan pointer record).

# NO 7
Akhir-akhir ini seringkali terjadi serangan brainrot ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Tanjungkulai untuk semua domain yang sudah dibuat sebelumnya.

# NO 8
Kamu juga diperintahkan untuk membuat subdomain khusus melacak kekuatan tersembunyi di Ohio dengan subdomain cakra.sudarsana.xxxx.com yang mengarah ke Bedahulu.

