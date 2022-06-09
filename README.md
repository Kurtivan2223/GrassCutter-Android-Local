# GrussCutter for Android

***NOTE***
- Only for local not public
- 2.7 guide

Let's say done setting up the server.

## ****[MitmProxy Edition]****
****[config.json]****

change the following to your ip

e.g
![App Screenshot](https://raw.githubusercontent.com/Kurtivan2223/GrassCutter-Android-Local/main/Screenshot/IP.PNG)

```
"accessAddress": "127.0.0.1",
```
into

e.g
```
"accessAddress": "192.168.254.105",
```
##
****[proxy_config.py]****

change:
```
REMOTE_HOST = "localhost"
```

into

e.g

```
REMOTE_HOST = "192.168.254.105"
```

##

****[mitmproxy]****

****1.****

Run following command in cmd :
```cmd
mitmdump -s proxy.py -k
```

after that open a new CMD with Administrator Rights/Elevated and enter command:

```
certutil -addstore root %USERPROFILE%\.mitmproxy\mitmproxy-ca-cert.cer
```

****2.****

Setting up Proxy

Wifi -> wifi_name_details -> Proxy -> manual

Host name: #your host name e.g 192.168.254.105

Port: #your host name port e.g 8080

Unused Sites: #decluded sites where you can still access

e.g 
 
![App Screenshot](https://raw.githubusercontent.com/Kurtivan2223/GrassCutter-Android-Local/main/Screenshot/285574571_401674071974696_8763711250069748976_n.jpg)

##

****Installing Certificate in Android****

in browser access 

[http://mitmi.it/](http://mitm.it)

Navigate to android part and choose *Get mitmproxy-ca-cert.cer*

Instruction for installing the cert is included in the site.

##

****[Patched Apk]****

download apk at

[Github](https://github.com/577fkj/GenshinProxy/releases/tag/releases)

##

## ****[NGINX Reverse Proxy Edition]****

****[config.json]****

IP
`
"accessAddress": "to your ipv4";
`

PORT SSL
`
"bindPort": 442, #443 was used for nginx
`

##

****[nginx.conf]****
```
worker_processes  1;

events {
    worker_connections  1024;
}


http {
    ssl_password_file certificate/password.txt; #if your cert has password

    ssl_session_cache   shared:SSL:10m;
    ssl_session_timeout 10m;

    server {
        server_name your_ipv4;

        location / {
            proxy_pass https://your_ipv4:442;
            proxy_set_header Host $host;
        }

        listen 443 ssl;
        listen [::]:443 ssl ipv6only=on;
        
        ssl_certificate certificate/private.pem;     
		ssl_certificate_key certificate/private.key;

		ssl_protocols       TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
		ssl_ciphers         HIGH:!aNULL:!MD5;
    }

    server {
        if ($host = your_ipv4){
            return 301 https://$host$request_uri;
        }

        listen 80 default_server;
        listen [::]:80 default_server;

        server_name your_ipv4;

        return 404;
    }
}
```

**NOTE:** After setting up nginx just connect to it, if ssl is invalid just check skip ssl verification

END ENJOY
