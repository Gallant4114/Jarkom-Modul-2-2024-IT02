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

Namun, setelah beberapa kali saya mencoba, client `HayamWuruk` dan `Srikandi` berhasil melakukan ping dengan hasil sebagai berikut

Node HayamWuruk

![image](https://github.com/user-attachments/assets/2b5c0789-90ae-41f8-b155-07f9b530f153)

Node Srikandi

![image](https://github.com/user-attachments/assets/d92b377d-6023-444a-813b-07f8c21c7b82)

Client `AlbertEinstein` merespon cukup lama dan berakhir dengan gagal melakukan ping.

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
2				IN	PTR	pasopati.it02.com.
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

Setelah beberapa kali mencoba, node `Srikandi` berhasil mengarah ke `pasopati.it02.com`.

![image](https://github.com/user-attachments/assets/5431f024-6285-4789-9225-b64fc3b25fe9)

Lalu untuk node `HayamWuruk` juga sudah berhasil mengarah ke `pasopati.it02.com`.

![image](https://github.com/user-attachments/assets/c93dd73e-6fa3-4ca2-be6c-ed485f052a4f)

Node `AlbertEinstein` merespon cukup lama dan belum berhasil mengarah ke `pasopati.it02.com`.

![image](https://github.com/user-attachments/assets/470a9775-1a50-4c20-b369-26715f5cbf06)

## Soal no 7

> Akhir-akhir ini seringkali terjadi serangan brainrot ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Majapahit untuk semua domain yang sudah dibuat sebelumnya yang mengarah ke Sriwijaya.

Menjalankan command berikut di Majapahit

```
apt install bind9 dnsutils -y
```

Masuk ke `/etc/bind`

```
cd /etc/bind
```

```
nano named.conf.local
```

Memasukkan konfigurasi seperti di bawah ini

```
zone "sudarsana.it02.com"
   {
    type master;
    file "/etc/bind/it02/sudarsana.it02.com";
   };

zone "pasopati.it02.com"
   {
    type master;
    file "/etc/bind/it02/pasopati.it02.com";
   };

zone "rujapala.it02.com"
   {
    type master;
    file "/etc/bind/it02/rujapala.it02.com";
   };

zone "3.234.192.in-addr.arpa"
   {
    type master;
    file "/etc/bind/it02/3.234.192.in-addr.arpa";
   };
```

Selanjutnya adalah merestart service bind9

```
service bind9 restart
```

![image](https://github.com/user-attachments/assets/b254489a-13a2-4cbd-a4ab-87d68d4cbc9b)

Melakukan pengujian

```
ping sudarsana.it02.com
```

Saat dilakukan pengujian, terdapat kendala belum bisa melakukan ping

![image](https://github.com/user-attachments/assets/c4205b4c-061d-4ef3-8cab-a53ee3d22dde)

## Soal no 8

> Kamu juga diperintahkan untuk membuat subdomain khusus melacak kekuatan tersembunyi di Ohio dengan subdomain cakra.sudarsana.xxxx.com yang mengarah ke Bedahulu.

IP address dari node Bedahulu adalah `192.234.3.3`

Menjalankan command berikut di node Majapahit

```
mkdir /etc/bind/it02
```

```
cp /etc/bind/db.local /etc/bind/it02/sudarsana.it02.com
```

Memasukkan konfigurasi berikut

```
      ;
      ; BIND data file for sudarsana.it02.com
      ;
      $TTL    604800
      @       IN      SOA     sudarsana.it02.com. root.sudarsana.it02.com. (
                                    2         ; Serial
                               604800         ; Refresh
                                86400         ; Retry
                              2419200         ; Expire
                               604800 )       ; Negative Cache TTL
      ;
      @       IN      NS      sudarsana.it02.com.
      @       IN      A       192.234.3.3     
      @       IN      AAAA    ::1
      www     IN      CNAME   sudarsana.it02.com.
      cakra   IN      A       10.79.1.4     
```

Melakukan restart service bind9

```
service bind9 restart
```

Melakukan pengujian

![image](https://github.com/user-attachments/assets/9c07e13e-28f0-4364-9d72-ce2958c7f0a7)
