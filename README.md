
# Pengertian Termux

Termux merupakan aplikasi di android yang bisa menjalankan CLI di android. Walaupun mempunyai beberapa batasan, termux berguna sekali untuk menjalankan code yang ringan dan juga mudah di bawa kemana mana. Termux harus di download [di sini](https://github.com/termux/termux-app) karena google sangat ketat mengenai keamanan android.

## Apa yang bisa dilakukan Termux
Termux bisa melakukan apa saja, mulai dari menjalankan php file, menjalankan discord bot(discord.js) dll. Termuk mempunyai berbagai package yang bisa di download, karna itu termux banyak di minati.

### Rooted Device
Termux akan berjalan dengan baik sekali jika perangkat anda sudah di root.


## SSH Server
Termux dapat dicontrol jarak jauh cli nya menggunakan SSH, bagaimana caranya? ikuti cara ini:

update dan upgrade dulu semua package nya(optional):
```
$ pkg update && pkg upgrade
```

jika sudah, install package yang di butuhkan:
```
$ pkg install openssh nmap neofetch -y
```
keterangan: 
* openssh adalah package untuk menjalankan SSH.
* nmap adalah package untuk melihat port apa yang sedang berjalan.
* neofetch (optional).

cek port yang berjalan menggunakan:
```
$ nmap localhost
```
biasanya openssh menggunakan port: 8022 (default nya harusnya 22).

setelah selesai semua di download, langsung set password dan juga cek nama user dari perangkat kamu:
```
$ whoami
```
> keterangan: whoami adalah command untuk menampilkan nama user di termux.

jika ada output: u0_XXXX salin dan lakukan ini:
```
$ passwd nama_user_termux
```
jangan lupa cek ip private kamu (bisa di setting wifi atau di settings > tentag ponsel).

Beralih ke pc dan jalankan command berikut(di cmd maupun gitbash):
```
$ ssh nama_user_kamu@ip_private_hp_kamu -p 8022
```
nanti akan seperti ini:
```
$ ssh u0_xt67s@192.168.0.3 -p 8022
```

jika sudah, masukan password yang di set tadi. dan tara sudah berhasil!. namun remote ssh ini hanya bisa di 1 jaringan yang sama, kalau mau ke luar jaringan gunakan port forward di router(di indihome ada) agar ip private di terusin ke ip public, biar bisa di akses di mana aja.



## Apache2, PHP, Mysql, dan Composer
Bagaimana cara menjalankan server di termux? ternyata bisa loh, menggunakan 3 aplikasi di atas, gimana caranya?

### A. Apache2 & PHP

Pertama, setup storage dahulu (agar internal storage mudah terbaca):
```
$ termux-setup-storage
```
jika sudah, kamu akan bisa mengakses storage internal `cd /sdcard/`

setelah itu install package ini:
```
$ pkg install php php-apache
```

buat dahulu folder htdocs di internal storage menggunakan:
```
$ cd /sdcard && mkdir htdocs && cd ~
```

lalu masukan command ini di cli:
```
$ htdocs='/sdcard/htdocs'
```

install apache2 nya:
```
$ pkg install apache2 -y
```

setelah itu ikuti instruksi ini:
```
edit file ini menggunakan nano lalu tambahkan baris code sesuai instruksi:

$ nano $PREFIX/etc/apache2/httpd.conf

on line 66 add this:
LoadModule php_module /data/data/com.termux/files/usr/libexec/apache2/libphp.so

replace this in line 70-73:
LoadModule mpm_prefork_module libexec/apache2/mod_mpm_prefork.so
#LoadModule mpm_worker_module libexec/apache2/mod_mpm_worker.so

enable rwrite module:
LoadModule rewrite_module libexec/apache2/mod_rewrite.so

find DocumentRoot and replace like this:
DocumentRoot "/sdcard/htdocs"
<Directory "/sdcard/htdocs">

matikan Options Indexes FollowSymLinks dengan cara ini:
#Options Indexes FollowSymLinks
tambahkan ini di dalam <Directory>:
DirectoryIndex index.php index.html

jangan lupa keluar dari nano dan save, caranya:
CTRL + x + y + ENTER
```

jangan lupa restart apache menggunakan:
```
$ apachectl -k stop
$ apachectl
```

selesai, coba buka browser dan jalankan `localhost:8080`maka server akan berjalan di tandai dengan tulisan "it's work!", untuk cek version dari php gunakan command berikut: 
```
$ php -v
```

### B. Mysql
cara penginstallan agak susah, jadi soon aja yaa~ kalo mau liat bisa liat preferensi [di sini](https://stackoverflow.com/questions/65006526/how-can-i-install-mysql-in-termux)

### C. Composer
composer adalah aplikasi untuk menginstall vendor vendor dari php(kalau di js namanya module, mirip mirip lah kek npm nya js).
Composer **Sangat butuh** php, jadi install php dahulu seperti di atas!

cara penginstallannya cukup mudah, yaitu:

install curl terlebih dahulu
```
$ pkg install curl
```

setelah itu lanjut install composernya:
```
$ curl -sS https://getcomposer.org/installer | php -- --install-dir=/data/data/com.termux/files/usr/bin --filename=composer
```

jika berhasil, coba gunakan command ini dan composer akan mengeluarkan info versi:
```
$ composer -V 
```
Selamat! kamu bisa menjalankan web server dan juga menginstall package di composer!

# Terimakasih
terimakasih untuk saya sendiri, saya buat ini karna takutnya lupa xixix, agak trcky juga main termux. udh si segitu aja, sekian, terimakasih!

# Support
- support on Saweria

# Error dan bug 
jika kamu mengalami kendala, hubungi saya lewat email di bawah ini atau buka issues di github ini!
- contact@kazukikunn.xyz
