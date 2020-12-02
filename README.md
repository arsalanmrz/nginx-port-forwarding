### nginx-port-forwarding

#### Commands (for instance : ubuntu os)
- first update your os's packages : `sudo apt update`
- install nginx on your server : `sudo apt install nginx`
- enable nginx service : `sudo systemctl enable nginx`
- start nginx service : `sudo systemctl start nginx`
- for example you run a java application (with tomcat or docker) or any other application and it is run on 8060 port.
- try this command : `sudo nano /etc/nginx/sites-available/test.conf` (like test.conf that I put it down for you)
- for save above file and exit the nano editor : press---> `ctrl+o` , `ctrl+m` , `ctrl+x`
- after that you should add this test.conf to your sites-enable nginx directory , so try this command : 
      `sudo ln -s /etc/nginx/sites-available/test.conf /etc/nginx/sites-enabled/`
- after all you should restart the nginx : `sudo systemctl restart nginx`
- finally , if you put `srv1.mirbozorgi.com` in your browser , it's redirected to `your_machine's_ip:8060`.
- If you wanna have https (because some new update of client framework or engine like unity accept just https):
    - `sudo apt update`
    - `sudo snap install core; sudo snap refresh core`
    - `sudo snap install --classic certbot`
    - `sudo ln -s /snap/bin/certbot /usr/bin/certbot`
    - `sudo certbot --nginx` (it will aks you some question , answer them with your personal data.)
    - `sudo certbot renew --dry-run` (cerbot is free and  your https will expire within 3 months,  with this,you run the automate cron for renew the SSL/TLS)

-  `https://srv1.mirbozorgi.com` is now available



#### Sample for test.conf
``` js
upstream port_forwarding {
    server 127.0.0.1:8060;
}

server {
    listen 80;
    server_name srv1.mirbozorgi.com;

    access_log off;
    error_log /var/log/nginx/srv1_mirbozorgi_com_error.log;

    location / {
        proxy_pass         http://port_forwarding;
        proxy_redirect     off;
        proxy_set_header   Host $host;
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Host $server_name;
    }
}


```
