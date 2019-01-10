# HubStatistics
Multiplayer Hub Landing Page

# Usage
Just host with a webserver, preferably on the same URL that your hub is running on.

You will need to setup a reverse proxy to upgrade WS to WSS connections if you run HTTPS. This is how to do it with nginx:

```nginx
# Web Sockets settings
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

# wss -> ws
server {
  listen [::]:3900;
  listen 3900;

  ssl on;
  ssl_certificate /path/to/fullchain.pem;
  ssl_certificate_key /path/to/privkey.pem;


  location / {
      proxy_pass http://localhost:3800;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;
  }
}
```