![image](https://user-images.githubusercontent.com/88971654/213976049-b6cc6293-a8be-4426-a118-5f7b6dc62ca1.png)
`

#
#### System Requirements:
1. 2 CPU 
2. 4 GB RAM 
3. 200 GB SSD - Ubuntu 20.04.
`
 
#
# 1. GETTING STARTED


### Buat user root kalian
```
sudo adduser <user>
sudo usermod -aG sudo <user>
```
- `<user>` ganti dengan username kalian ( bebas )
- masukkan pasword kalian 2x

- izinkan akses

### Login user kalian

```
su - <user>
```
- masukkan password yang kalian atur tadi

### Download dependensi yang diperlukan

```
sudo apt-get update && sudo apt-get upgrade -y 
sudo apt-get install tar curl ufw jq make clang pkg-config libssl-dev build-essential git jq expect -y
```

### Install paket GO
```
wget https://golang.org/dl/go1.17.2.linux-amd64.tar.gz && sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.17.2.linux-amd64.tar.gz 
```

### Update Go di environmental variable di bashrc

```
mkdir $HOME/go && mkdir $HOME/go/bin
echo 'export GOROOT=/usr/local/go' >> ~/.bashrc 
echo 'export GOPATH=$HOME/go' >> ~/.bashrc 
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> ~/.bashrc 
source ~/.bashrc && source $HOME/.profile && go version
```

### Kalian juga Bisa memperbarui environmental variable di bash_profile
```echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile 
source ~/.bash_profile 
go version
```
![image](https://user-images.githubusercontent.com/88971654/213969623-30ba77b4-1c7a-4fd7-9a79-4f5b2266950e.png)


# 2. BASIC SETUP

``` 
cd
git clone https://github.com/stratosnet/stratos-chain.git 
cd stratos-chain
```
``` 
git checkout v0.5.0 
make build 
```
- Cek Versi stratos, kalian akan melihat output seperti yang ditunjukkan:
![image](https://user-images.githubusercontent.com/88971654/213971306-94b939d4-dfc6-4bdf-a1b0-b1ff964c0e49.png)

### Jika tidak keluar seperti di screenshoot coba dengan cara ini :
Download file binary langsung

```
cd
mkdir stratos && cd stratos
wget https://github.com/stratosnet/stratos-chain/releases/download/v0.7.0/stchaincli
wget https://github.com/stratosnet/stratos-chain/releases/download/v0.7.0/stchaind
```

### Salin kedua file biner ke jalur bin
```
chmod +x stchaincli stchaind 
sudo cp -r stchaincli stchaind /usr/bin/ /usr/local/bin/ $HOME/go/bin/
```
output akan sama

![image](https://user-images.githubusercontent.com/88971654/213971863-406b6124-6dec-4b09-b327-13b3b517670d.png)

### Atur Nickname kalian

```
echo "export NICKNAME="<nickname>"" >> ~/.bash_profile
```

ubah `<nickname>` dengan nickname kalian

#### Reload variable kalian

```
source ~/.bash_profile
```

#### Cek variabel akan muncul nickname kalian
![image](https://user-images.githubusercontent.com/88971654/213973104-1826e865-db7e-4314-ab06-09f2f32b7133.png)

# 3. Initialize & Konfigurasi Node

### Initialize node
```
stchaind init $NICKNAME --chain-id tropos-3
```

### Perbarui file genesis dan config
```
rm -vf $HOME/.stchaind/config/genesis* $HOME/.stchaind/config/config.toml 
wget -P $HOME/.stchaind/config/ https://raw.githubusercontent.com/stratosnet/stratos-chain-testnet/main/genesis.json
wget -P $HOME/.stchaind/config/ https://raw.githubusercontent.com/stratosnet/stratos-chain-testnet/main/config.toml
```

### Konfigurasikan NodeName kalian di Configure dengan mengimpor dari bash_profile
```
sed -i "s/mynode/"$NICKNAME"/g" $HOME/.stchaind/config/config.toml
```

### Periksa apakah nama Moniker disebutkan dengan benar di file Config.toml
```
cat $HOME/.stchaind/config/config.toml | grep "moniker"
```

- #### Jika berbeda, ubah secara manual dengan cara :
```
$HOME/.stchaind/config/config.toml
```
cari `moniker=<node-name>` ganti dengan nama moniker kalian

