# Jarkom-Modul-2-E12-2022
Kelas Jaringan Komputer E - Kelompok E12
### Nama Anggota
- Azzura Mahendra Putra Malinus (5025201211) 
- Amanda Salwa Salsabila (5025201172) 
- Michael Ariel Manihuruk (5025201158) 

### Pembuatan Topologi
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
### Sebelum Revisi
### Nomor 1 sampai 6
Pada Ostania, connect ke jaringan dengan menggunakan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.198.0.0/16` <br/>
Pada tiap node, connect ke jaringan tersebut dengan menggunakan `echo nameserver 192.168.122.1 > /etc/resolv.conf`
<br/>
<br/>
Sebelum menjadikan WISE sebagai DNS Master/domain, perlu melakukan instalasi bind9 dengan cara `apt-get update` dan `apt-get install bind9 -y`. Kemudian, WISE dijadikan DNS Master/domain dan Berlint dijadikan DNS Slave <br/>
<img width="960" alt="named conf local2" src="https://user-images.githubusercontent.com/90702710/198830014-dac44a08-c305-4124-a0dd-85a6fc5595ed.png"><br/>
Lakukan `mkdir /etc/bind/jarkom-e12`  dan `cp /etc/bind/db.local /etc/bind/jarkom-e12/wise.e12.com` pada WISE dan buat konfigurasi domain pada `/etc/bind/jarkom-e12/wise.e12.com` menggunakan `nano` <br/>
<img width="960" alt="nano wise" src="https://user-images.githubusercontent.com/90702710/198830150-41fba31e-713f-491c-9863-f4360ae5447e.png"> <br/>
Tidak lupa untuk melakukan `service bind9 restart` setiap konfigurasi diupdate
<br/>
<br/>

Sebelum menjadikan Berlint sebagai DNS Slave, perlu juga melakukan instalasi bind9 dengan cara `apt-get update` dan `apt-get install bind9 -y` pada Berlint. <br/>
<img width="960" alt="berlint dns slave" src="https://user-images.githubusercontent.com/90702710/198830717-97eb08a1-7818-47a1-9363-ef256d5e6440.png">
<br/>
Tidak lupa untuk melakukan `service bind9 restart` setiap konfigurasi diupdate <br/>
<br/>
Untuk menjadikan SSS dan Garden sebagai client, perlu dilakukan `echo nameserver 192.168.122.1 > /etc/resolv.conf` dan install dnsutils dengan cara `apt-get update` dan `apt-get install dnsutils -y` dan melakukan koneksi kepada jaringan IP WISE dan IP Berlint
```
echo nameserver 192.198.3.2 > /etc/resolv.conf
echo nameserver 192.198.2.2 >> /etc/resolv.conf
```
