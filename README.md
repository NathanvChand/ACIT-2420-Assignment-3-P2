# ACIT-2420-Assignment-3
ACIT 2420 Assignment 3 part 1

## Note:

make sure nginx and ufw is installed first.

use this: 

```
sudo pacman -S nginx ufw
```



## Task 1

### creatting the 'webgen' user with the home directory '/var/lib/' with no login. 

```
sudo useradd -r -m -d /var/lib/webgen -s /usr/bin/nologin webge 
```
Move the cloned repo files to the webgen users directory.

this is how I did it:

```
 sudo mv /home/arch/2420-as3-p2-start/generate_index /var/lib/webgen/bin
 ```




## Task 2

Go into the terminal and make your service file using a text editor.

```
sudo nvim /etc/systemd/system/generate-index.service
```

Write your service file so that it runs at 5:00 every day, only when the network goes online, while specifying the webgen user and its group.

Here is my generate-index.service script:

```
[Unit]
Description=Run the damn script
After=network-online.target

[Service]
Type=simple
ExecStart=/var/lib/webgen/bin/generate_index
User=webgen
WorkingDirectory=/var/lib/webgen
```

Then create your .timer file

Here is my timer: 

```
[Unit]
Description=Run the damn script at 5:00 every day

[Timer]
OnCalendar=*-*-* 5:00:00
Persistent=true

[Install]
WantedBy=timers.target
```
## Task 3

make a new file for your nginx config.

Do this: 

```
sudo nvim /etc/nginx/websites/web-config
````

Open nigix config:

```
sudo nvim /etc/nginx/nginx.conf
```

then change the user to webgen and specify its home dir.

Next make the server setup file:
```
sudo nvim /etc/nginx/websites/web-configs
```

set up the index.html to port 80

```
 server {
        listen 80;
        server_name "localhost"

        root /var/lib/webgen/HTML
        index index.html

        location /static/ {
                root /var/www/HTML;
        }
  }
```

## Then add permissions:

```
perms sudo chown -R webgen:webgen /var/www/HTML 
```


## Task 4

Firstly, enable the previosuly installed ufw service:

Do this: 

```
sudo systemctl enable --now ufw.service
```

Then use these commands to configure ufw:

Execute these in the order shown: 

```
sudo ufw allow ssh

sudo ufw limit ssh

sudo ufw allow http

sudo ufw enable
```

## Task 5

I ran out of time for trouble shooting and making it all work, so no screenshot here, however I gave this assignment the ol' coledge try.
