
# nginx tips

* [config generator](https://www.digitalocean.com/community/tools/nginx)

* [checker](https://github.com/yandex/gixy)

* disable server info

```
http {
    ...
    server_tokens off;
    ...
}
```

* change header

```
# install nginx-extras
http {
      ...
      # Replace nginx with apache or whatever you want as server name.
      more_set_headers 'Server: apache';
      ...
}
```

* limit buffer

```
client_body_buffer_size     1k
client_header_buffer_size   1k
client_max_body_size        1k
large_client_header_buffers 2 1k
```

* security headers

```
http {
    ...
    # Prevent clickjacking attacks
    add_header X-Frame-Options "SAMEORIGIN" always;

    # Add an HSTS header to your nginx server
    add_header Strict-Transport-Security "max-age=31536000; includeSubdomains; always";

    # Cross-site scripting protection
    add_header X-XSS-Protection "1; mode=block";
    default-src 'self' https: data: 'unsafe-inline' 'unsafe-eval';

    # Prevention of MIME confusion-based attacks
    add_header X-Content-Type-Options "nosniff" always;

    # Hide X-Powered-By header
    proxy_hide_header X-Powered-By;
    more_clear_headers 'X-Powered-By';

    # Referrer policy
    add_header Referrer-Policy "origin-when-cross-origin" always;
    ...
}
```

* block user agent

```
server {
    ...
    # Block wget user agent
    if ($http_user_agent ~* (wget|curl) ) {
          return 403;
    }
    ...
}
```

* allow/deny IP

```
location / {
    ...
    # Block one workstation
    deny    192.168.1.1;

    # Allow anyone in 192.168.1.0/24
    allow   192.168.1.0/24;

    # Drop rest of the world
    deny    all;
    ...
}
```

* disable unused requests

```
server {
    ...
    location / {
        limit_except GET HEAD POST {
            deny all;
        }
    }
    ...
}
```
* security.txt

/var/www/html/.security.txt
```
Contact: secu@example.org
Rate-limit: 300
Preferred-Languages: EN
```

```
server {
        ...
        location /security.txt {
                return 301 http://$host/.well-known/security.txt;
        }

        location = /.well-known/security.txt {
                alias /var/www/html/.security.txt;
        }
        ...
}
```

* ssl

```
ssl_protocols TLSv1.2 TLSv1.3;
ssl_prefer_server_ciphers on;
```

