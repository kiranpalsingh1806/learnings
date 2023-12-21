## 1. Go Installation [Source](https://golangdocs.com/install-go-linux)

```sh
wget https://dl.google.com/go/go1.13.5.linux-amd64.tar.gz
sudo tar -C /usr/local/ -xzf go1.13.5.linux-amd64.tar.gz
cd /usr/local/
echo $PATH
sudo nano $HOME/.profile
export PATH=$PATH:/usr/local/go/bin
source .profile
cat $HOME/.profile
go version
```