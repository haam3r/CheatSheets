# Apache Hardening

Tested on Ubuntu 14.04 LTS and Apache 2.4 and appears to mostly work on Ubuntu 12.04 LTS with Apache 2.22

## `vim /etc/apache2/conf-enabled/security.conf`

```bash
# Lisanud Andres 05.01.2016
ServerTokens Prod
ServerSignature Off
TraceEnable Off
Header set X-Content-Type-Options: "nosniff"
Header set X-Frame-Options: "sameorigin"
FileETag None

#Cookie setting on Ubuntu 12.04 messes with wordpress
Header edit Set-Cookie ^(.*)$ $1;HttpOnly;Secure
Header set X-XSS-Protection "1; mode=block"
Header unset X-Powered-By
```

## `vim /etc/php5/apache2/php.ini`

1. Add exec, system, shell_exec, and passthru to disable_functions.
1. Change expose_php to Off.
1. Ensure that display_errors, track_errors and html_errors are set to Off.


### References
* http://geekflare.com/apache-web-server-hardening-security/
* http://insiderattack.blogspot.com.ee/2014/03/apache-security-configuring-secure.html
* https://www.owasp.org/index.php/List_of_useful_HTTP_headers
* http://blog.mattbrock.co.uk/hardening-the-security-on-ubuntu-server-14-04/



