# Laporan Resmi Praktikum Komunikasi Data dan Jaringan Komputer Modul 2  Kelompok T14

## Anggota Kelompok:

1. Hisyam Zulkarnain F – 05311840000019
2. Muhamad Rifaldi - 05311840000022



## Soal No 1
### membuat sebuah website utama dengan alamat http://semerut14.pw yang memiliki
- Buka MALANG dan update package lists dengan menjalankan command: ```apt-get update```
- Install aplikasi bind9 pada MALANG ```apt-get install bind9 -y```
- Pada MALANG lakukan perintah ```nano /etc/bind/named.conf.local```
- Konfigurasi domain semerut14.pw berisi:
```
zone "semerut14.pw" {
	type master;
	file "/etc/bind/jarkom/semerut14.pw";
};
```
- Buat folder ```jarkom``` pada direktori ```/etc/bind``` : ```mkdir /etc/bind/jarkom```
- Salin file ```db.local``` pada ```/etc/bind``` ke dalam  folder jarkom dengan perintah: ```cp /etc/bind/db.local /etc/bind/jarkom/semerut14.pw```
- Buka dan edit file semerut14.pw dengan perintah ```nano /etc/bind/jarkom/semerut14.pw```
    ![img](/img/1a.png)
    
- Kemudian restart bind9 dengan perintah ```service bind9 restart```
- Pada client GRESIK dan SIDOARJO arahkan nameserver menuju IP MALANG dengan mengedit file resolve.conf dengan perintah ```nano /etc/resolv.conf```
```
nameserver 10.151.77.162     #IP MALANG
```
   ![img](/img/1b.png)
   ![img](/img/1c.png)
    
- Untuk mencoba koneksi DNS, lakukan ping domain semerut14.pw dengan melakukan perintah berikut pada client GRESIK dan SIDOARJO ```ping semerut14.pw```

   ![img](/img/1d.png)
   ![img](/img/1e.png)

</br></br></br>

## Soal No 2
### alias http://www.semerut14.pw
- Menambahkan konfigurasi pada server MALANG di dalam dile semerut14.pw ```nano /etc/bind/jarkom/semerut14.pw```
```
www	IN	CNAME	semerut14.pw.
```
![img](/img/2a.png)
	
- Kemudian restart bind9 dengan perintah ```service bind9 restart```
- Lalu cek di  client GRESIK ```ping www.semerut14.pw```
![img](/img/2b.png)
</br></br></br>

## Soal No 3
### subdomain http://penanjakan.semerut14.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO serta dibuatkan
- edit file semeru.pw pada MALANG ```nano /etc/bind/jarkom/semerut14.pw```
```
@	IN	A	10.151.77.164	; IP PROBOLINGGO
www	IN	CNAME	semerut14.pw.
penanajakan	IN	A	10.151.77.164	; IP PROBOLINGGO
```
![img](/img/3a.png)

- edit ```nano /etc/bind/named.conf.local```

- Kemudian restart bind9 dengan perintah ```service bind9 restart```
- Kemudian pada client GRESIK lakukan testing ```ping penanjakan.semerut14.pw```
![img](/img/3b.png)
</br></br></br>

## Soal No 4
### reverse domain untuk domain utama. Untuk mengantisipasi server dicuri/rusak, Bibah minta dibuatkan
- Edit file ```nano /etc/bind/named.conf.local``` pada MALANG
```
zone "77.151.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/77.151.10.in-addr.arpa";
};
```
![img](/img/4a.png)


- copykan file db.local ke dalam file 77.151.10.in-addr.arpa pada folder jarkom dengan perintah ```cp /etc/bind/db.local /etc/bind/jarkom/77.151.10.in-addr.arpa```
- ```77.151.10``` merupakan 3 byte pertama IP MALANG yang di-reverse urutannya
- Kemudian edit file dengan ```nano /etc/bind/jarkom/77.151.10.in-addr.arpa```
![img](/img/4b.png)

- Kemudian restart bind9 dengan perintah ```service bind9 restart```
- Untuk mengecek konfigurasi dapat melakukan perintah ```host -t PTR 10.151.77.106``` pada client GRESIK
![img](/img/4c.png)
</br></br></br>

## Soal No 5
### DNS Server Slave pada MOJOKERTO agar Bibah tidak terganggu menikmati keindahan Semeru pada Website. Selain website utama Bibah juga meminta dibuatkan 
- Pada MALANG edit ```nano /etc/bind/named.conf.local```
```
zone "semerut14.pw" {
    type master;
    notify yes;
    also-notify { 10.151.77.163; }; // IP MOJOKERTO
    allow-transfer { 10.151.77.163; }; // MOJOKERTO
    file "/etc/bind/jarkom/semerut14.pw";
};
```
![img](/img/5a.png)

- Kemudian restart bind9 dengan perintah ```service bind9 restart```
- Buka MOJOKERTO dan update package lists dengan menjalankan command: ```apt-get update```
- Kemudian install aplikasi bind9 pada MOJOKERTO ```apt-get install bind9 -y```
- Edit file pada MOJOKERTO ```nano /etc/bind/named.conf.local```
![img](/img/5b.png)

- Kemudian restart bind9 pada MOJOKERTO dengan perintah ```service bind9 restart```
- Pada sever MALANG matikan service bind9 ```service bind9 stop```
- Pada client GRESIK atur nameserver mengarah ke IP MALANG dan MOJOKERTO ```nano /etc/resolv.conf```
![img](/img/5c.png)

- Lakukan ```ping semerut14.pw``` pada client GRESIK
![img](/img/5d.png)
</br></br></br>

## Soal No 6
### subdomain dengan alamat http://gunung.semerut14.pw yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO. Bibah juga ingin memberi petunjuk mendaki gunung semeru kepada anggota komunitas sehingga dia meminta dibuatkan 
#### MALANG
- Edit file ```nano /etc/bind/jarkom/semerut14.pw```
- Tambahkan konfigurasi
```
gunung	IN	A	10.151.77.164	; IP PROBOLINGGO
naik	IN	NS	gunung
```
![img](/img/6a.png)

- Edit ```nano /etc/bind/named.conf.options```
![img](/img/6b.png)

- Comment kan ```dnimgec-validation auto;``` menjadi ```//dnimgec-validation auto;```
- Tambahkan ```allow-query{any;};```

- Edit ```nano /etc/bind/named.conf.local```
```
zone "semerut14.pw" {
    type master;
    notify yes;
    also-notify { 10.151.77.163; }; // IP MOJOKERTO
    allow-transfer { 10.151.77.163; }; // MOJOKERTO
    file "/etc/bind/jarkom/semerut14.pw";
};
```
![img](/img/6c.png)
- Kemudian restart bind9 dengan perintah ```service bind9 restart```

#### MOJOKERTO
- Edit ```nano /etc/bind/named.conf.options```
- Comment kan ```//dnimgec-validation auto;```
- Tambahkan ```allow-query{any;};```

- Edit file ```nano /etc/bind/named.conf.local```
```
zone "semerut14.pw" {
    type slave;
    masters { 10.151.77.163; }; // IP MALANG
    file "/var/lib/bind/gunung.semerut14.pw";
};
```
![img](/img/6e.png)

- Buat direktori delegasi ```mkdir /etc/bind/delegasi```
- Copykan db.local ke dalam gunung.semeru.t14.pw ```cp /etc/bind/db.local /etc/bind/delegasi/gunung.semerut14.pw```
- Edit ```nano /etc/bind/delegasi/gunung.semerut14.pw``` menjadi seperti gambar di bawah
![img](/img/6-3.png)
- Kemudian restart bind9 dengan perintah ```service bind9 restart```

- Lakukan testing ```ping gunung.semerut14.pw```
![img](/img/6-4.png)
</br></br></br>

## Soal No 7
### subdomain dengan nama http://naik.gunung.semerut14.pw, domain ini diarahkan ke IP Server PROBOLINGGO. Setelah selesai membuat keseluruhan domain, kamu diminta untuk segera mengatur web server.
- Pada MOJOKERTO edit ```nano /etc/bind/delegasi/gunung.semerut14.pw``` dan tambahkan 
	```naik	IN	A	10.151.77.164```
![img](/img/7-1.png)
- Kemudian restart bind9 dengan perintah ```service bind9 restart```

- Lakukan testing ```ping naik.gunung.semerut14.pw```
![img](/img/7.png)
</br></br></br>

