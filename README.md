# Praktikum Modul 02 Komunikasi Data dan Jaringan Komputer
## Kelompok IT02
### Anggota Kelompok :
|             Nama              |     NRP    |
|-------------------------------|------------|
| Gallant Damas Hayuaji         | 5027231037 |
| Danar Bagus Rasendriya        | 5027231055 |

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
www     IN       CNAME     sudarsana.it02.com.

```

Setelah memasukkan konfigurasi, perlu dilakukan restart bind9.
```
service bind9 restart
```

## Soal no 3
> Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu pasopati.xxxx.com dengan alias www.pasopati.xxxx.com yang mengarah ke Kotalingga.

Membuka web console pada node Sriwijaya dan melakukan update

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

Simpan konfigurasi dengan `ctrl + x`.
Setelah itu perlu merestart bind9.

```
service bind9 restart
```

## Soal no 4
> Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke Tanjungkulai dan domain yang ingin digunakan adalah rujapala.xxxx.com dengan alias www.rujapala.xxxx.com.

Membuka web console pada node Sriwijaya dan melakukan update

```
nano /etc/bind/named.conf.local
```

```
zone “rujapala.it02.com”
{
type master;
file “/etc/bind/it02/rujapala.it02.com”;
};
```

```
cp /etc/bind/db.local /etc/bind/it02/rujapala.it02.com
```

```
nano /etc/bind/it02/rujapala.it02.com
```

Menambahkan konfigurasi di bawah ini

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it02.com. root.rujapala.it02.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it02.com.
@       IN      A       192.234.3.4
@       IN      AAAA    ::1
www     IN      CNAME   rujapala.it02.com.
```


```
service bind9 restart
```

## Soal no 5
>Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Nusantara.

Agar domain dapat diakses, maka perlu penambahan konfigurasi pada komputer/client. Client ditandai dengan simbol komputer, yaitu `HayamWuruk`, `Srikandi`, dan `AlbertEinstein`.

Konfigurasi yang ditambahkan adalah sebagai berikut

```
up echo nameserver 192.234.2.2 >> /etc/resolv.conf
up echo nameserver 192.234.1.3 >> /etc/resolv.conf
```

HayamWuruk

```
auto eth0
iface eth0 inet static
	address 192.234.1.2
	netmask 255.255.255.0
	gateway 192.234.1.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf

up echo nameserver 192.234.2.2 >> /etc/resolv.conf
up echo nameserver 192.234.1.3 >> /etc/resolv.conf
```

Srikandi

```
auto eth0
iface eth0 inet static
	address 192.234.1.5
	netmask 255.255.255.0
	gateway 192.234.1.1
up echo nameserver 192.168.122.1 > /etc/resolv.conf

up echo nameserver 192.234.2.2 >> /etc/resolv.conf
up echo nameserver 192.234.1.3 >> /etc/resolv.conf
```

AlbertEinstein

```
auto eth0
iface eth0 inet static
	address 192.234.3.5
	netmask 255.255.255.0
	gateway 192.234.3.1
up echo nameserver 192.168.122.1 > /etc/resolv.conf

up echo nameserver 192.234.2.2 >> /etc/resolv.conf
up echo nameserver 192.234.1.3 >> /etc/resolv.conf
```

Untuk mengecek apakah domain dapat diakses, maka dilakukan ping pada client tersebut

```
ping sudarsana.it02.com -c 4
ping pasopati.it02.com -c 4
ping rujapala.it02.com -c 4
```

Pada soal nomor 4 ini saya mengalami kendala, yaitu belum bisa berhasil melakukan ping pada setiap node untuk memastikan bahwa domain dapat diakses.
Tampilan yang didapatkan adalah sebagai berikut:

![image](https://github.com/user-attachments/assets/b314843d-4814-4d97-9e8c-4566bb80de98)

![image](https://github.com/user-attachments/assets/32deb5a0-b429-43fd-bc80-8802429afda3)

![image](https://github.com/user-attachments/assets/c1d7447f-33a0-48c0-9002-d761a33bdf6e)

## Soal no 6
> Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain pasopati.xxxx.com melalui alamat IP Kotalingga (Notes: menggunakan pointer record).

Alamat IP Kotalingga sesuai dengan topologi adalah `192.234.3.2`

Saya menjalankan command berikut di web console Sriwijaya

```
nano /etc/bind/named.conf.local
```

Karena IP yang saya gunakan adalah `192.234.3.x`, maka perlu di-reverse menjadi `3.234.192`.

```
zone “3.234.192.in-addr.arpa” {
type master;
file “/etc/bind/it02/3.234.192.in-addr.arpa”;
};
```

```
cp /etc/bind/db.local /etc/bind/it02/3.234.192.in-addr.arpa
```

```
nano /etc/bind/it02/3.234.192.in-addr.arpa
```

Konfigurasinya adalah sebagai berikut

```
;
; BIND data file for local loopback interface
;
$TTL	604800
@	IN	SOA	pasopati.it02.com. root.pasopati.it02.com. (
			      2		; Serial
			 604800		; Refresh
			  84600		; Retry
                        2419200		; Expire
			 604800 )	; Negative Cache TTL
;
3.234.192.in-addr.arpa.		IN	NS	pasopati.it02.com.
3				IN	PTR	pasopati.it02.com.
```

Setelah di-save, perlu dilakukan restart bind9

```
service bind9 restart
```

Melakukan pengujian pada client dengan cara

```
host -t PTR 192.234.3.2
```

Saat menguji pada client, saya masih mengalami kendala, yaitu terjadi time out.

![Screenshot 2024-10-04 030723](https://github.com/user-attachments/assets/d6ee8642-39ed-4a4c-ada3-9ca9bb7599d3)

![Screenshot 2024-10-04 030730](https://github.com/user-attachments/assets/6180ef93-6769-4fdc-b85c-eb55b3ab3cb4)

![Screenshot 2024-10-04 030739](https://github.com/user-attachments/assets/6a211419-67dc-483e-9aee-c74e4d48d4b9)


***
end
***
***
***
