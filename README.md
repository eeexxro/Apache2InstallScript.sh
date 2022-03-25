# Apache2InstallScript.sh
надо улучшить скрипт
#!/bin/bash
# приветствие
echo "welcome !!!"
# apache установка 
  apt-get update > /dev/null
  echo "apt-get update"
  apt-get upgrade > /dev/null
  echo "apt-get upgrade"
  apt-get install -y apache2
  echo "installing apache2 services"
# получения информации
read -p "введите имя виртуального хоста: " site
if
 [ ! "$site" = "-n" ]

then
echo "создание директории"
# создание директории
  mkdir /var/www/$site.com

 else 
 exit
 fi
# редактирования конфигурационных файлов
     sed -e "
           9c\      ServerName ${site}.com
           10c\      ServerAlias www\.${site}.com
           11c\      ServerAdmin admin\@${site}.com
           12c\    
           13c\      DocumentRoot /var/www/${site}.com
           " /etc/apache2/sites-available/000-default.conf > /etc/apache2/sites-available/$site.com.conf
     sed "s/Apache2/${site}/" /var/www/html/index.html > /var/www/${site}.com/index.html

# модификация и изменения владельца файла
  chmod -R 755 /var/www
  echo "модификация и изменения владельца файла"
  chown -R $USER:$USER /var/www/$site.com/index.html

# приминение конфигурации
  a2ensite $site.com.conf
  echo $site.com is enabling
  systemctl reload apache2.service
  a2dissite 000-default.conf
  echo "default site is disabling"
  systemctl reload apache2.service
  systemctl restart apache2.service
  systemctl status apache2.service
