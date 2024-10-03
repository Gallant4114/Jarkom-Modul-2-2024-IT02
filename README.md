# Praktikum Modul 02 Komunikasi Data dan Jaringan Komputer
## Kelompok IT02
### Anggota Kelompok :
|             Nama              |     NRP    |
|-------------------------------|------------|


***

## Topologi
![image](https://github.com/user-attachments/assets/86844250-9f7f-40b4-b342-133e7fc5b3f1)


## Soal No 1

> Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave. 

Langkah pertama adalah membuat topologi sesuai dengan pembagian menggunakan image ubuntu.

![image](https://github.com/user-attachments/assets/2d7e4c24-0910-4da4-a885-c8b7c30018e4)

Konfigurasi yang saya gunakan untuk setiap node adalah sebagai berikut

Nusantara:

```
auto eth0
iface eth0 inet dhcp
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.234.0.0/16

auto eth1
iface eth1 inet static
	address 192.234.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.234.2.1
	netmask 255.255.255.0

auto eth3
iface eth2 inet static
	address 192.234.3.1
	netmask 255.255.255.0
```


Sriwijaya:

```
auto eth0
iface eth0 inet static
    address 192.234.2.2
    netmask 255.255.255.0
    gateway 192.234.2.1
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```


Majapahit:

```
auto eth0
iface eth0 inet static
	address 192.234.1.3
	netmask 255.255.255.0
	gateway 192.234.1.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

HayamWuruk

```
auto eth0
iface eth0 inet static
	address 192.234.1.2
	netmask 255.255.255.0
	gateway 192.234.1.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

Solok
```
auto eth0
iface eth0 inet static
	address 192.234.1.4
	netmask 255.255.255.0
	gateway 192.234.1.1
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```


Srikandi
```
auto eth0
iface eth0 inet static
	address 192.234.1.5
	netmask 255.255.255.0
	gateway 192.234.1.1
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

AlbertEinstein
```
auto eth0
iface eth0 inet static
	address 192.234.3.5
	netmask 255.255.255.0
	gateway 192.234.3.1
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```


Tanjungkulai

```
auto eth0
iface eth0 inet static
	address 192.234.3.4
	netmask 255.255.255.0
	gateway 192.234.3.1
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```


Bedahulu
```
auto eth0
iface eth0 inet static
	address 192.234.3.3
	netmask 255.255.255.0
	gateway 192.234.3.1
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```


Kotalingga
```
auto eth0
iface eth0 inet static
	address 192.234.3.2
	netmask 255.255.255.0
	gateway 192.234.3.1
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

## Soal no 2
> Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke Solok dengan alamat sudarsana.xxxx.com dengan alias www.sudarsana.xxxx.com, dimana xxxx merupakan kode kelompok. Contoh: sudarsana.it01.com.

Membuka web console pada node Sriwijaya dan melakukan update

```
apt-get update -y
```

Menginstall Bind9
```
apt-get install bind9 -y
```

```
nano /etc/bind/named.conf.local
```

```
zone “sudarsana.it02.com”
{
type master;
file “/etc/bind/it02/sudarsana.it02.com”;
};
```

```
mkdir /etc/bind/it02
```

```
cp /etc/bind/db.local /etc/bind/it02/sudarsana.it02.com
```

```
nano /etc/bind/it02/sudarsana.it02.com
```

Memasukkan konfigurasi sebagai berikut:

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN       SOA       sudarsana.it02.com. root.sudarsana.it02 (
                                 2         ; Serial
                            604800         ; Refresh
                             86400         ; Retry
                           2419200         ; Expire
                            604800 )       ; Negative Cache TTL
;
@       IN       NS        sudarsana.it02.com.
@       IN       A         192.234.1.4
@       IN       AAAA      ::1
www     IN       CNAME     sudarsana.it19.com

```

Setelah memasukkan konfigurasi, perlu dilakukan restart bind9.
```
service bind9 restart
```

## Soal no 3
> Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu pasopati.xxxx.com dengan alias www.pasopati.xxxx.com yang mengarah ke Kotalingga.

```
nano /etc/bind/named.conf.local
```

```
zone “pasopati.it02.com”
{
type master;
file “/etc/bind/it02/pasopati.it02.com”;
};
```

```
cp /etc/bind/db.local /etc/bind/it02/pasopati.it02.com
```

```
nano /etc/bind/it02/pasopati.it02.com
```

Memasukkan konfigurasi sebagai berikut

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it02.com. root.pasopati.it02.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it02.com.
@       IN      A       192.234.3.2
@       IN      AAAA    ::1
www     IN      CNAME   pasopati.it02.com.
```

Simpan konfigurasi dengan `ctrl + x`. Setelah itu perlu merestart bind9.

```
service bind9 restart
```

## Soal no 4
> Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke Tanjungkulai dan domain yang ingin digunakan adalah rujapala.xxxx.com dengan alias www.rujapala.xxxx.com.

```
nano /etc/bind/named.conf.local
```
