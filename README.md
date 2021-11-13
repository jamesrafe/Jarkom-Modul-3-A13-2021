# Jarkom-Modul-3-A13-2021

|Nama|NRP|
|----|-----|
|Afifah Nur Sabrina Syamsudin|05111940000022|
|James Rafferty Lee|05111940000055|
|Zulfiqar Fauzul Akbar|05111940000101|

### Soal No. 1
Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, dan Water7 sebagai Proxy Server.

**Penyelesaian**
- Buat topologi seperti soal

![topologi_modul3](https://user-images.githubusercontent.com/75364000/141644547-939563e4-6b3a-445d-947d-c3d2047e9a89.PNG)
- Setting network masing-masing node dengan fitur `Edit network configuration`:
**Foosha (sebagai Router / DHCP Relay)**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.175.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.175.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.175.3.1
	netmask 255.255.255.0
```

**Loguetown (sebagai Client)**
```
auto eth0
iface eth0 inet dhcp
auto eth0
iface eth0 inet static
	address 192.175.1.2
	netmask 255.255.255.0
	gateway 192.175.1.1
```

**Alabasta (sebagai Client)**
```
auto eth0
iface eth0 inet static
	address 192.175.1.3
	netmask 255.255.255.0
	gateway 192.175.1.1
```

**EniesLobby (sebagai DNS Master)**
```
auto eth0
iface eth0 inet static
	address 192.175.2.2
	netmask 255.255.255.0
	gateway 192.175.2.1
```

**Water7 (sebagai Proxy Server)**
```
auto eth0
iface eth0 inet static
	address 192.175.2.3
	netmask 255.255.255.0
	gateway 192.175.2.1
```

**Jipangu (sebagai DHCP Server)**
```
auto eth0
iface eth0 inet static
	address 192.175.2.4
	netmask 255.255.255.0
	gateway 192.175.2.1
```

**Skypie (sebagai Client)**
```
auto eth0
iface eth0 inet static
	address 192.175.3.2
	netmask 255.255.255.0
	gateway 192.175.3.1
```

**TottoLand (sebagai Client)**
```
auto eth0
iface eth0 inet static
	address 192.175.3.3
	netmask 255.255.255.0
	gateway 192.175.3.1
```

Ketikkan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.175.0.0/16` pada router `Foosha`.

Ketikkan `echo nameserver 192.168.122.1 > /etc/resolv.conf` pada node ubuntu yang lain.

Restart semua node dan coba `ping google.com`.

[Screenshot]

### Soal No. 2
Foosha sebagai DHCP Relay.

**Penyelesaian**
**Pada Foosha**
- Install aplikasi isc-dhcp-relay.
  ```
  apt-get install isc-dhcp-relay -y
  ```
- Edit file `/etc/default/isc-dhcp-relay` seperti gambar berikut:

  [Screenshot]
- Restart isc-dhcp-relay.
  ```
  service isc-dhcp-relay restart
  ```

### Soal No. 3
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169.

**Penyelesaian**
**Pada Jipangu**
- Install aplikasi isc-dhcp-server.
  ```
  apt-get install isc-dhcp-server -y
  ```
  
- Edit file `/etc/default/isc-dhcp-server` seperti gambar berikut:

  [Screenshot]
  
- Edit file `/etc/dhcp/dhcpd.conf` seperti gambar berikut:

  [Screenshot]
- Restart isc-dhcp-server.
  ```
  service isc-dhcp-server restart
  ```

**Pada Loguetown**
- Edit file `/etc/network/interfaces` seperti gambar berikut:

  [Screenshot]
- Restart Loguetown dengan klik stop dan start pada node Loguetown.
- Lakukan testing pada IP dan nameserver.

  [Screenshot]

**Pada Alabasta**
- Edit file `/etc/network/interfaces` seperti gambar berikut:

  [Screenshot]
- Restart Alabasta dengan klik stop dan start pada node Alabasta.
- Lakukan testing pada IP dan nameserver.

  [Screenshot]


### Soal No. 4
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50.

**Penyelesaian**
**Pada Jipangu**
- Edit file `/etc/dhcp/dhcpd.conf` seperti gambar berikut:

  [Screenshot]
- Restart isc-dhcp-server.
  ```
  service isc-dhcp-server restart
  ```

**Pada TottoLand**
- Edit file `/etc/network/interfaces` seperti gambar berikut:

  [Screenshot]
- Restart TottoLand dengan klik stop dan start pada node TottoLand.
- Lakukan testing pada IP dan nameserver.

  [Screenshot]

### Soal No. 5
Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut.

**Penyelesaian**
**Pada EniesLobby**
- Install aplikasi bind9.
  ```
  apt-get install bind9 -y
  ```
- Edit file `/etc/bind/named.conf.options` seperti pada gambar berikut:

  [Screenshot]
- Restart bind9.
  ```
  service bind9 restart
  ```

**Pada Loguetown**
- Mencoba ping `google.com`.

  [Screenshot]

**Pada Alabasta**
- Mencoba ping `google.com`.

  [Screenshot]

**Pada Skypie**
- Mencoba ping `google.com`.

  [Screenshot]

**Pada TottoLand**
- Mencoba ping `google.com`.

  [Screenshot]


### Soal No. 6
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

**Penyelesaian**
**Pada Jipangu**
- Edit file `/etc/dhcp/dhcpd.conf` pada bagian `default-lease-time` dan `max-lease-time` seperti pada gambar berikut:

  [Screenshot]
  
  [Screenshot]
- Restart isc-dhcp-server.
  ```
  service isc-dhcp-server restart
  ```

### Soal No. 7
Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69.

**Penyelesaian**
**Pada Jipangu**
- Edit file `/etc/dhcp/dhcpd.conf` seperti pada gambar berikut:
  
  [Screenshot]
- Restart isc-dhcp-server.
  ```
  service isc-dhcp-server restart
  ```

**Pada Skypie**
- Edit file `/etc/network/interfaces` seperti pada gambar berikut:

  [Screenshot]
- Restart Skypie dengan klik stop dan start pada node Skypie.
- Lakukan testing pada IP dan nameserver.

  [Screenshot]

### Soal No. 8
Pada Loguetown, proxy harus bisa diakses dengan nama `jualbelikapal.yyy.com` dengan port yang digunakan adalah 5000.

**Penyelesaian**
**Pada Water7**
- Install aplikasi squid dan apache2-utils.
  ```
  apt-get install squid -y
  apt-get install apache2-utils -y
  ```
- Lakukan backup terlebih dahulu file konfigurasi default yang disediakan Squid.
  ```
  mv /etc/squid/squid.conf /etc/squid/squid.conf.bak
  ```
- Buat konfigurasi Squid baru.
  ```
  nano /etc/squid/squid.conf
  ```
- Edit file `/etc/squid/squid.conf` seperti gambar berikut:

  [Screenshot]
- Tambahkan IP EniesLobby (192.175.2.2) pada file `/etc/resolv.conf`.
- Restart squid.
  ```
  service squid restart
  ```

**Pada Loguetown**
- Install aplikasi lynx.
  ```
  apt-get install lynx -y
  ```
- Aktifkan proxy dengan syntax berikut:
  ```
  export http_proxy="http://192.175.2.3:5000"
  ```
- Buka `http://its.ac.id` menggunakan lynx.

  [Screenshot]

### Soal No. 9
Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu `luffybelikapalyyy` dengan password `luffy_yyy` dan `zorobelikapalyyy` dengan password `zoro_yyy`.

**Penyelesaian**
**Pada Water7**
- Jalankan perintah berikut untuk membuat akun autentikasi baru dengan username `luffybelikapalB09`. Kita akan diminta untuk memasukkan password baru dan confirm password tersebut diisi `luffy_A13`.
  ```
  htpasswd -cm /etc/squid/passwd luffybelikapalA13
  ```
- Jalankan perintah berikut untuk membuat akun autentikasi baru dengan username `zorobelikapalA13`. Kita akan diminta untuk memasukkan password baru dan confirm password tersebut diisi `zoro_A13`.
  ```
  htpasswd -m /etc/squid/passwd zorobelikapalA13
  ```
- Edit file `/etc/squid/squid.conf` seperti pada gambar berikut:

  [Screenshot]
- Restart squid.
  ```
  service squid restart
  ```

**Pada Loguetown**
- Buka `http://its.ac.id` menggunakan lynx.

  [Screenshot]

### Soal No. 10
Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00).

**Penyelesaian**


### Kendala yang Dialami
- Tidak bisa me lynx super.franky
