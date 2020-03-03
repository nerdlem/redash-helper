This repo contains some companion files to my [post on using Redash](https://lem.click/post/notes-on-running-standalone-redash/) as standalone processes, without containers. Feel free to reuse and adapt.

# TL;DR

File `redash-production.default` is meant as a template for the environment variables required by the Python runtime, to launch an instance named _production_. You need to customize this to your environment.

The `redash.nginx` file contains the site configuration for the _Nginx_ that fronts my redash instances. Simply rename accordingly and place it under `/etc/nginx/sites-available`, place the requisite symlink to `/etc/nginx/sites-enabled` and edit to suit your environment. You'll need to edit IP addresses and domain names, and point to the right certificate location.

If you followed my post, the `*.service` files would be simple additions to the `/etc/systemd/system` directory. Then you would simply…

```bash
systemctl enable  redash
systemctl enable  redash-scheduler@production
systemctl enable  redash-worker@production
systemctl enable  redash-server@production
```

You will need to configure the redash database – where Redash actually stores its own data – in order to launch the services.

```bash
systemctl start   redash-scheduler@production
systemctl start   redash-worker@production
systemctl start   redash-server@production
systemctl restart nginx
```
