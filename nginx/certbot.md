# HTTPS with Certbot

If you want add https to the site, use [Certbot](https://certbot.eff.org/).

Generate certificate for your domain:

```bash
sudo certbot --nginx
```

Certbot will ask you about desired domain and will modify nginx config files.
