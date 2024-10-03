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
