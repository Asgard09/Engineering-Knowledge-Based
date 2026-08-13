# 1. Concepts
Nginx is an open source, high performance web server (Listen request, Process request, Return response), reverse proxy, load balancer, TCP-UDP proxy
# 2. Problems with exposing application directly
## 2.1. Serving static files is inefficient
Suppose your website has: HTML, CSS, JS, Images, Videos. If have 1000 image requests, backend will handle all of this –> Less CPU available for business logic.
Nginx is writing in C and extremely fast in serving static files
```nginx
server {
    listen 80;

    location /images/ {
        root /var/www;
    }
}
```
## 2.2. Security
The user never know exactly your backend port, they just access nginx’s port, nginx can also provide: block malicious request, filter IP address.
## 2.7. Multiple application in one server
Nginx can route request into multiple service (api to Srping, chat to Node, ui to React)
# 3. Nginx commands in Linux server
- Check if Nginx is installed: `nginx -v`
- Check the Nginx Configuration Syntax: `sudo nginx -t`
- Check Nginx status: `sudo systemctl status nginx`
- Start Nginx: `sudo systemctl status nginx`
- Stop Nginx: `sudo systemctl stop nginx`
- Restart Nginx: `sudo systemctl restart nginx`
- Reload Nginx: `sudo systemctl reload nginx`
- Open Config of Nginx: `nano /etc/nginx/sites-available/name`
