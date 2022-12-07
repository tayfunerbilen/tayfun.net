---
title: 'Centos 9 LAMP Stack Kurulumu'
date: '2022-12-08'
---

# Centos 9 LAMP Stack Kurulumu

LAMP yani Linux, Apache, Mysql/Mariadb ve PHP. Eğer bir php projeleriniz için sunucu kurmak istiyorsanız, bu makalede sizlere sırasıyla:

- Apache
- MariaDB (MySQL)
- PHP 8
- phpMyAdmin

kurulumlarını göstereceğim.

Başlamadan önce, ben sunucularımı https://digitalocean.com üzerinden alıyorum. Kayıt olduktan sonra kartınızı tanımlayıp panele giriş yapın. Sağ üstten `Create > Droplets` diyerek ilgili sayfaya girin. Ve lokasyon olarak istediğiniz bir lokasyonu seçin, işletim sistemi olarak Centos 9'u seçin, kendinize uygun paketi seçin (şu an için min. aylık 7$) ve root şifrenizi belirleyip kurulumu gerçekleştirin.

Sunucu kurulduktan sonra ip adresini göreceksiniz. Bu ip adresi ile ssh'e şöyle bağlanabilirsiniz:

```sh
ssh root@IP_ADRES
```

Sonrada belirlediğiniz root şifresini girip bağlanıyorsunuz. Artık ssh'a bağlandıysanız, ki bu bir terminal yardımıyla olacak. Windows kullananlar putty uygulamasına bakabilir,o taraftaki en güncel client hangisi bilemiyorum. Mac kullananlar, terminal uygulamasını açıp bağlantıyı gerçekleştirdikten sonra aşağıdaki adımları izleyebilir.

## Apache Kurulumu

İlk olarak paketleri güncelleyelim.

```
dnf update
```

Apache kurulumu için şu komutu çalıştıralım:

```
dnf install httpd httpd-tools 
```

Her şey okeyse, sırasıyla aktif edelim, başlatalım.

```
systemctl enable httpd
systemctl start httpd
```

Durumuna bakmak içinde şu komutu kullanabilirsiniz:

```
systemctl status httpd
```

`http` ve `https` ayarları için şu paketi kuralım.

```
dnf install firewalld
```

Ve şu şekilde başlatalım.

```
sudo systemctl start firewalld
```

Ve `http` ve `https` ayarlarını açalım.

```
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload
```

Apache kuruldu, kontrol etmek için ip adresinizle giriş yapıp bakın:

```
http://IP_ADRES
```

## MariaDB (MySQL) Kurulumu

Kurulum için şu komutu çalıştıralım.

```
dnf install mariadb-server mariadb -y
```

Kurulduktan sonra aktif edip başlatalım.

```
systemctl start mariadb
systemctl enable mariadb
```

Durumunu kontrol etmek için:

```
systemctl status mariadb
```

Artık MariaDB'yi daha güvenli hale getirmek için şu komutu çalıştırın:

```
mysql_secure_installation
```

Mevcut root şifrenizi girdikten sonra yeni bir şifre belirleyin ve geri kalan tüm sorulara [y] olarak işaretleyip devam edin.

Kurulumun doğru olduğundan emin olmak için mysql'e bağlanıp test edelim.

```
mysql -e "SHOW DATABASES;" -p
```

Komutu çalıştırınca bir önceki adımda belirlediğiniz şifreyi isteyecek, girin ve enter'a basın. Eğer sorgu size bir değer döndürdüyse tamamdır bu iş.

## PHP 8 Kurulumu

Kurulum için şu komutu çalıştırın:

```
yum install php php-mysqlnd php-pdo php-gd php-mbstring
```

apache'yi php'den haberdar etmek için yeniden başlatalım.

```
systemctl restart httpd 
```

Artık test etmek için `/var/www/html` altına girip bir `index.php` dosyası oluşturun ve içine bir php kodu yazıp test edin.

## phpMyAdmin Kurulumu

Öncelikle EPEL'i yükleyelim.

```
dnf config-manager --set-enabled crb
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
dnf install https://dl.fedoraproject.org/pub/epel/epel-next-release-latest-9.noarch.rpm
```

şimdide phpMyAdmin'i yükleyelim.

```
dnf install phpmyadmin
```

phpMyAdmin'in ayarlarını okuması için apache'yi yeniden başlatalım.

```
systemctl restart httpd
```

şimdi config dosyasında bir düzenleme yapmak lazım. şu komutu çalıştırın:

```
nano /etc/httpd/conf.d/phpMyAdmin.conf
```

Not: eğer nano hata verirse `dnf install nano` diyerek paketi kurup tekrar deneyin.

Açılan dosyada `Require local` kısımlarını `Require all granted` olarak belirleyin ve CTRL + X'e basıp Y tuşuna basın enter'a basıp kaydedin.

apache'yi yeniden başlatın:

```
systemctl restart httpd
```

Hazırsınız! Erişmek için:

```
http://IP_ADRES/phpmyadmin
```

## .htaccess Ayarı

`.htaccess` dosyasını çalışmıyorsa, şu ayarı yapın:

```
nano /etc/httpd/conf/httpd.conf
```

açılan dosyada `Override` değerini şöyle değiştirin:

```
. . .
 # 
 # AllowOverride controls what directives may be placed in .htaccess files.
 # It can be "All", "None", or any combination of the keywords:
 # Options FileInfo AuthConfig Limit
 #
 AllowOverride All
. . .
```

Ve apache'yi yeniden başlatıyoruz, tamamız!

```
systemctl restart httpd
```

## Yazma İzinleri (CHMOD)

Dosya yazma izinleriyle ilgili sorun yaşarsanız şu komutu çalıştırın:

```
chcon -R -t httpd_sys_rw_content_t /var/www
```

## Cloudflare Ayarları

Cloudflare.com’a girip kayıt olun. Daha sonra sağ üstten add site diyerek sitenin adresini yazıp start scan butonuna basıp 40 saniye bekleyin. İşlem tamamlandıktan sonra gelen yerde 2 tane A recordu girmeniz gerek bunlar aşağıdaki gibi;

A —- siteadi.com —- sunucu ip adresi
A —- www —- sunucu ip adresi

Bunları girip next deyince ödeme kısmı gelir, free deyip sonraki adıma geçin. O adımda size 2 tane nameserver verecek. Bunları alıp domaini aldığınız siteye girin, domaini düzenleye tıklayıp ns’leri bu 2 ns ile değiştirin.

http://intodns.com/siteadi.com buradan kontrol edin, değiştiğinde cloudflare üzerinden verify yapın o kısım yeşil olunca cloudflare ayarlarıda tamam demektir. Artık domain adını yazarak sunucunuza bağlanabilirsiniz ????
