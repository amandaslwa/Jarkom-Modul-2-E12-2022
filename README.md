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
