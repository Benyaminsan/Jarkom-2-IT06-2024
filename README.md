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

# NO 2
Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke **Solok** dengan alamat **sudarsana.xxxx.com** dengan alias **www.sudarsana.xxxx.com**, dimana **xxxx merupakan kode kelompok**. Contoh: sudarsana.it01.com.
- Buat nano file baru, disini saya namakan **sriwijaya2**
```
nano sriwijaya2
```
Lalu, masukkan script berikut kedalamnya
```
#!/bin/bash

echo 'zone "sudarsana.it06.com" {
	type master;
	file "/etc/bind/jarkom/sudarsana.it06.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it06.it23.com

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

# NO 3
Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu **pasopati.xxxx.com** dengan **alias www.pasopati.xxxx.com** yang mengarah ke **Kotalingga**.
- Buat nano file baru, disini saya namakan **sriwijaya3**
```
nano sriwijaya3
```
Lalu, masukkan script berikut kedalamnya
```
#!/bin/bash

echo 'zone "pasopati.it06.com" {
	type master;
	file "/etc/bind/jarkom/pasopati.it06.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/pasopati.it06.it23.com

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

# NO 4
Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke **Tanjungkulai** dan domain yang ingin digunakan adalah **rujapala.xxxx.com** dengan alias **www.rujapala.xxxx.com**.
Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke **Solok** dengan alamat **sudarsana.xxxx.com** dengan alias **www.sudarsana.xxxx.com**, dimana **xxxx merupakan kode kelompok**. Contoh: sudarsana.it01.com.
- Buat nano file baru, disini saya namakan **sriwijaya4**
```
nano sriwijaya4
```
Lalu, masukkan script berikut kedalamnya
```
#!/bin/bash

echo 'zone "rujapala.it06.com" {
	type master;
	file "/etc/bind/jarkom/rujapala.it06.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/rujapala.it06.it23.com

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
