对了，这个改编于此 [这个页面](https://gist.github.com/MrZYX/719014). 

```
# Make sure mod_ssl, mod_rewrite, mod_headers, mod_proxy,
# mod_proxy_http and mod_proxy_balancer are enabled
 
<VirtualHost *:80>
    ServerName huginn.example.org
    RedirectPermanent / https://huginn.example.org/
</VirtualHost>
<VirtualHost *:443>
    ServerName huginn.example.org
    DocumentRoot /path/to/huginn/public
 
    RewriteEngine On
 
    RewriteCond %{DOCUMENT_ROOT}/%{REQUEST_FILENAME} !-f
    RewriteRule ^/(.*)$ balancer://upstream%{REQUEST_URI} [P,QSA,L]
 
    <Proxy balancer://upstream>
        BalancerMember http://127.0.0.1:3000
    </Proxy>
 
    ProxyRequests Off
    ProxyVia On
    ProxyPreserveHost On
    RequestHeader set X_FORWARDED_PROTO https
 
    <Proxy *>
        Order allow,deny
        Allow from all
    </Proxy>
 
    <Directory /path/to/huginn/public>
        Allow from all
        AllowOverride all
        Options -MultiViews
    </Directory>
 
    SSLEngine On
    SSLCertificateFile /path/to/cert
    SSLCertificateKeyFile /path/to/private_key
    # maybe not needed, need for example for startssl to point to a local
    # copy of http://www.startssl.com/certs/sub.class1.server.ca.pem
    SSLCertificateChainFile /path/to/chain_file
</VirtualHost>
```

重要提示:


在 Apache 2.4 .htaccess 和 VirtualHost setting 有一些新的变化.
你需要用 Allow 替换 Deny  
在设置里面还有 Require all granted 和 Require all denied 比如这样.

从
```
    <Proxy *>
        Order allow,deny
        Allow from all
    </Proxy>

    <Directory /path/to/huginn/public>
        Allow from all
        AllowOverride all
        Options -MultiViews
    </Directory>
```
改为
```
    <Proxy *>
         Require all granted
    </Proxy>

    <Directory /path/to/huginn/public>
        Require all granted
        AllowOverride all
        Options -MultiViews
    </Directory>
```
更多的信息可以查看这个网页: http://tecadmin.net/authz-core-error-client-denied-by-server-configuration/
