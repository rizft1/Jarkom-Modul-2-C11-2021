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
Membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku.
