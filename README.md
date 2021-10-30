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
