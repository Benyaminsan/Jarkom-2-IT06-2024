# Jarkom-2-IT06-2024
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
- Lalu masukkan script berikut
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
@       IN      A       192.236.2.4     ; IP Kotalingga
www     IN      CNAME   pasopati.it06.com.' > /etc/bind/jarkom/pasopati.it06.com

service bind9 restart
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

- Masukkan kedalam Sriwijaya
```
nano /root/.bashrc
```
```
#!/bin/bash

# Buat reverse DNS (Record PTR)
echo 'zone "2.236.192.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.236.192.in-addr.arpa";
};' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/2.236.192.in-addr.arpa

echo ';
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it06.com. root.pasopati.it06.com. (
                        2024050301      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
2.236.192.in-addr.arpa.    IN      NS      pasopati.it06.com.
3                       IN      PTR    pasopati.it06.com.' > /etc/bind/jarkom/2.236.192.in-addr.arpa

service bind9 restart
```
- Masukkan kedalam Sanjaya, Jayanegara, dan Anusapati

```
nano /root/.bashrc
```

```
#!/bin/bash

# Set nameserver kembali ke Ip Nusantara
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

# Install package dnsutils
apt-get update
apt-get install dnsutils -y

# Kembalikan nameserver ke Ip Sriwijaya & Majapahit
echo '
nameserver 192.236.1.3
nameserver 192.236.2.2' > /etc/resolv.conf
```
- Dokumentasi:
# NO 7
Akhir-akhir ini seringkali terjadi serangan brainrot ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Tanjungkulai untuk semua domain yang sudah dibuat sebelumnya.
- Masukkan kedalam Sriwijaya
```
nano /root/.bashrc
```
```
# Tambahkan kebutuhan untuk menjadi master sriwijaya
echo '
zone "sudarsana.it06.com" {
        type master;
        notify yes;
        also-notify { 192.236.1.4; }; //IP Tanjungkulai 
        allow-transfer { 192.236.1.4; }; //IP Tanjungkulai 
        file "/etc/bind/jarkom/sudarsana.it06.com";
};

zone "pasopati.it06.com" {
        type master;
        notify yes;
        also-notify { 192.236.1.4; }; //IP Tanjungkulai 
        allow-transfer { 192.236.1.4; }; //IP Tanjungkulai 
        file "/etc/bind/jarkom/pasopati.it06.com";
};

zone "rujapala.it06.com" {
        type master;
        notify yes;
        also-notify { 192.236.1.4; }; //IP Tanjungkulai 
        allow-transfer { 192.236.1.4; }; //IP Tanjungkulai 
        file "/etc/bind/jarkom/rujapala.it06.com";
};' > /etc/bind/named.conf.local

service bind9 restart
```
- Masukkan kedalam Tanjungkulai
```
#!/bin/bash

# Tambahkan keperluan untuk menjadi slave TANJUNGKULAI

# Cek apakah bind9 sudah terinstal
if ! command -v named &> /dev/null
then
    echo "Bind9 belum terinstal, melakukan instalasi..."
    # Melakukan instalasi bind9
    apt-get update
    apt-get install bind9 -y
else
    echo "Bind9 sudah terinstal."
fi

echo '
zone "sudarsana.it06.com" {
    type slave;
    masters { 192.236.1.3; }; // IP SRIWIJAYA
    file "/var/lib/bind/sudarsana.it06.com";
};

zone "pasopati.it06.com" {
    type slave;
    masters { 192.236.1.3; }; // IP SRIWIJAYA
    file "/var/lib/bind/pasopati.it06.com";
};

zone "rujapala.it06.com" {
    type slave;
    masters { 192.236.1.3; }; // IP SRIWIJAYA
    file "/var/lib/bind/rujapala.it06.com";

};' > /etc/bind/named.conf.local

service bind9 restart
```
- Dokumentasi:

# NO 8
Kamu juga diperintahkan untuk membuat subdomain khusus melacak kekuatan tersembunyi di Ohio dengan subdomain cakra.sudarsana.xxxx.com yang mengarah ke Bedahulu.
- Masukkan script dibawah ini kedalam Sriwijaya

```
#!/bin/bash
apt update
apt install bind9 -y

echo 'zone "sudarsana.it06.com" {
	type master;
	file "/etc/bind/jarkom/sudarsana.it06.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it06.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it06.com. root.sudarsana.it06.com. (
                        2024100318      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it06.com.
@         IN      A       192.236.3.2     ; IP Solok
www     IN      CNAME   sudarsana.it06.com.
cakra    IN      A       192.236.1.5     ; IP Bedahulu
www     IN      CNAME   cakra.sudarsana.it06.com.' > /etc/bind/jarkom/sudarsana.it06.com
```
- Lalu coba PING menggunakan client Anusapati
```
ping cakra.sudarsana.it06.com
```
```
ping www.cakra.sudarsana.it06.com
```
- Dokumentasi:
![image](https://github.com/user-attachments/assets/128a56b4-ce30-40f2-ad76-95661a1144ca)

# NO 9
Karena terjadi serangan DDOS oleh shikanoko nokonoko koshitantan (NUN), sehingga sistem komunikasinya terhalang. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan dari siren man oleh Frekuensi Freak dan memasukkannya ke subdomain panah.pasopati.xxxx.com dalam folder panah dan pastikan dapat diakses secara mudah dengan menambahkan alias www.panah.pasopati.xxxx.com dan mendelegasikan subdomain tersebut ke Tanjungkulai dengan alamat IP menuju radar di Kotalingga.
- Masukkan script berikut kedalam Sriwijaya:
```
#!/bin/bash


echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it06.com. root.pasopati.it06.com. (
                        2024100319                   ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@         IN      NS      pasopati.it06.com.
@         IN      A       192.236.2.4    ; IP Kotalingga
www     IN      CNAME   pasopati.it06.com.
panah   IN      A       192.236.2.2     ; Delegasikan ke Majapahit
ns1       IN      A       192.236.2.2    ; Delegasikan ke Majapahit
panah   IN      NS      ns1' > /etc/bind/jarkom/pasopati.it06.com

echo '
options {
        directory "/var/cache/bind";

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```
- Masukkan script berikut ke Majapahit:
```
#!/bin/bash

echo '
options {
        directory "/var/cache/bind";

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

echo 'zone "panah.pasopati.it06.com" {
	type master;
	file "/etc/bind/panah/panah.pasopati.it06.com";
};' >> /etc/bind/named.conf.local

mkdir /etc/bind/panah

cp /etc/bind/db.local /etc/bind/panah/panah.pasopati.it06.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it06.com. root.panah.pasopati.it06.com. (
                        2024100119                    ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it06.com.
@       IN      A       192.236.2.2     ; IP Majapahit
www.panah     IN      CNAME   panah.pasopati.it06.com.' > /etc/bind/panah/panah.pasopati.it06.com

service bind9 restart
```
- Gunakan command dibawah untuk nge-test PING
```
ping panah.pasopati.it06.com
```
```
ping www.panah.pasopati.it06.com
```
- Dokumentasi:
**SANJAYA**
  ![Screenshot (300)](https://github.com/user-attachments/assets/3a050153-4e30-4586-b188-3d7cb961cb96)
**ANUSAPATI**
  ![Screenshot (301)](https://github.com/user-attachments/assets/45a420e2-5c79-4c68-b33f-8f88c346d887)
**JAYANEGARA**
  ![Screenshot (302)](https://github.com/user-attachments/assets/696a622d-e945-492d-9ae9-270cda16b05a)

# NO 10
Markas juga meminta catatan kapan saja meme brain rot akan dijatuhkan, maka buatlah subdomain baru di subdomain panah yaitu log.panah.pasopati.xxxx.com serta aliasnya www.log.panah.pasopati.xxxx.com yang juga mengarah ke Kotalingga.
- Masukkan script berikut ke Majapahit:
```
#!/bin/bash

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it06.com. root.panah.pasopati.it06.com. (
                         2024100618     ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it36.com.
@       IN      A       192.236.2.4     ; IP Kotalingga
www     IN      CNAME   panah.pasopati.it06.com.
log     IN      A       192.236.2.4     ; IP Kotalingga
www.log IN      CNAME   panah.pasopati.it06.com.' > /etc/bind/panah/panah.pasopati.it06.com

service bind9 restart
```
- Gunakan command dibawah untuk nge-test PING
```
ping log.panah.pasopati.it06.com
```
```
ping www.log.panah.pasopati.it06.com
```

- Dokumentasi:

# NO 11
Setelah pertempuran mereda, warga IT dapat kembali mengakses jaringan luar dan menikmati meme brainrot terbaru, tetapi hanya warga Majapahit saja yang dapat mengakses jaringan luar secara langsung. Buatlah konfigurasi agar warga IT yang berada diluar Majapahit dapat mengakses jaringan luar melalui DNS Server Majapahit.
- Masukkan script berikut ke Sriwijaya:
```
#!/bin/bash

echo '
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1; // IP Nusantara
        };
        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```
- Masukkan script berikut ke Majapahit:
```
#!/bin/bash

echo '
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1; // IP Nusantara
        };
        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```
- Coba ping google.com beberapa kali lewat client:
```
ping -c 5 google.com
```
- Dokumentasi:

# NO 12
Karena pusat ingin sebuah laman web yang ingin digunakan untuk memantau kondisi kota lainnya maka deploy laman web ini (cek resource yg lb) pada Kotalingga menggunakan apache.
- Masukkan script berikut ke Sriwijaya dan Majapahit:
```
#!/bin/bash

# tambahkan nameserver IP Kotalingga
echo '
nameserver 192.168.122.1
nameserver 192.236.2.4' > /etc/resolv.conf
```
- Masukkan script berikut ke Sanjaya, Jayanegara dan Anusapati:
```
#!/bin/bash

if ! command -v named &> /dev/null
then
    echo "Lynx belum terinstal, melakukan instalasi..."
    # Instalasi lynx
    apt-get update
    apt-get install lynx -y
else
    echo "lynx sudah terinstal."
fi

```
- Masukkan script berikut ke Kotalingga:
```
(Kotalingga)
#!/bin/bash

if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    # Instalasi apache2
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "Unzip belum terinstal, melakukan instalasi..."
    # Instalasi unzip
    apt-get update
    apt-get install unzip -y
else
    echo "unzip sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    # Melakukan instalasi php
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

curl -L -o lb.zip --insecure "https://drive.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7"

unzip lb.zip

rm -rf /var/www/html/index.php

cp worker/index.php /var/www/html/index.php

service apache2 restart
```
- Gunakan command dibawah untuk test:
```
lynx http://192.236.2.4/index.php
```
- Dokumentasi:

# NO 13
- Masukkan script berikut ke Sriwijaya dan Majapahit:
```
#!/bin/bash

# tambahkan nameserver IP Solok
echo '
nameserver 192.168.122.1
nameserver 192.236.3.2 ' > /etc/resolv.conf
```
- Masukkan script berikut ke Tanjungkulai, Kotalingga, dan Bedahulu
```
#!/bin/bash

if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    # Instalasi apache2
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "Unzip belum terinstal, melakukan instalasi..."
    # Instalasi unzip
    apt-get update
    apt-get install unzip -y
else
    echo "unzip sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    # Melakukan instalasi php
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

curl -L -o lb.zip --insecure "https://drive.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7"

unzip lb.zip

rm -rf /var/www/html/index.php

cp worker/index.php /var/www/html/index.php

service apache2 restart
```
- Masukkan script berikut ke Solok
```
#!/bin/bash

if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    # Instalasi apache2
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    # Melakukan instalasi php
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

# Enable apache2 module
a2enmod proxy_balancer
a2enmod proxy_http
a2enmod lbmethod_byrequests
echo '
<VirtualHost *:80>
    <Proxy balancer://serverpool>
        BalancerMember http://192.236.2.4/
        BalancerMember http://192.236.1.4/
        BalancerMember http://192.236.1.5/
        Proxyset lbmethod=byrequests
    </Proxy>
    ProxyPass / balancer://serverpool/
    ProxyPassReverse / balancer://serverpool/
</VirtualHost>' > /etc/apache2/sites-available/000-default.conf

service apache2 restart
```
- Test dengan command berikut:
```
lynx http://192.236.3.2 /index.php
```
- Dokumentasi:

# NO 14
Selama melakukan penjarahan mereka melihat bagaimana web server luar negeri, hal ini membuat mereka iri, dengki, sirik dan ingin flexing sehingga meminta agar web server dan load balancer nya diubah menjadi nginx.
- Masukkan script berikut ke Sriwijaya
```
#!/bin/bash
apt-get update
apt-get install dnsutils lynx nginx apache2 libapache2-mod-php7.0 wget unzip php php-fpm -y

service php7.0-fpm start
service nginx start

mkdir -p /var/www/html/config/

wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1xn03kTB27K872cokqwEIlk8Zb121HnfB' -O /var/www/html/config/lb.zip

unzip /var/www/html/config/lb.zip -d /var/www/html/config/

mv /var/www/html/config/worker/index.php /var/www/html/

rm -rf /var/www/html/config/

echo 'server {
    listen 8082;

    root /var/www/html;

    index index.php index.html index.htm;
    server_name _;

    location / {
        try_files \$uri \$uri/ /index.php?\$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
     deny all;
    }

    error_log /var/log/nginx/jarkom_error.log;
    access_log /var/log/nginx/jarkom_access.log;
}' > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
rm /etc/nginx/sites-enabled/default
service nginx restart
```

- Masukkan script berikut ke Solok
```
#!/bin/bash
apt-get update
apt-get install dnsutils nginx php-fpm php -y

service php7.0-fpm start
service nginx start

echo 'upstream solok {
    server 192.236.2.4:8082; # IP Kotalingga
    server 192.236.1.4:8083; # IP Bedahulu
    server 192.236.1.5:8084; # IP Tanjungkulai
}

server {
  listen 8082;
  server_name 192.236.3.2;

  location / {
    proxy_pass http://solok;
  }
}

server {
  listen 8083;
  server_name 192.236.3.2;

  location / {
    proxy_pass http://solok;
  }
}

server {
  listen 8084;
  server_name 192.236.3.2;

  location / {
    proxy_pass http://solok;
  }
}' > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
rm /etc/nginx/sites-enabled/default
service nginx restart
```
- Test dengan command berikut:
```
lynx 192.236.3.2:8082/index.php
```
- Dokumentasi:

# NO 15
Markas pusat meminta laporan hasil benchmark dengan menggunakan apache benchmark dari load balancer dengan 2 web server yang berbeda tersebut dan meminta secara detail dengan ketentuan:
Nama Algoritma Load Balancer
Report hasil testing apache benchmark 
Grafik request per second untuk masing masing algoritma. 
Analisis
Meme terbaik kalian (terserah ( ͡° ͜ʖ ͡°)) 🤓
```
```
- Dokumentasi:
# NO 16
Karena dirasa kurang aman dari brainrot karena masih memakai IP, markas ingin akses ke Solok memakai solok.xxxx.com dengan alias www.solok.xxxx.com (sesuai web server terbaik hasil analisis kalian).
- Masukkan script berikut ke Sriwijaya:
```
```
- Dokumentasi:
# NO 17
Agar aman, buatlah konfigurasi agar solok.xxx.com hanya dapat diakses melalui port sebesar π x 10^3 = (phi nya desimal) dan 2000 + 2000 log 10 (10) +700 - π = ?.
```
```
- Dokumentasi:
# NO 18
Apa bila ada yang mencoba mengakses IP mylta akan secara otomatis dialihkan ke www.solok.xxxx.com.
```
```
- Dokumentasi:
# NO 19
Karena probset sudah kehabisan ide masuk ke salah satu worker buatkan akses direktori listing yang mengarah ke resource worker2.
```
```
- Dokumentasi:
# NO 20
Worker tersebut harus dapat di akses dengan sekiantterimakasih.xxxx.com dengan alias www.sekiantterimakasih.xxxx.com.
```
```
- Dokumentasi:
