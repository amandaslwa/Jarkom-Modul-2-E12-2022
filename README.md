# Jarkom-Modul-2-E12-2022
Kelas Jaringan Komputer E - Kelompok E12
### Nama Anggota dan Pembagian Kerja
- Azzura Mahendra Putra Malinus (5025201211) 
- Amanda Salwa Salsabila (5025201172) 
- Michael Ariel Manihuruk (5025201158) 

Pembuatan Topologi <br/>
<img width="960" alt="image" src="https://user-images.githubusercontent.com/90702710/198829584-e18fa38e-cc6d-4376-b4e4-19aca74ff66c.png">
<br/>
### Konfigurasi Ostania 
```
#nat
auto eth0
iface eth0 inet dhcp

#switch1
auto eth1
iface eth1 inet static
	address 192.198.1.1
	netmask 255.255.255.0

#switch2
auto eth3
iface eth3 inet static
	address 192.198.2.1
	netmask 255.255.255.0
  
#wise
auto eth2
iface eth2 inet static
	address 192.198.3.1
	netmask 255.255.255.0
```
### Konfigurasi WISE 
```
auto eth0
iface eth0 inet static
	address 192.198.3.2
	netmask 255.255.255.0
      gateway 192.198.3.1
```
### Konfigurasi SSS
```
auto eth0
iface eth0 inet static
	address 192.198.1.2
	netmask 255.255.255.0
	gateway 192.198.1.1
```
### Konfigurasi Garden
```
auto eth0
iface eth0 inet static
	address 192.198.1.3
	netmask 255.255.255.0
	gateway 192.198.1.1
```
### Konfigurasi Berlint
```
auto eth0
iface eth0 inet static
	address 192.198.2.2
	netmask 255.255.255.0
	gateway 192.198.2.1
```
### Konfigurasi Eden
```
auto eth0
iface eth0 inet static
	address 192.198.2.3
	netmask 255.255.255.0
	gateway 192.198.2.1
```
## Sebelum Revisi
### Nomor 1 sampai 6
<br/> 
<br/> 
Pada Ostania, connect ke jaringan dengan menggunakan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.198.0.0/16` <br/>
Pada tiap node, connect ke jaringan tersebut dengan menggunakan `echo nameserver 192.168.122.1 > /etc/resolv.conf`
<br/>
Sebelum menjadikan WISE sebagai DNS Master/domain, perlu melakukan instalasi bind9 dengan cara `apt-get update` dan `apt-get install bind9 -y`. Kemudian, WISE dijadikan DNS Master/domain dan Berlint dijadikan DNS Slave <br/>
<img width="960" alt="named conf local2" src="https://user-images.githubusercontent.com/90702710/198830014-dac44a08-c305-4124-a0dd-85a6fc5595ed.png"><br/>
Lakukan `mkdir /etc/bind/jarkom-e12`  dan `cp /etc/bind/db.local /etc/bind/jarkom-e12/wise.e12.com` pada WISE dan buat konfigurasi domain pada `/etc/bind/jarkom-e12/wise.e12.com` menggunakan `nano` <br/>
<img width="960" alt="nano wise" src="https://user-images.githubusercontent.com/90702710/198830150-41fba31e-713f-491c-9863-f4360ae5447e.png"> <br/>
Tidak lupa untuk melakukan `service bind9 restart` setiap konfigurasi diupdate
<br/>
Sebelum menjadikan Berlint sebagai DNS Slave, perlu juga melakukan instalasi bind9 dengan cara `apt-get update` dan `apt-get install bind9 -y` pada Berlint. <br/>
<img width="960" alt="berlint dns slave" src="https://user-images.githubusercontent.com/90702710/198830717-97eb08a1-7818-47a1-9363-ef256d5e6440.png">
<br/>
Tidak lupa untuk melakukan `service bind9 restart` setiap konfigurasi diupdate <br/>

Untuk menjadikan SSS dan Garden sebagai client, perlu dilakukan `echo nameserver 192.168.122.1 > /etc/resolv.conf` dan install dnsutils dengan cara `apt-get update` dan `apt-get install dnsutils -y` dan melakukan koneksi kepada jaringan IP WISE dan IP Berlint
```
echo nameserver 192.198.3.2 > /etc/resolv.conf
echo nameserver 192.198.2.2 >> /etc/resolv.conf
```
Sekarang kita dapat melakukan ping ke domain yang telah kita buat <br/>
<img width="960" alt="ping wise di sss" src="https://user-images.githubusercontent.com/90702710/198831233-d2ae2b1a-b850-4d12-8a58-977b76c6c8ca.png">
<img width="960" alt="ping www wise di garden" src="https://user-images.githubusercontent.com/90702710/198831265-fc6528d6-75ab-4dda-9a18-ef8b5cee83a7.png">
<img width="960" alt="ping eden wise di sss" src="https://user-images.githubusercontent.com/90702710/198831278-2f2645c1-0b37-4a5d-9150-61bd1148cd8f.png">
<img width="960" alt="ping www eden wise di sss" src="https://user-images.githubusercontent.com/90702710/198831287-ca867e2e-9765-405e-af19-c5b0911a9f4e.png">


Setelah itu, untuk membuat reverse domain untuk domain utama, perlu mengupdate file `/etc/bind/named.conf.local` dan mengcopy nya ke konfigurasi reverse domain dengan cara `cp /etc/bind/db.local /etc/bind/jarkom-e12/3.198.192.in-addr.arpa` <br/>
<img width="960" alt="reverse dns di named" src="https://user-images.githubusercontent.com/90702710/198831708-ccdeb1d7-420d-4a76-890d-b2d35a32921b.png"> <br/>
<img width="960" alt="domain reverse ptr" src="https://user-images.githubusercontent.com/90702710/198831756-fa6ea1e5-680d-4703-a521-66045a76b3f9.png"> <br/>
Tidak lupa untuk melakukan `service bind9 restart` setiap konfigurasi diupdate <br/>
