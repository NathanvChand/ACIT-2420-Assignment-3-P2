# ACIT-2420-Assignment-3-P2
ACIT 2420 Assignment 3 part 2

## This is a continuation of the Assignmemt 3 homework

---

## Task 1

### Creating two new DigitalOcean droplets

 Firstly, Ive craeted two new DigitalOcean droplets for this assignment. These two new droplets will be used for the load balancer.

Here they are:
![image](https://github.com/user-attachments/assets/02e5bb1c-bf04-4171-82f8-5713fb249fac)

---

## Task 2

### Creating the Load Balancer

Here is the created Load Balancer:
![image](https://github.com/user-attachments/assets/5b7d00c9-6223-49b7-8310-94c3a381186e)

---

## Task 3

### Cloning the starter code

Use Git clone command to clone the following repository into your machine:

```
https://git.sr.ht/~nathan_climbs/2420-as3-p2-start
```
---

## Task 4

### Updating your server configuration to include a file server

To do this I'll refer back to the part 1 assignment, and reuse the code I previously made.

#### Creating a System User
```
sudo useradd -r -m -d /var/lib/webgen -s /usr/bin/nologin webge 
```
Move the cloned repo files to the webgen users directory.

this is how I did it:

```
 sudo mv /home/arch/2420-as3-p2-start/generate_index /var/lib/webgen/bin
 ```

#### The File Structure
Should look like this:
```
.
├── bin/
│ └── generate_index
├── documents/
│ ├── file-one
│ └── file-two
└── HTML/
└── index.html
```

#### The .service file
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

#### The .timer file
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

#### The nginx Configuration

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
---

