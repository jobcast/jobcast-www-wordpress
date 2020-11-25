# www.jobcast.net on WordPress

## Back up WordPress site
On October 26, 2020, I used the Duplicator WordPress plugin to back up www.jobcast.net that was hosted on WordPress at Bluehost.

Instructions: https://www.wpbeginner.com/wp-tutorials/how-to-properly-move-wordpress-to-a-new-domain-without-losing-seo/

The Duplicator plugin  generated the following 2 files:
* Installer file: `installer.php`
* Archive bundle: `20201026_postjobsonfacebooksocialre_d54bfd85ceb8baef5839_20201026183057_archive.zip`

## Set up WordPress on Docker
Instructions: https://themeisle.com/blog/local-wordpress-development-using-docker/

Run the following command to install and run WordPress and MySQL:
```
docker-compose up -d
```
That command will start 2 containers. One for WordPress and one for MySQL. Connect to the WordPress container with the following command:
```
docker exec -it <container name> bash
```
Delete everything in the root WordPress folder (`/var/www/html`) using bash commands.
```
rm -rf *
rm .htaccess
```

Exit the container:
```
exit
```
Copy the installer and archive into the Docker container:
```
docker cp .\20201026_postjobsonfacebooksocialre_d54bfd85ceb8baef5839_20201026183057_archive.zip <container name>:/var/www/html/20201026_postjobsonfacebooksocialre_d54bfd85ceb8baef5839_20201026183057_archive.zip

docker cp .\installer.php <container name>:/var/www/html/installer.php
```

Add this to host file:
```
# for Docker wordpress development
127.0.0.3		www.jobcast.local
```

Now run the installer:
http://www.jobcast.local:8090/installer.php

In Step 2 of 4, database information will be requested. For Host, use the network link, `mysql`.
```
Action: Connect and Remove All Data
Host: mysql
Database: jobcast-db
User: johnny
Password: password
```

## Start / Stop WordPress Site
Start:
```
docker-compose up -d
```

Stop:
```
docker-compose stop
```

## Teardown
```
docker-compose down
```