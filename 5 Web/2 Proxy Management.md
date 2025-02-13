# Proxy Management

File system locations:

```
/etc/apache2/sites-enabled
/etc/nginx/sites-enabled
```

- `/etc/nginx/nginx.conf` acts as the primary configuration file for Nginx (a high level overview).
- `/etc/nginx/sites-available` stores configuration files for all available sites/vhosts.
- `/etc/nginx/sites-enabled` contains symbolic links to the configuration files in `sites-available` that you want to enable.

**Note:** The paths above are the same for Apache, just replace `nginx` with `apache2`.
