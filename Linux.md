# Linux

- [Linux](#linux)
  - [1. How to give sudo permission inside /var/www](#1-how-to-give-sudo-permission-inside-varwww)
  - [2. How to increase disk space in EC2 Link](#2-how-to-increase-disk-space-in-ec2-link)
  - [3. Install/Upgrading PostgreSQL](#3-installupgrading-postgresql)

## 1. How to give sudo permission inside /var/www

```sh
sudo addgroup webusers
sudo usermod -aG webusers ubuntu
sudo chown -R :webusers /var/www
sudo chmod -R 775 /var/www
sudo usermod -aG www-data ubuntu
```

```sh
ls -l /var/www
sudo chmod +w /var/www
ls -l /var/www
sudo chown ubuntu:ubuntu /var/www
```

## 2. How to increase disk space in EC2 [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html)

```sh
# Check the disk space
sudo du -h --max-depth=1

lsblk

sudo fdisk -l

df -h | head -n 2

df -hT

sudo resize2fs /dev/root
```

## 3. Install/Upgrading PostgreSQL

```sh
# Check version of postgresql
/usr/lib/postgresql/14/bin/psql -V

# Install important dependencies for postgresql
sudo apt install dirmngr ca-certificates software-properties-common apt-transport-https lsb-release curl -y

```