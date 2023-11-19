# 🖥️🖥️ Jarkom-Modul-3-D08-2023 🖥️🖥️

Nama Anggota | NRP
------------------- | --------------		
Timothy Hosia Budianto | 5025211098
Arif Nugraha Santosa | 5025211048

## 🟩🟩 LINK PROJECT 🟩🟩 
Link: 

## 🟩🟩 IP ADDRESS KELOMPOK D08 🟩🟩 
Node Aura (Dynamic)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.195.1.195
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.195.2.195
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.195.3.195
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.195.4.195
	netmask 255.255.255.0
```
Node Sein (Dynamic)
```
auto eth0
iface eth0 inet dhcp
```
Node Stark (Dynamic)
```
auto eth0
iface eth0 inet dhcp
```
Node Frieren (Fixed DHCP)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 62:85:dc:12:6a:e1 //hwaddress eth0
```

Node Flamme (Fixed DHCP)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 0a:c9:e8:93:9a:99 //hwaddress eth0

```
Node Fern (Fixed DHCP)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 56:68:5d:5c:05:38 //hwaddress eth0
```
Node Himmel (Static)
```
# Static config for eth0
auto eth0
iface eth0 inet static	
address 192.195.1.1
netmask 255.255.255.0
gateway 192.195.1.195
```
Node Heiter (Static)
```
# Static config for eth0
auto eth0
iface eth0 inet static	
address 192.195.1.2
netmask 255.255.255.0
gateway 192.195.1.195
```
Node Denken (Fixed DHCP)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether d2:5b:49:77:c6:53
```
Node Eisen (Fixed DHCP)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 2e:14:fa:49:d4:26
```
Node Revolte (Dynamic)
```
auto eth0
iface eth0 inet dhcp
```
Node Richter (Dynamic)
```
auto eth0
iface eth0 inet dhcp
```
Node Lawine (Fixed DHCP)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 26:5f:26:7d:8f:93
```
Node Linie (Fixed DHCP)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether a2:22:e8:fa:6f:3d
```
Node Lugner (Fixed DHCP)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 26:88:d2:3f:d6:34
```

## 🟩🟩 PENYELESAIAN 🟩🟩

### ⭕ Nomor 1
### 🟢 Jawaban Nomor 1
### 1️⃣ Membuat Topologi
- susun topologi seperti gambar dibawah
![image](https://github.com/thossb/Jarkom-Modul-3-D08-2023/assets/90438426/07f3e133-e19f-4440-b047-b5bb7854ac5e)
- set iptables iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.195.0.0/16 pada aura (Router) dan simpan di root/.bashrc
- lakukan echo nameserver 192.168.122.1 > /etc/resolv.conf pada node himmel (DHCP Server) dan heiter (DNS server)
- sedangkan node lain lakukan nameserver ke Heiter (DNS Server)

### ⭕ Nomor 2 - 5
- melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.
- Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 (2)
- Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 (3)
- Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut (4)
- Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit (5)
- set fixed address untuk node static (*)

### 🟢 Jawaban Nomor 2 - 5
- set up dhcp server
- pada Himmel lakukan `apt-get update`
- Install isc-dhcp-server di Westalis. `apt-get install isc-dhcp-server -y`
- Pastikan isc-dhcp-server telah ter-install dengan perintah.`dhcpd --version`
#### Konfigurasi DHCP Server
- Buka File Konfigurasi Interface Silakan edit file konfigurasi `isc-dhcp-server` pada `/etc/default/isc-dhcp-server.`
- Dan isi INTERFACESv4nya dengan interface yang ingin diberi dhcp.
![image](https://github.com/thossb/Jarkom-Modul-3-D08-2023/assets/90438426/8b00dbfa-c752-4e28-a1dc-cb1f2e1b4985)
- Edit file konfigurasi `isc-dhcp-server` pada `nano /etc/dhcp/dhcpd.conf`
- Tambahkan script Konfigurasi
```
# Switch 1
subnet 192.195.1.0 netmask 255.255.255.0 {
}

# Switch 2
subnet 192.195.2.0 netmask 255.255.255.0 {
    option routers 192.195.2.195;
    option broadcast-address 192.195.2.255;
    option domain-name-servers 192.195.1.2;
    default-lease-time 7200;
    max-lease-time 7200;
}

#Client Switch 3
subnet 192.195.3.0 netmask 255.255.255.0 {
    range 192.195.3.16 192.195.3.32;
    range 192.195.3.64 192.195.3.80;
    option routers 192.195.3.195;
    option broadcast-address 192.195.3.255;
    option domain-name-servers 192.195.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

#Client Switch 4
subnet 192.195.4.0 netmask 255.255.255.0 {
    range 192.195.4.12 192.195.4.20;
    range 192.195.4.160 192.195.4.168;
    option routers 192.195.4.195;
    option broadcast-address 192.195.4.255;
    option domain-name-servers 192.195.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}

#Database Server
host Denken {
    hardware ethernet d2:5b:49:77:c6:53;
    fixed-address 192.195.2.1;
}

#Load Balancer
host Eisen {
    hardware ethernet 2e:14:fa:49:d4:26;
    fixed-address 192.195.2.2;
}

#Laravel Workers
host Frieren {
    hardware ethernet 62:85:dc:12:6a:e1;
    fixed-address 192.195.4.1;
}

host Flamme {
    hardware ethernet 0a:c9:e8:93:9a:99;
    fixed-address 192.195.4.2;
}

host Fern {
    hardware ethernet 56:68:5d:5c:05:38;
    fixed-address 192.195.4.3;
}

#PHP Workers
host Lawine {
    hardware ethernet 26:5f:26:7d:8f:93;
    fixed-address 192.195.3.1;
}

host Linie {
    hardware ethernet a2:22:e8:fa:6f:3d;
    fixed-address 192.195.3.2;
}

host Lugner {
    hardware ethernet 26:88:d2:3f:d6:34;
    fixed-address 192.195.3.3;
}
```
- Restart Service `isc-dhcp-server` Dengan Perintah `service isc-dhcp-server restart`

#### Konfigurasi DHCP Relay
- lakukan installasi
```
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start
```
- konfigurasi pada `isc-dhcp-relay`
- Pada /etc/default/isc-dhcp-relay lakukan konfigurasi berikut.
```
SERVERS="[IP Address dari DHCP Server]"  
INTERFACES="eth1 eth3 eth 4" // eth dhcp server, client, client
OPTIONS=
```
- konfigurasi IP Forwarding
- Pada /etc/sysctl.conf. `net.ipv4.ip_forward=1`
- Konfigurasi tersebut digunakan untuk mengaktifkan IP Forwarding. Kemudian, `restart service isc-dhcp-relay`.
- Test dengan cara : buka salah satu client, hasilnya
- ![image](https://github.com/thossb/Jarkom-Modul-3-D08-2023/assets/90438426/1e962afe-05d6-489e-84b2-f62942c97752)

#### Konfigurasi DNS Server
- instalasi bind dengan command
```
apt-get update
apt-get install bind9 -y
```
- edit `nano /etc/bind/named.conf.local`
- buat domain baru
```
zone "riegel.canyon.d08.com" { 
 type 
 master ; 
 file " /etc/bind/jarkom/riegel.canyon.d08.com";
};
 
 zone "granz.channel.d08.com" { 
 type master; 
 file "/etc/bind/jarkom/granz.channel.d08.com";
};
```
- buat folder untuk menyimpan konfigurasi `mkdir /etc/bind/jarkom`
- cp dari db.local
```
cp /etc/bind/db.local /etc/bind/jarkom/riegel.canyon.d08.com
cp /etc/bind/db.local /etc/bind/jarkom/granz.channel.d08.com
```
- edit konfigurasi bind dengan `nano /etc/bind/jarkom/riegel.canyon.d08.com` `nano /etc/bind/jarkom/granz.channel.d08.com` arahkan pada worker laravel yang memiliki  IP [prefix IP].x.1.

### ⭕ Nomor 6
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)
### 🟢 Jawaban Nomor 6
### 6️⃣
- Sebelum mengerjakan perlu untuk melakukan setup terlebih dahulu pada seluruh PHP Worker. Jika sudah, silahkan untuk melakukan konfigurasi tambahan sebagai berikut untuk melakukan download dan unzip menggunakan command wget
```
wget -O '/var/www/granz.channel.d08.com' 'https://drive.google.com/u/0/uc?id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1&export=download'
unzip -o /var/www/granz.channel.d08.com -d /var/www/
rm /var/www/granz.channel.d08.com
mv /var/www/modul-3 /var/www/granz.channel.d08.com
```
- Setelah melakukan download dan unzip. Sekarang kita bisa melakukan konfigurasi pada nginx sebagai berikut:
```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/granz.channel.d08.com
ln -s /etc/nginx/sites-available/granz.channel.d08.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/granz.channel.d08.com;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;  # Sesuaikan versi PHP dan socket
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/granz.channel.d08.com

service nginx restart
```
- hasil
- ![image](https://github.com/thossb/Jarkom-Modul-3-D08-2023/assets/90438426/bbb12468-a7dd-45fa-9c79-5e5eb8734e05)


### ⭕ Nomor 7
Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
Lawine, 4GB, 2vCPU, dan 80 GB SSD.
Linie, 2GB, 2vCPU, dan 50 GB SSD.
Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)
### 🟢 Jawaban Nomor 7
### 7️⃣ 

### ⭕ Nomor 8
Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
A. Nama Algoritma Load Balancer
B. Report hasil testing pada Apache Benchmark
C. Grafik request per second untuk masing masing algoritma. 
D. Analisis (8)
### 🟢 Jawaban Nomor 8
### 8️⃣ 

### ⭕ Nomor 9
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. (9)
### 🟢 Jawaban Nomor 9
### 9️⃣ 

### ⭕ Nomor 10
Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/ (10)
### 🟢 Jawaban Nomor 10
### 🔟

### ⭕ Nomor 11
Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. (11) hint: (proxy_pass)
### 🟢 Jawaban Nomor 11
### 1️⃣1️⃣ 

### ⭕ Nomor 12
Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168. (12)
### 🟢 Jawaban Nomor 12
### 1️⃣2️⃣ 

### ⭕ Nomor 13
Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern. (13)
### 🟢 Jawaban Nomor 13
### 1️⃣3️⃣ 

### ⭕ Nomor 14
Frieren, Flamme, dan Fern memiliki Granz Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer (14)
### 🟢 Jawaban Nomor 14
### 1️⃣4️⃣ 

### ⭕ Nomor 15
Granz Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
POST /auth/register (15)
### 🟢 Jawaban Nomor 15
### 1️⃣5️⃣ 

### ⭕ Nomor 16
POST /auth/login (16)
### 🟢 Jawaban Nomor 16
### 1️⃣6️⃣ 

### ⭕ Nomor 17
GET /me (17)
### 🟢 Jawaban Nomor 17
### 1️⃣7️⃣ 

### ⭕ Nomor 18
Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Granz Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern. (18)
### 🟢 Jawaban Nomor 18
### 1️⃣8️⃣ 

### ⭕ Nomor 19
Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.(19)
### 🟢 Jawaban Nomor 19
### 1️⃣9️⃣ 

### ⭕ Nomor 20
Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second. (20)
### 🟢 Jawaban Nomor 20
### 2️⃣0️⃣ 

### Backup
DHCP Server
```
mkdir /root/etc

cp -rf /etc/default /root/etc
cp -rf /etc/dhcp /root/etc
```
DHCP Relay
```
mkdir /root/etc

cp -rf /etc/default /root/etc
cp -r /etc/sysctl.conf /root/etc
```
DNS Server
```
mkdir /root/etc

cp -rf /etc/bind /root/etc
```

