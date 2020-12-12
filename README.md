# Html error pages
Simple html error page templates. Before upload on your own server, you should change footer text and google analytics tracking id. 

[google analytics](#google-analytics) |
[footer](#footer) | 
[nginx](#nginx)

## Google Analytics
If you use google analytics on your page, you should change google analytics tracking code in the HTML header section.

```
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-XXXXXXXXX-X"></script>
<script>
	window.dataLayer = window.dataLayer || [];
	function gtag() { dataLayer.push(arguments); }
	gtag('js', new Date());
	gtag('config', 'UA-XXXXXXXXX-X');
</script>
```

## Footer

Change author, add links etc. 

```
<footer class="footer">
	<div class="container text-center">
		<span>&copy; <script>document.write(new Date().getUTCFullYear());</script> Tomáš Večeřa</span>
	</div>
</footer>
```

## NGINX
Nginx error pages are harcoded, but server supports custom error pages.  Error pages can be configured in the /etc/nginx/sites-available/default for default domain or you can add custom error pages only to the specific domain configuration.
```
$ vim /etc/nginx/sites-available/default
```
You can put your custom error pages in the /var/www/html/error directory e.g. 

```
server {
        listen 443 default_server;
        
		. . .

  		# Custom error pages
  		error_page 401 /error/e401.html;
		error_page 403 /error/e403.html;
		error_page 404 /error/e404.html;
		error_page 405 /error/e405.html;
		error_page 500 501 504 /error/e500.html;
		error_page 502 /error/e502.html;
		error_page 503 /error/e503.html;

		location ^~ /error/ {
			root /var/www/html/;
			internal;
		}
}
```
If you use the allow and deny directives, the server also denies access to your custom 403 error page. In this case, you must override the access rules in the error pages’ location block, you can change the internal keyword to the allow all.	

```
  location ^~ /error/ {
    root /var/www/html/;
    allow all;
  }
  ```

You can test your configuration file's syntax by typing:
```
$ sudo nginx -t
```

When no syntax errors are returned, restart Nginx by typing:
```
$ sudo service nginx restart
```
