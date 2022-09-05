# Automatic Git deploy to Server Using webhook of Github

## Pre-requestment 

Install php
```
apt-get install -y php7.4-cli php7.4-fpm

```
Install Nginx to run php
```
apt-get install nginx
```
config Nginx
```
sudo nano /etc/nginx/sites-enabled/default
    
    // paste this content
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
    }
```

## SSh key generation 

Generate pub/priv ssh keys
```
    sudo mkdir /var/www/.ssh
    sudo chown -R www-data:www-data /var/www/.ssh/
    sudo -Hu www-data ssh-keygen -t rsa     // continue with enter     
    sudo cat /var/www/.ssh/id_rsa.pub
```
copy the public key and add new deploy key in github/setting/deploy keys.
Change owner of directory to use git.
```
    sudo chown -R www-data:www-data /var/www/html/
    sudo chown -R www-data:www-data /home/saas_mqtt/
```
paste deploy.php file in /var/www/html. and add new web hook in git setting's webhooks tab.

Here set http://206.189.82.115/deploy.php as a payload.

change the user to www-data and clone the git using ssh(not http).
```
    sudo su -l www-data -s /bin/bash
    git clone git@... /home/saas_mqtt
```
Now the server would sync automatically whenever git pushed.

### If you have an existing github directory, You need to set the URL for an existing remote:
```
    git remote set-url origin git@github.com:gitusername/repository.git
```