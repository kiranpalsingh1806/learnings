# Linux

- [Linux](#linux)
  - [1. How to give sudo permission inside /var/www](#1-how-to-give-sudo-permission-inside-varwww)

## 1. How to give sudo permission inside /var/www

```sh
sudo addgroup webusers
sudo usermod -aG webusers ubuntu
sudo chown -R :webusers /var/www
sudo chmod -R 775 /var/www
sudo usermod -aG www-data ubuntu
```
