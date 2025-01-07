# DevCybSec Machines Info

## atacker

| ipv4 | mask | gateway|
|------|------|--------|
| 192.168.154.100 | /24 | 192.168.154.1 |

__user__ : parrot
__password__ :  parrot

## metasploitable

| ipv4 | mask | gateway|
|------|------|--------|
| 192.168.154.2 | /24 | 192.168.154.1 |

__user__: mfsadmin
__password__: mfsadmin

## backend

| ipv4 | mask | gateway|
|------|------|--------|
| 192.168.154.69 | /24 | 192.168.154.1 |


### if custom gateway ip appears as default delete it from routing table and added in the following way:

```terminal
  sudo ip route show
  default via <bridge-gateway-ip> dev bridge-interface ## ens33
  default via <custom-gateway-ip> dev custom-interface

  sudo ip route del default via <custom-gateway-ip>
  sudo ip route add <ip-range/mask> via custom-gateway-ip dev <interface>
  # example - 192.168.154.0/24 via 192.168.154.1 dev ens33
```

⚠️ you can watch your interface name using:
```
# ip addr show
```

### install node, npm, pnpm, nestjs

``` terminal
sudo apt install nodejs npm
sudo npm i -g pnpm
sudo npm i -g @nestjs/cli
```

## create application directory

```terminal
cd ⁓/
sudo mkdir applications
```

### create a nestjs project or make a git pull from some repository
``` terminal
cd ⁓/applications/
nest new backend-init
```

### run the application

```
# cd ⁓/applications/backend-init/
# sudo npm run build
# sudo pm2 start pnpm --name "backend-init" -- start
```

## nginx

| ipv4 | mask | gateway|
|------|------|--------|
| 192.168.154.3 | /24 | 192.168.154.1 |

### if custom gateway ip appears as default delete it from routing table.

```terminal
  sudo ip route show
  default via <bridge-gateway-ip> dev bridge-interface ## ens33
  default via <custom-gateway-ip> dev custom-interface

  sudo ip route del default via <custom-gateway-ip>
```

### Install nginx and ufw

``` terminal
sudo apt install nginx ufw
```

### key and crt creation

```terminal 
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/domainname.key \
    -out /etc/ssl/certs/domainname.crt \
    -subj "/CN=domainname.com"
```

### enable ports using ufw

``` terminal
  sudo ufw allow 80
  sudo ufw allow 443
```

### create the domain file 
``` terminal
cd /etc/nginx/sites-available/
sudo nano domainname
```

### paste the next content

``` nginx
server {

    set $backend_ip 192.168.154.69;
    set $client_ip 192.168.154.50

    listen 443 ssl;
    server_name domainname.com;

    ssl_certificate /etc/ssl/certs/domainname.crt;
    ssl_certificate_key /etc/ssl/private/domainname.key;

    location /api {
        proxy_pass http://$backend_ip:3000; # Node A's IP and Nest.js app port
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;

        rewrite ^/api(/.*)$ $1 break;
    }

    location / {
        proxy_pass http://$client_ip:3000; # Node B's IP and Next.js app port
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }
}

server {
    listen 80;
    server_name domainname.com;

    return 301 https://$host$request_uri;
}
```

Copy the created file running this command:

```
$ sudo ln -s /etc/nginx/sites-available/domainname /etc/nginx/sites-enabled/
```

Check if syntax in domainname file is correct

```
$ sudo nginx -t
```

Restart nginx server

```
sudo systemctl reload nginx
```


## client

| ipv4 | mask | gateway|
|------|------|--------|
| 192.168.154.50 | /24 | 192.168.154.1 |


### if custom gateway ip appears as default delete it from routing table and added in the following way:

```terminal
  sudo ip route show
  default via <bridge-gateway-ip> dev bridge-interface ## ens33
  default via <custom-gateway-ip> dev custom-interface

  sudo ip route del default via <custom-gateway-ip>
  sudo ip route add <ip-range/mask> via custom-gateway-ip dev <interface>
  # example - 192.168.154.0/24 via 192.168.154.1 dev ens33
```

### install node, npm, pnpm

``` terminal
sudo apt install nodejs npm
```

## create application directory

```terminal
cd ⁓/
sudo mkdir applications
```

### create a nextjs project or make a git pull from some repository
``` terminal
cd ⁓/applications/
npx create-next-app@latest client-init
```

### run the application

```
# cd ⁓/applications/client-init/
# sudo npm run build
# sudo pm2 start pnpm --name "client-init" -- start
```