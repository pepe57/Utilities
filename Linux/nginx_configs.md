## Redirect

`sudo nano /etc/nginx/sites-available/<domain>`

```
map $host $redirect_target {
    sub.domain.com     <URL>
    sub.domain.com     <URL>
}

server {
    listen 80;
    server_name sub.domain.com sub.domain.com;

    return 301 $redirect_target;
}
```

## telemt

<https://github.com/telemt/telemt/issues/617#issuecomment-4286171352>

`sudo nano /etc/nginx/sites-available/<domain>`

```
# latest nginx

server {
    listen 127.0.0.1:8443 ssl;
    http2 on;
    server_name <domain>;

    # Issuing within certbot
    ssl_certificate     /etc/letsencrypt/live/<domain>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/<domain>/privkey.pem;

    ssl_protocols TLSv1.3;

    root /var/www/<domain>;
    index index.html;

    access_log /var/log/nginx/<domain>.access.log;
    error_log  /var/log/nginx/<domain>.error.log;

    location / {
        try_files $uri $uri/ =404;
    }
}

server {
    listen      80;
    server_name <domain>;

    return 301 https://<domain>$request_uri;
}
```

## x-ui

```
map $host $redirect_target {
    script.sophia.team https://raw.githubusercontent.com/farag2/Sophia-Script-for-Windows/refs/heads/main/Download_Sophia.ps1;
    app.sophia.team    https://raw.githubusercontent.com/Sophia-Community/SophiApp/refs/heads/master/Download_SophiApp.ps1;
    hv.sophia.team     https://raw.githubusercontent.com/farag2/Hyper-V/refs/heads/master/Hyper-V.ps1;
    sl.sophia.team     https://raw.githubusercontent.com/farag2/Sophia-Script-for-Windows/refs/heads/main/Download_Latest_Sophia.ps1;
    nv.sophia.team     https://raw.githubusercontent.com/farag2/NVidia-Driver-Downloader/refs/heads/main/Download_NVidia_Driver.ps1;
    oobe.sophia.team   https://raw.githubusercontent.com/farag2/Utilities/refs/heads/master/OOBE/OOBE.ps1;
    bd.sophia.team     https://raw.githubusercontent.com/farag2/Utilities/refs/heads/master/Download/Better_Discord.ps1;
}

server {
    listen 80;
    server_name <domain> <domain>;

    return 301 $redirect_target;
}

server {
    listen 80;
    server_name <domain>;

    root /var/www/<domain>;
    index index.html;
}

server {
    # for nginx lower than 1.25
    listen 127.0.0.1:8443 ssl;
    http2 on;
    server_name <domain>;

    # Issuing within x-ui
    ssl_certificate     /root/cert/<domain>/fullchain.pem;
    ssl_certificate_key /root/cert/<domain>/privkey.pem;
    ssl_protocols       TLSv1.3;

    root  /var/www/<domain>;
    index index.html;
    location / { try_files $uri $uri/ =404; }
}
```
