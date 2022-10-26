# Jarkom-Modul-2-ITA03-2022

## Anggota Kelompok
1. Muhammad Jovan Adiwijaya Yanuarsyah (5027201025)
2. Muhammad Naufal Pasya (5027201045)
3. Fatchia Farhan (5027201044)

## Deskripsi
Laporan ini dibuat dengan tujuan untuk menjelaskan pengerjaan serta permasalahan yang kami alami dalam pengerjaan soal shift Jaringan Komputer modul 2 tahun 2022.

## Gambar Topologi
<img width="525" alt="image" src="https://user-images.githubusercontent.com/90241942/197974446-52a1d39c-6492-4a82-a271-e4b592342898.png">

## Nomor 1
WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet.

### Jawaban Nomor 1

Kami membuat topologi sesuai dengan soal lalu melakukan konfigurasi untuk setiap node sebagai berikut

**Konfigurasi Ostania**

**Konfigurasi SSS**

**Konfigurasi Garden**

**Konfigurasi WISE**

**Konfigurasi Berlint**

**Konfigurasi Eden**

## Nomor 2
Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise.

### Jawaban Nomor 2

Dalam mengerjakan soal ini, pertama kami melakukan konfigurasi terhadap file `/etc/bind/named.conf.local` dengan menambahkan kode berikut ini menggunakan command `nano /etc/bind/named.conf.local`

![image](https://user-images.githubusercontent.com/90241858/197996014-57bf773a-5327-4ca6-9b21-a27355ab2e37.png)


setelah membuat konfigurasi zone untuk `wise.ita03.com` kami membuat direktori baru yaitu `/etc/bind/wise` dengan command `mkdir /etc/bind/wise` lalu menambahkan konfigurasi berikut ini pada `/etc/bind/wise/wise.ita03.com` 

![image](https://user-images.githubusercontent.com/90241858/197995690-986127c9-7b99-4371-b337-16b4a8fa203f.png)


Pada file konfigurasi diatas kami mengatur domain menjadi `wise.ita03.com` lalu membuat CNAME `www` untuk `wise.ita03.com`


## Nomor 3
Setelah itu ia juga ingin membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden.

### Jawaban Nomor 3

Kami menambahkan beberapa konfigurasi pada `/etc/bind/wise/wise.ita03.com` dengan command `nano /etc/bind/wise/wise.ita03.com` sebagai berikut

![image](https://user-images.githubusercontent.com/90241858/197996474-bbca32a7-70f8-4e05-bf32-33d568c64eff.png)

Pada kode tersebut dapat dilihat bahwa kami menambahakan subdomain `eden` lalu membuat CNAME `www.eden` sebagai alias dari `eden.wise.ita03.com`

## Nomor 4
Buat juga reverse domain untuk domain utama.

### Jawaban Nomor 4

Untuk membuat reverse domain, kami melakukan konfigurasi tambahan pada `/etc/bind/named.conf.local` dengan bantuan command `cp /etc/bind/db.local /etc/bind/jarkom/3.41.10.in-addr.arpa` dan `nano /etc/bind/jarkom/3.41.10.in-addr.arpa` sehingga menjadi seperti berikut ini :

![image](https://user-images.githubusercontent.com/90241858/197996680-b6149cdc-8cf8-4b16-84b5-795f9232789b.png)

![image](https://user-images.githubusercontent.com/90241858/197996566-bbbf4b44-ab88-4604-8667-e8955757010a.png)


###Testing

Untuk memeriksa apakah konfigurasi sudah benar kami melakukan perintah berikut di client SSS:

*masukin gambar*

## Nomor 5
Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama.

### Jawaban Nomor 5

**Pada WISE**

Untuk menjadikan Berlint sebagai DNS Slave, kami melakukan konfigurasi pada `/etc/bind/named.conf.local` menjadi sebagai berikut:

![unknown](https://user-images.githubusercontent.com/90241942/197995514-787862da-d4d3-4b2b-ab0d-9e595033d474.png)

Kami menambahkan `notify yes`, `also-notify { 10.41.2.2; };` serta `allow-transfer {10.41.2.2; };` untuk menjadikan Water 7 sebagai DNS Slave.

**Pada Berlint**

Pada Berlint pertama sekali menambahkan zone pada file `/etc/bind/named.conf.local` sebagai berikut:

<img width="306" alt="image" src="https://user-images.githubusercontent.com/90241942/197996226-e18558a7-f315-4c7f-9ed4-511de1fc12fd.png">

pada Berlint dapat dilihat bahwa type nya adalah slave dan master nya adalah WISE.

## Nomor 6
Karena banyak informasi dari Handler, buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation 

### Jawaban Nomor 6

## Nomor 7
Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden.

### Jawaban Nomor 7
