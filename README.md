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

  ![Screenshot 2021-11-13 203550](https://user-images.githubusercontent.com/62832487/141645874-5ade5746-787e-457c-8f2e-8b62920fff08.png)
  
- Edit file `/etc/dhcp/dhcpd.conf` seperti gambar berikut:

  ![Screenshot 2021-11-13 203841](https://user-images.githubusercontent.com/62832487/141645954-3e35bb88-fd8c-4898-b2fb-92403966a995.png)
  
- Restart isc-dhcp-server.
  ```
  service isc-dhcp-server restart
  ```

**Pada Loguetown**
- Edit file `/etc/network/interfaces` seperti gambar berikut:

  ![Screenshot 2021-11-13 204054](https://user-images.githubusercontent.com/62832487/141646017-414a63b2-df7b-46fa-98ce-e6796b5bed4e.png)
  
- Restart Loguetown dengan klik stop dan start pada node Loguetown.
- Lakukan testing pada IP dan nameserver.

  ![Screenshot 2021-11-13 204134](https://user-images.githubusercontent.com/62832487/141646046-c07db7da-da45-487e-a9f4-65246d06fc1c.png)

**Pada Alabasta**
- Edit file `/etc/network/interfaces` seperti gambar berikut:

![Screenshot 2021-11-13 204251](https://user-images.githubusercontent.com/62832487/141646092-da55da53-3e66-4171-8f29-719b89e7650b.png)
  
- Restart Alabasta dengan klik stop dan start pada node Alabasta.
- Lakukan testing pada IP dan nameserver.

![Screenshot 2021-11-13 204327](https://user-images.githubusercontent.com/62832487/141646110-63b62199-3bf8-4e0b-8828-cb1c84fcf98c.png)

### Soal No. 4
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50.

**Penyelesaian**
**Pada Jipangu**
- Edit file `/etc/dhcp/dhcpd.conf` seperti gambar berikut:

  ![Screenshot 2021-11-13 204503](https://user-images.githubusercontent.com/62832487/141646152-784e6f22-f16d-4a04-92eb-aa0ea39e685b.png)

- Restart isc-dhcp-server.
  ```
  service isc-dhcp-server restart
  ```

**Pada TottoLand**
- Edit file `/etc/network/interfaces` seperti gambar berikut:

![Screenshot 2021-11-13 204601](https://user-images.githubusercontent.com/62832487/141646182-df73e2f1-e8a7-47d4-b646-ff6b74ed2e45.png)
  
- Restart TottoLand dengan klik stop dan start pada node TottoLand.
- Lakukan testing pada IP dan nameserver.

  ![Screenshot 2021-11-13 204711](https://user-images.githubusercontent.com/62832487/141646219-b3f1ea54-2149-487a-b16e-b30a419d702d.png)

### Soal No. 5
Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut.

**Penyelesaian**
**Pada EniesLobby**
- Install aplikasi bind9.
  ```
  apt-get install bind9 -y
  ```
- Edit file `/etc/bind/named.conf.options` seperti pada gambar berikut:

  ![Screenshot 2021-11-13 204920](https://user-images.githubusercontent.com/62832487/141646288-7fd305b2-32f3-4764-ad00-ce4b7642ce35.png)

- Restart bind9.
  ```
  service bind9 restart
  ```

**Pada Loguetown**
- Mencoba ping `google.com`.

  ![4](https://user-images.githubusercontent.com/62832487/141646316-4cfeb685-1b68-49cc-98ec-14f428735526.png)

**Pada Alabasta**
- Mencoba ping `google.com`.

![Screenshot 2021-11-13 205047](https://user-images.githubusercontent.com/62832487/141646334-f9eb8801-49fa-4b57-9b83-00aa066462ec.png)

**Pada Skypie**
- Mencoba ping `google.com`.

![Screenshot 2021-11-13 205157](https://user-images.githubusercontent.com/62832487/141646363-aa0b3b47-af52-4999-8f48-35258c7918d0.png)  

**Pada TottoLand**
- Mencoba ping `google.com`.

![Screenshot 2021-11-13 205244](https://user-images.githubusercontent.com/62832487/141646379-5be0203f-84c7-4c4e-919b-6936fb49522a.png)


### Soal No. 6
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

**Penyelesaian**
**Pada Jipangu**
- Edit file `/etc/dhcp/dhcpd.conf` pada bagian `default-lease-time` dan `max-lease-time` seperti pada gambar berikut:

![Screenshot 2021-11-13 205415](https://user-images.githubusercontent.com/62832487/141646415-28294bf4-05b7-4f67-87c7-d47c4e1db1ac.png)
  
- Restart isc-dhcp-server.
  ```
  service isc-dhcp-server restart
  ```

### Soal No. 7
Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69.

**Penyelesaian**
**Pada Jipangu**
- Edit file `/etc/dhcp/dhcpd.conf` seperti pada gambar berikut:
  
![Screenshot 2021-11-13 205559](https://user-images.githubusercontent.com/62832487/141646481-053ef469-755e-486e-822c-d6a8e6f54034.png)

- Restart isc-dhcp-server.
  ```
  service isc-dhcp-server restart
  ```

**Pada Skypie**
- Edit file `/etc/network/interfaces` seperti pada gambar berikut:

![Screenshot 2021-11-13 205657](https://user-images.githubusercontent.com/62832487/141646519-f334050f-b206-476d-884c-2f7b1281f3c0.png)

- Restart Skypie dengan klik stop dan start pada node Skypie.
- Lakukan testing pada IP dan nameserver.

![Screenshot 2021-11-13 205740](https://user-images.githubusercontent.com/62832487/141646539-20923c06-fcc0-498b-895e-7b564a4e075b.png)

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

![Screenshot 2021-11-13 205911](https://user-images.githubusercontent.com/62832487/141646588-a9cb2a71-ab53-41d4-9e67-6f181c8a57be.png)

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

  ![Screenshot 2021-11-13 210230](https://user-images.githubusercontent.com/62832487/141646694-69ea26c1-e7d0-4e15-979d-b69c541f67d5.png)

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

![Screenshot 2021-11-13 210344](https://user-images.githubusercontent.com/62832487/141646726-f4c7957f-4f38-4136-8ddc-506d1c1b7a88.png)

- Restart squid.
  ```
  service squid restart
  ```

**Pada Loguetown**
- Buka `http://its.ac.id` menggunakan lynx.

  ![Screenshot 2021-11-13 210436](https://user-images.githubusercontent.com/62832487/141646762-884ce060-99b9-48fa-bfa5-2409b47fa6a0.png)

### Soal No. 10
Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00).

**Penyelesaian**
- Pada etc/squid/acl.conf, kita masukkan yang di bawah. Artinya adalah pada hari senin, selasa, rabu, dan kamis, hanya bisa jam 7 sampai 11, dan selanjutnya. Setiap huruf artinya satu hari tertentu pada sebuah minggu.

- Ubah /etc/squid/squid.conf untuk include acl.conf dan meng-allow sesuai dengan acl.conf.

![Screenshot 2021-11-13 210436](https://user-images.githubusercontent.com/62832487/141646762-884ce060-99b9-48fa-bfa5-2409b47fa6a0.png)

- Jika dibuka saat waktu yang ditentukan di acl.conf, maka akan keluar website yang diinginkan:

![image](https://user-images.githubusercontent.com/68369091/141647486-1520b980-a953-4101-87a0-bd19ced168dd.png)

- Jika dibuka saat waktu yang di luar di acl.conf, maka akan ditidak-perbolehkan untuk membuka website:

![image](https://user-images.githubusercontent.com/68369091/141647505-7ef37a6b-794f-4398-9ff0-bf998778c218.png)

### Kendala yang Dialami
- Tidak bisa me lynx super.franky
