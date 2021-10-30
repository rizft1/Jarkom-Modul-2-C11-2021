# Jarkom-Modul-2-C11-2021

## Anggota Kelompok C11 : <br>
- 05111940000001 Christopher Baptista
- 05111940000093 Riki Mi’roj Achmad
- 05111940000219 M Rizqi Fiqih Thalib

# Soal 1 <br>
Membuat peta atau topologi dengan EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. dibuat 2 Client yaitu Loguetow dan Alabasta kemudian semua node terhubung pada router Foosha sehingga dapat mengakses internet. Gambar sebagai berikut : <br>

![topologi](https://user-images.githubusercontent.com/62735317/139535006-fd099c8b-d573-429a-9b8a-c0e7a6f1abfd.PNG)

Menjalankan command `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.189.0.0/16` yang digunakan supaya dapat terhubung ke jaringan luar pada router `Foosha`

Kemudian menambahkan command `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.189.0.0/16` ke `/root/.bashrc` agar dijalankan setiap kali project distart dengan command `echo "iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.189.0.0/16" >> /root/.bashrc`

Setelah itu pada semua node lainnya ditambahkan command `echo "nameserver 192.189.122.0"` untuk setting IP DNS ke `/root/.bashrc` agar dijalankan setiap kali project distart dengan command `echo 'echo "nameserver 192.189.122.1" > /etc/resolv.conf' >> /root/.bashrc`


# Soal 2 <br>
Membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku. Dikerjakan sebagai berikut : 

pada EniesLobby jalankan command `apt-get update` dan `apt-get install bind9 -y` untuk menginstall bind9

Kemudian mengedit `/etc/bind/named.conf.local` dengan menambahkan :

```bash
    zone "franky.c11.com" {
            type master;
            file "/etc/bind/kaizoku/franky.c11.com";
    };
```

lalu dibuat folder kaizoku di `/etc/bind`. Lalu copy `/etc/bind/db.local` menjadi `/etc/bind/kaizoku/franky.c11.com`. Lalu konfigurasi file tersebut agar memiliki SOA `franky.c11.com.`, NS `franky.c11.com.`, record A yang mengarah ke `IP Skypie`, dan CNAME `www` pada `franky.c11.com.`.

# Soal 3
Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie. Dikerjakan sebagai berikut : 

Mengedit file `/etc/bind/kaizoku/franky.c11.com` dengan menambahkan:

```bash
        ns1     IN      A       192.189.2.4 ; IP Skypie
        super   IN      NS      ns1
```

Kemudian menambahkan zone pada `/etc/bind/named.conf.local` dengan menambahkan:

```bash
    zone "super.franky.c11.com" {
            type master;
            file "/etc/bind/kaizoku/super.franky.c11.com";
    };
```

Lalu copy `/etc/bind/db.local` menjadi `/etc/bind/kaizoku/super.franky.c11.com`. Lalu konfigurasi file tersebut agar memiliki SOA `super.franky.c11.com.`, NS `super.franky.c11.com.`, record A yang mengarah ke `IP Skypie`, dan CNAME `www` pada `super.franky.c11.com.`.

# Soal 4
Buat juga reverse domain untuk domain utama. Dikerjakan sebagai berikut :

Pertama, menambahkan zone pada `/etc/bind/named.conf.local` dengan menambahkan:

```bash
    zone "2.194.189.in-addr.arpa" {
            type master;
            file "/etc/bind/kaizoku/2.194.189.in-addr.arpa";
    };
```

Lalu copy `/etc/bind/db.local` menjadi `/etc/bind/kaizoku/2.194.189.in-addr.arpa`. Lalu konfigurasi file tersebut agar memiliki SOA `franky.c11.com.`,`2.194.189.in-addr.arpa.` yang memiliki NS `franky.c11.com.`, dan `2` yang merupakan byte ke-4 IP EniesLobby memiliki PTR `franky.c11.com.`.

# Soal 5
Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama :

Di EniesLobby:

Pertama edit zone `franky.d05.com` pada `/etc/bind/named.conf.local` menjadi:

```bash
    zone "franky.c11.com" {
            type master;
            notify yes;
            also-notify { 192.189.2.3; };
            allow-transfer { 192.189.2.3; };
            file "/etc/bind/kaizoku/franky.c11.com";
    };
```

Di Water7:
Jalankan command `apt-get update` dan `apt-get install bind9 -y` untuk menginstall bind9

Kemudian mengedit `/etc/bind/named.conf.local` dengan menambahkan :

```bash
    zone "franky.c11.com" {
        type slave;
        masters { 192.189.2.2; };
        file "/var/lib/bind/franky.c11.com";
    };
```

# Soal 6
Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo. :

Di EniesLobby:

Pertama, mengedit file `/etc/bind/kaizoku/franky.c11.com` dengan menambahkan:

```bash
        ns2     IN      A       192.189.2.3 ; IP Water7
        mecha   IN      NS      ns2
```

Kemudian mengedit file `/etc/bind/named.conf.options` dengan comment bagian `dnssec-validation auto;` dan menambahkan line `allow-query{any;};`.

Pastikan sudah ada line `allow-transfer { "IP Water7"; };` pada zone `franky.c11.com` di file `/etc/bind/named.conf.local`.

Di Water7:

Pertama edit file `/etc/bind/named.conf.options` dengan comment bagian `dnssec-validation auto;` dan menambahkan line `allow-query{any;};`.

Kemudian menambahkan zone pada `/etc/bind/named.conf.local` dengan menambahkan:

```bash
    zone "mecha.franky.c11.com" {
            type master;
            file "/etc/bind/sunnygo/mecha.franky.c11.com";
    };
```

lalu dibuat folder sunnygo di `/etc/bind`. Lalu copy `/etc/bind/db.local` menjadi `/etc/bind/sunnygo/mecha.franky.c11.com`. Lalu konfigurasi file tersebut agar memiliki SOA `mecha.franky.c11.com.`, NS `mecha.franky.c11.com.`, record A yang mengarah ke `IP Skypie`, dan CNAME `www` pada `mecha.franky.c11.com.`.

# Soal 7

Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Franky dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie. :

Di Water7:

Mengedit file `/etc/bind/sunnygo/mecha.franky.c11.com` dengan menambahkan:

```bash
        ns1     IN      A       192.189.2.4 ; IP Skypie
        general IN      NS      ns1
```

Kemudian menambahkan zone pada `/etc/bind/named.conf.local` dengan menambahkan:

```bash
    zone "general.mecha.franky.c11.com" {
            type master;
            file "/etc/bind/sunnygo/general.mecha.franky.c11.com";
    };
```

Lalu copy `/etc/bind/db.local` menjadi `/etc/bind/sunnygo/general.mecha.franky.c11.com`. Lalu konfigurasi file tersebut agar memiliki SOA `general.mecha.franky.c11.com.`, NS `general.mecha.franky.c11.com.`, record A yang mengarah ke `IP Skypie`, dan CNAME `www` pada `general.mecha.franky.c11.com.`.

# Soal 8

Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com. :

Di Skypie:
Pertama, install `apache2`, `php`, dan `libapache2-mod-php7.0`

Kemudian melakukan `wget` untuk mendownload file yang diperlukan. Setelah itu diunzip sehinggan menampilkan folder-folder seperti ini.

Setelah itu, pindah ke directory `/etc/apache2/sites-available`.Kemudian copy file `000-default.conf` menjadi file `franky.c11.com.conf`

Lalu setting file `franky.c11.com.conf` agar memiliki line `ServerName franky.c11.com`, `ServerAlias www.franky.c11.com`, dan `DocumentRoot /var/www/franky.c11.com`.

Kemudian bikin directory baru dengan nama `franky.c11.com` pada `/var/www/` menggunakan command `mkdir /var/www/franky.c11.com`. lalu copy isi dari folder `franky` yang telah didownload ke `/var/www/franky.c11.com`.

Setelah itu jalankan command `a2ensite franky.c11.com` dan `service apache2 restart`

# Soal 9

Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home. :

Pertama, jalankan perintah `a2enmod rewrite` kemudian `service apache2 restart`.

Kemudian pindah ke directory `/var/www/franky.c11.com` dan buat file `.htaccess` dengan isi file:

```bash
    RewriteEngine On
    RewriteRule ^home$ index.php/home
```

Kemudian buka file `/etc/apache2/sites-available/franky.c11.com.conf` dan tambahkan:

```bash
    <Directory /var/www/franky.c11.com>
        Options +FollowSymLinks -Multiviews
        AllowOverride All
    </Directory>
```

# Soal 10

Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com :

Pertama, pindah ke directory `/etc/apache2/sites-available`.Kemudian copy file `000-default.conf` menjadi file `super.franky.c11.com.conf`

Lalu setting file `super.franky.c11.com.conf` agar memiliki line `ServerName super.franky.c11.com`, `ServerAlias www.super.franky.c11.com`, dan `DocumentRoot /var/www/super.franky.c11.com`.

Kemudian bikin directory baru dengan nama `super.franky.c11.com` pada `/var/www/` menggunakan command `mkdir /var/www/super.franky.c11.com`. lalu copy isi dari folder `super.franky` yang telah didownload ke `/var/www/super.franky.c11.com`.

Setelah itu jalankan command `a2ensite super.franky.c11.com` dan `service apache2 restart`

# Soal 11

Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja. :

Pindah ke directory `/etc/apache2/sites-available` kemudian buka file `super.franky.c11.com` dan tambahkan:

```bash
    <Directory /var/www/super.franky.c11.com/public>
        Options +Indexes
    </Directory>
```
