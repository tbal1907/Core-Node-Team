# <h1 align="center">Avail Light Node Kurulumu</h1>
![image](https://github.com/molla202/Avail/assets/91562185/a6461113-7737-40a0-9d2a-3049a7097663)


## Linkler:
 * [Avail Resmi Websitesi](https://www.availproject.org/)
 * [Avail Resmi Twitter](https://twitter.com/AvailProject)
 * [Avail Resmi Discord](https://discord.gg/kkHAXZCNZa)
 * [Avail Light Node Dökümantasyon](https://docs.availproject.org/operate/node/light-client/)
 * [Avail Light Node Form](https://docs.google.com/forms/d/e/1FAIpQLSeL6aXqz6vBbYEgD1cZKaQ4vwbN2o3Rxys-wKTuKySVR-oS8g/viewform)

#### Kurulum Video : [LINK](https://www.youtube.com/watch?v=ToiTJdsGf_o&t)
#### Cüzdan import video : [LINK](https://www.youtube.com/watch?v=GcT0J6IUhI8)

### Sistem Gereksinimleri
| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| ✔️CPU |	2+ vcpu|
| ✔️RAM	| 4+ GB |
| ✔️Storage	| 40+ GB SSD |
| ✔️UBUNTU | 22 |
## Update
```
sudo apt update && sudo apt upgrade -y
sudo apt-get install make clang pkg-config libssl-dev build-essential
```
```
screen -S alight
```
Screen contabo da yüklü gelmiyor. yüklemek için

```
sudo apt install screen
```
* Screen -S oturumismi  ==> bu şekil de yeni bir oturum açılır CTRL +A+D ile oturum kapatılmadan ana ekrana geçilir.
* Screen -xS oturumismi ==> açık olan oturuma tekrardan bağlanılır.
* screen -ls ==> açık olan oturumların listesi gelir.


### Avail Light Client'i yükleyin
```
cd
wget https://github.com/availproject/avail-light/releases/download/v1.7.9/avail-light-linux-amd64.tar.gz
tar -xvzf avail-light-linux-amd64.tar.gz
mv avail-light-linux-amd64 avail-light
rm -rf avail-light-linux-amd64.tar.gz
```



#### Servis dosyasını oluşturalım.
```
sudo tee /etc/systemd/system/availd.service > /dev/null <<EOF
[Unit]
Description=Avail Light Client
After=network.target
StartLimitIntervalSec=0
[Service]
User=root
ExecStart=/root/avail-light --network goldberg
Restart=always
RestartSec=120
[Install]
WantedBy=multi-user.target
EOF
```

#### Başlatalım
```
sudo systemctl daemon-reload
systemctl enable availd
sudo systemctl restart availd
```

#################### Eklediklerim ################################
* 20.02.2024 itibari ile versiyon 1.7.8 olarak yenilendi.

* [Burada açıklamalar var.](https://github.com/availproject/avail-light
* Nodu başlatırken aşağı da olan komutu gir çalışmaya başladıktan sonra Ctrl + C ile nodu durdur.
```
./avail-light --network goldberg

```
* Daha sonra 
```
nano identity.toml

```
* Bu komut ile  Node Config dosyasına ulaşmış olacaksın. Avail cüzdanı ilk kurduğun da sana verdiği keyler vardı. Onları burada olnalar ile değiştir. Sonra CTRL+X yap. Kaydetmek için Y veya N demeni isteyecek. Y yazıp enterle. Sonra nodu yukarıda ki komut ile başlat. Herşeyi doğru yaptıysan hem logları isterken hem de block isterken sorunsuz çalışması lazım. 

## Loglar
```
journalctl -u availd -fo cat
```

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/6c65bb10-3b46-4df8-a5b3-7cbf59e58cdc)



#### Son blok görüntüleme
```
curl "http://localhost:7000/v1/latest_block"
```



-------------------------------------------
## <h1 align="center">Avail Light Node ubuntu 20 kısa yol </h1>

## Update
```
sudo apt update && sudo apt upgrade -y
sudo apt-get install make clang pkg-config libssl-dev build-essential
```
```
screen -S alight
```

### Avail Light Client'i yükleyin
```
cd
wget https://github.com/molla202/Avail/raw/main/avail-light.tar.gz
tar -xvzf avail-light.tar.gz
chmod 744 avail-light
```



#### Servis dosyasını oluşturalım.
```
sudo tee /etc/systemd/system/availd.service > /dev/null <<EOF
[Unit]
Description=Avail Light Client
After=network.target
StartLimitIntervalSec=0
[Service]
User=root
ExecStart=/root/avail-light --network goldberg
Restart=always
RestartSec=120
[Install]
WantedBy=multi-user.target
EOF
```

#### Başlatalım
```
sudo systemctl daemon-reload
systemctl enable availd
sudo systemctl restart availd
```

## Loglar
```
journalctl -u availd -fo cat
```

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/d376265b-d818-4056-a991-f1707c24d085)

Not: eğer hata verirse resimdeki gibi [bu kısımdaki](https://github.com/Core-Node-Team/Testnet-TR/blob/main/Avail/Light%20Node.md#rust-kural%C4%B1m) (rust kuralım kısmı sadece)kurulumu yapıp restart edip deneyin.
```
sudo systemctl restart availd
journalctl -u availd -fo cat
```
#### Son blok görüntüleme
```
curl "http://localhost:7000/v1/latest_block"
```

--------------------------------------------

## <h1 align="center">Avail Light Node ubuntu 20 veya hata alanalr için uzun yol</h1>

## Update
```
sudo apt update && sudo apt upgrade -y
sudo apt install make clang pkg-config libssl-dev build-essential git screen protobuf-compiler -y
```



### Rust kuralım
```
curl https://sh.rustup.rs -sSf | sh
```
Not: 1 seçiyoruz
```
source $HOME/.cargo/env
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```
```
screen -S alight
```
### Dosyaları çekelim
```
git clone https://github.com/availproject/avail-light.git
cd avail-light
git checkout v1.7.6
```
### Kuralım
```
cargo build --release
```

### Servis oluşturalım
```
sudo tee /etc/systemd/system/availd.service > /dev/null <<EOF
[Unit] 
Description=Avail Light Client
After=network.target
StartLimitIntervalSec=0
[Service] 
User=root 
ExecStart=/root/avail-light/target/release/avail-light --network goldberg
Restart=always 
RestartSec=120
[Install] 
WantedBy=multi-user.target
EOF
```
### Başlatalım
```
sudo systemctl daemon-reload
sudo systemctl enable availd.service
sudo systemctl restart availd.service
```
### Loglar
```
journalctl -u availd -fo cat
```
