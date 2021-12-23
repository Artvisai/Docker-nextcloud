Docker & Docker-compose install:
https://serveradmin.ru/kak-ustanovit-docker-na-centos/

Installing MariaDB:
https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-debian-10

Installing nextcloud:
Folder for container:
	$ mkdir nextcloud && cd nextcloud
Make file for docker:
	$ vi docker-compose.yml
Write it in this file:
version: '2'
<
volumes:
  nextcloud:
  db:
  data:

services:
  db:
    image: mariadb
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW --skip-innodb-read-only-compressed
    restart: always
    volumes:
      - db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=1234osradar
      - MYSQL_PASSWORD=1234osradar
      - MYSQL_DATABASE=nextclouddb
      - MYSQL_USER=nextclouduser

  app:
    image: nextcloud
    ports:
      - 1234:80
    links:
      - db
    volumes:
      - nextcloud:/var/www/html
      - data:/var/www/html/data
    restart: always
>
Run docker-compose:
  $ docker-compose up -d

Go to:
http://your-server-ip:1234
Insert required data to sign up in next cloud (Don't forget to change "localhost" to "db:3306" while loging in mariaDB)

