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

```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.41.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.41.2.1
	netmask 255.255.255.0

    
auto eth3
iface eth2 inet static
	address 10.41.3.1
	netmask 255.255.255.0
```

**Konfigurasi SSS**

```
auto eth0
iface eth0 inet static
	address 10.41.1.2
	netmask 255.255.255.0
	gateway 10.41.1.1
```

**Konfigurasi Garden**

```
auto eth0
iface eth0 inet static
	address 10.41.1.3
	netmask 255.255.255.0
	gateway 10.41.1.1
```

**Konfigurasi WISE**

```
auto eth0
iface eth0 inet static
	address 10.41.3.2
	netmask 255.255.255.0
	gateway 10.41.3.1
```

**Konfigurasi Berlint**

```
auto eth0
iface eth0 inet static
	address 10.41.2.2
	netmask 255.255.255.0
	gateway 10.41.2.1
```

**Konfigurasi Eden**

```
auto eth0
iface eth0 inet static
	address 10.41.2.3
	netmask 255.255.255.0
	gateway 10.41.2.1
```


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

**Pada Wise**

Pada Wise kita mengonfigurasikan wise.ITA03.com untuk mendelegasikan Wise ke Berlin

Menambahkan ns1 ke alamat Berlint

![image](https://user-images.githubusercontent.com/90241942/198831860-adc39c15-453f-4959-b029-9ed22d74be5e.png)

Menambahkan konfigurasi ke `/etc/bind/named.conf.options`

![image](https://user-images.githubusercontent.com/90241942/198831917-e06a60c2-9242-4d9f-9e0b-517837a4b906.png)

Pada konfigurasi di atas kita menambahkan `allow-query{any;};`

**Pada Berlint**

Menambahkan konfigurasi ke `/etc/bind/named.conf.options`

![image](https://user-images.githubusercontent.com/90241942/198831932-d30796ce-aada-4007-8292-d5ad21f2f326.png)

Pada konfigurasi di atas kita menambahkan `allow-query{any;};`

Menambahkan zone untuk alamat operation.wise.ita03.com dengan type master dan folder konfigurasinya di folder operation

![image](https://user-images.githubusercontent.com/90241942/198831939-2f41a05d-dcdc-4b6c-9f67-ef1712846b45.png)

Dapat dilihat bahwa domain  akan mengarah ke alamat Eden

![image](https://user-images.githubusercontent.com/90241942/198833106-ed0f9994-0fd5-465d-8cc0-022fb3abbd60.png)

## Nomor 7
Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden.

### Jawaban Nomor 7

Pertama menambahkan zone untuk operation.wise.ITA03.com kemudian dikonfigurasikan seperti ini

![image](https://user-images.githubusercontent.com/90241942/198833133-dad3dae0-6f4e-467f-94b0-f8e708f138a4.png)

## Nomor 8 (Revisi)
setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.wise.yyy.com. Pertama, Loid membutuhkan webserver dengan DocumentRoot pada /var/www/wise.yyy.com

## Nomor 9 (REvisi)
Setelah itu, Loid juga membutuhkan agar url www.wise.yyy.com/index.php/home dapat menjadi menjadi www.wise.yyy.com/home

### Jawaban Nomor 8 & 9
Untuk mengerjakan soal ini, kami perlu menginstall apache2 terlebih dahulu dengan `apt-get install libapache2-mod-php7.0 -y` pada node eden
dilanjutkan dengan tambahkan file wise.ita03.com.conf pada /etc/apache2/sites-available sehingga konfigurasinya akan menjadi sebagai berikut:
```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/wise.ita03.com
        ServerName wise.ita03.com
        ServerAlias www.wise.ita03.com
        
        <Directory /var/www/wise.ita03.com/>
                Options +Indexes
        </Directory>
 
        Alias \"/home\" \"/var/www/wise.ita03.com/index.php/home\"
        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```
Pada file konfigurasi tersebut document root sudah diarahkan ke `/var/www/wise.yyy.com` dan ditambahkan alias agar url `www.wise.yyy.com/index.php/home` dapat menjadi `menjadi www.wise.yyy.com/home \`

Setelah itu, kita akan membuat sebuah folder bernama `wise.ita03.com` di dalam `/var/www/` dan mendownload isi dari `wise.ita03.com` dengan command sebagai berikut:
```
wget "https://drive.google.com/uc?id=1q9g6nM85bW5T9f5yoyXtDqonUKKCHOTV&export=download" -O eden.wise.zip
```
```
unzip eden.wise.zip
```
```
mv eden.wise/* /var/www/eden.wise.ITB10.com
```

### Testing

sebelum melakukan testing, kita perlu mengganti IP `wise.ita03.com` pada node wise dan juga pada reverse domainnya
```
\$TTL    604800
@       IN      SOA     wise.ita03.com. root.wise.ita03.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@             IN      NS      wise.ita03.com.
@             IN      A       10.41.3.2 ; IP WISE
@             IN      AAAA    ::1
www           IN      CNAME   wise.ita03.com.
eden          IN      A       10.41.2.3 ; IP Eden
www.eden      IN      CNAME   eden.wise.ita03.com
ns1           IN      A       10.41.2.2 ; IP Berlint
operation     IN      NS      ns1
www.operation IN      CNAME   wise.ita03.com
```

```
\$TTL    604800
@       IN      SOA     wise.ita03.com.com. root.wise.ita03.com.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
3.41.10.in-addr.arpa.   IN      NS      wise.ita03.com.
2                       IN      PTR     wise.ita03.com.
```

Setelah itu, kami *enable* `wise.ita03.com` dengan command berikut:
```
a2ensite wise.ita03.com
```

Lalu, akan kami *reload* apache2-nya dengan command
```
service apache2 reload
```
Selanjutnya, kami install lynx pada node eden jika sudah berhasil akan muncul tampilan seperti dibawah:

![image](https://user-images.githubusercontent.com/90241858/198820064-fdb6270a-d9cb-4dbc-b865-587e4d244558.png)


## Nomor 10 (Revisi)
Setelah itu, pada subdomain www.eden.wise.yyy.com, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com

## Nomor 11 (Revisi)
Akan tetapi, pada folder /public, Loid ingin hanya dapat melakukan directory listing saja

### Jawaban Nomor 10&11
Pertama-tama, kami membuat folder `eden.wise.ita03.com` lalu mendownload file dan mengunzip file tersebut ke dalam folder `eden.wise.ita03.com` menggunakan command berikut:
Download:
```
wget "https://drive.google.com/uc?id=1q9g6nM85bW5T9f5yoyXtDqonUKKCHOTV&export=download" -O eden.wise.zip
```
Unzip:
```
unzip eden.wise.zip
```
Memindahkan file:
```
mv eden.wise/* /var/www/eden.wise.ITB10.com
```

Setelah itu, kami membuat file `eden.wise.ita03.com.conf` pada `/etc/apache2/sites-available ` yang berisi konfigurasi berikut:
```
<VirtualHost *:80>
-
-
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/eden.wise.ita03.com
        ServerName eden.wise.ita03.com
        ServerAlias www.eden.wise.ita03.com
       
        <Directory /var/www/eden.wise.ita03.com/public>
                Options +Indexes
        </Directory>
-
-
</VirtualHost>
```

## Nomor 12 (Revisi)
Tidak hanya itu, Loid juga ingin menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache

### Jawaban Nomor 12
Untuk nomor 12, kami menambahkan konfigurasi baru ke dalam `/etc/apache2/sites-available/eden.wise.ita03.com.conf` seperti berikut:
```
<VirtualHost *:80>
-
-
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/eden.wise.ita03.com
        ServerName eden.wise.ita03.com
        ServerAlias www.eden.wise.ita03.com
       
        <Directory /var/www/eden.wise.ita03.com/public>
                Options +Indexes
        </Directory>
-
-
</VirtualHost>
```
