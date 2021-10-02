# ubuntu-20.04-nginx

`/etc/nginx/conf.d/git.conf`を作成

```
server {
        listen 80;

        root /var/www/html/git;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name 192.168.56.30;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }

        location ~ (/.*) {
            client_max_body_size 0;
            auth_basic "Git Login";
            auth_basic_user_file "/var/www/html/git/htpasswd";
            include /etc/nginx/fastcgi_params;
            fastcgi_param SCRIPT_FILENAME /usr/lib/git-core/git-http-backend;
            fastcgi_param GIT_HTTP_EXPORT_ALL "";
            fastcgi_param GIT_PROJECT_ROOT /var/www/html/git;
            fastcgi_param REMOTE_USER $remote_user;
            fastcgi_param PATH_INFO $1;
            fastcgi_pass  unix:/var/run/fcgiwrap.socket;
        }
}
```

nginx ,fcgiwrap の再起動

```
systemctl restart nginx
systemctl restart fcgiwrap
```
