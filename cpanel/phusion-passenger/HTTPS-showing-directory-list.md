### Symptom
- HTTP is working
- HTTPS caused directory list to load

### Cause
- The include line of Phusion passenger (Application Manager in cPanel) configuration in `/etc/apache2/conf/httpd.conf` is missing for HTTPS block


### Fix
- Copy the include line from HTTP block to HTTPS block

- Edit `/etc/apache2/conf/httpd.conf` file
- Find the HTTP block (Tip: Search by the domain name to find it)
The HTTP Block will have code written inside `<VirtualHost 1.2.3.4:80` block, where 1.2.3.4 is the public IP of the cPanel server
- Look for a line that looks like the following in the HTTP block for the domain

```bash
Include "/etc/apache2/conf.d/userdata/std/2_4/username/test.domain.com/*.conf"
```

This line includes the Phusion passenger config file located inside the `/etc/apache2/conf.d/userdata/std/2_4/username/test.domain.com/` directory.

Here, `username` is the cpanel user and `test.domain.com` is the domain we are configuring.

- Copy the `Include` line and paste it in inside the HTTPS block for `test.domain.com` 
- Check apache syntax and restart apache server
```bash
apachectl configtest && systemctl restart {httpd,apache2} ; systemctl status {httpd,apache2} 
```


### References
- https://forums.cpanel.net/threads/node-js-app-registered-but-not-working-correctly-on-add-on-domain.673833/

