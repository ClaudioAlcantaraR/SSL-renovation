## Renew The Let’s Encrypt Certificate
Let’s Encrypt certificates are only valid for 90 days. To renew the certificate before it expires, run the following commands from the server console as the bitnami user. Remember to replace the DOMAIN placeholder with your actual domain name, and the EMAIL-ADDRESS placeholder with your email address.

## For Apache

### 1- Login your server using SSH

`ssh -i Key.pem  bitnami@00.00.00.00`

### 2- Stop Apache Server

`sudo /opt/bitnami/ctlscript.sh stop`

### 3- Renew Certificate

`sudo /opt/bitnami/letsencrypt/lego --tls --email="EMAIL-ADDRESS" --domains="DOMAIN" --path="/opt/bitnami/letsencrypt" renew --days 90`

### 4- Start Apache Server 

`sudo /opt/bitnami/ctlscript.sh start`

## For NGINX

### 1- Login your server using SSH

`ssh -i Key.pem  bitnami@00.00.00.00`

### 2- Stop Nginx

`sudo /opt/bitnami/ctlscript.sh stop nginx`

### 3- Renew Certificate

`sudo /opt/bitnami/letsencrypt/lego --tls --email="EMAIL-ADDRESS" --domains="DOMAIN" --path="/opt/bitnami/letsencrypt" renew --days 90`

### 4- Start Nginx Server 

`sudo /opt/bitnami/ctlscript.sh start nginx`

----

## Automatically renew

To automatically renew your certificates before they expire, write a script to perform the above tasks and schedule a cron job to run the script periodically. To do this:

- Create a script at `/opt/bitnami/letsencrypt/scripts/renew-certificate.sh`

`sudo nano /opt/bitnami/letsencrypt/scripts/renew-certificate.sh`

Enter the following content into the script and save it. Remember to replace the DOMAIN placeholder with your actual domain name, and the EMAIL-ADDRESS placeholder with your email address.

### For Apache

```
#!/bin/bash

sudo /opt/bitnami/ctlscript.sh stop apache
sudo /opt/bitnami/letsencrypt/lego --tls --email="EMAIL-ADDRESS" --domains="DOMAIN" --path="/opt/bitnami/letsencrypt" renew --days 90
sudo /opt/bitnami/ctlscript.sh start apache
```

### For Nginx

```
#!/bin/bash

sudo /opt/bitnami/ctlscript.sh stop nginx
sudo /opt/bitnami/letsencrypt/lego --tls --email="EMAIL-ADDRESS" --domains="DOMAIN" --path="/opt/bitnami/letsencrypt" renew --days 90
sudo /opt/bitnami/ctlscript.sh start nginx
```

- Make the script executable:

`chmod +x /opt/bitnami/letsencrypt/scripts/renew-certificate.sh`

- Execute the following command to open the crontab editor:

`sudo crontab -e`

- Add the following lines to the crontab file and save it:

`0 0 1 * * /opt/bitnami/letsencrypt/scripts/renew-certificate.sh 2> /dev/null`

NOTE: If renewing multiple domains, remember to update the /opt/bitnami/letsencrypt/renew-certificate.sh script to include the additional domain name(s) in the lego command.
