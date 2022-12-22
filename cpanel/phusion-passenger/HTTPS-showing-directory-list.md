### Symptom
- Accessing the Node/Ruby application via HTTP is working
- But accessing HTTPS is causing directory list to load

### Cause
- The include line of Phusion passenger (Application Manager in cPanel) configuration in `/etc/apache2/conf/httpd.conf` is missing for HTTPS block for the domain name


### Fix
- Copy the phusion passenger configuration include line from HTTP block to HTTPS block

For instance, if the issue is with the domain `test.domain.com` , then we have to do the following

- Edit `/etc/apache2/conf/httpd.conf` file. This file contains configuration for all the domains in the server.
- Find the HTTP block for the target domain `test.domain.com` 
- The HTTP Block will have code written inside `<VirtualHost 1.2.3.4:80>` block, where `1.2.3.4` is the public IP of the cPanel server
- Look for a line that looks like the following in the HTTP block for the domain

```bash
Include "/etc/apache2/conf.d/userdata/std/2_4/username/test.domain.com/*.conf"
```

This line includes the Phusion passenger config file located inside the `/etc/apache2/conf.d/userdata/std/2_4/username/test.domain.com/` directory.

In this example, `username` denotes the cpanel user and `test.domain.com` denotes the domain we are configuring.

- Copy the `Include` line and paste it in inside the HTTPS `<VirtualHost 1.2.3.4:443>` block for `test.domain.com` 
- Check apache syntax and restart apache server
```bash
apachectl configtest && systemctl restart {httpd,apache2} ; systemctl status {httpd,apache2} 
```


### References
- https://forums.cpanel.net/threads/node-js-app-registered-but-not-working-correctly-on-add-on-domain.673833/

