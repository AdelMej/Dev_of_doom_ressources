# Nginx Cheat Sheet (Container & Static Hosting)

## Core Concepts
- **Worker-based, event-driven** web server
- Excellent for **static files**, **reverse proxy**, **load balancing**
- Configuration is declarative and deterministic

---

## Important Paths (Docker / Linux)

| Purpose | Path |
|------|------|
| Main config | `/etc/nginx/nginx.conf` |
| Site configs | `/etc/nginx/conf.d/*.conf` |
| Default web root | `/usr/share/nginx/html` |
| Common custom web root | `/var/www/html` |
| Logs | `/var/log/nginx/` |

---

## Minimal Static Server Config

```nginx
server {
    listen 9000;
    server_name localhost;

    root /var/www/html/site;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

---

## Key Directives

### Server
- `listen` → port to bind
- `server_name` → domain / hostname
- `root` → directory of files
- `index` → default file

### Location
- `/` → match all paths
- `try_files` → file resolution logic

---

## Common listen patterns

```nginx
listen 80;
listen 9000;
listen 443 ssl;
listen [::]:80;
```

---

## Reverse Proxy (Backend API)

```nginx
location /api/ {
    proxy_pass http://backend:5000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}
```

---

## SPA (Single Page App) Support

```nginx
location / {
    try_files $uri /index.html;
}
```

---

## Enable Caching (Static Assets)

```nginx
location ~* \.(css|js|png|jpg|svg)$ {
    expires 30d;
    add_header Cache-Control "public";
}
```

---

## Security Headers (Basic)

```nginx
add_header X-Content-Type-Options nosniff;
add_header X-Frame-Options DENY;
add_header X-XSS-Protection "1; mode=block";
```

---

## Nginx + Docker Invariants

- Do NOT install nginx if using `nginx:latest`
- One container = one server block
- Config lives in `/etc/nginx/conf.d/`
- Logs go to stdout/stderr
- Immutable image, mutable volumes

---

## Minimal Dockerfile (Static Site)

```dockerfile
FROM nginx:latest

COPY site /var/www/html/site
COPY site.conf /etc/nginx/conf.d/default.conf

EXPOSE 9000
```

---

## Run Container

```bash
podman run -p 9000:9000 nginx-site
```

---

## Reload Config (Inside Container)

```bash
nginx -s reload
```

---

## Debugging

```bash
nginx -t          # test config
ps aux | grep nginx
curl localhost:9000
```

---

## Mental Model

- Nginx does **exactly** what you tell it
- If it fails, it’s almost always:
  - wrong path
  - wrong port
  - missing file
  - typo

No magic. No guessing.
