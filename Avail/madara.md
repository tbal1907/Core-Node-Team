# <h1 align="center">Karnot-Madara Kurulumu</h1>

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/5fccf5ae-bc54-4619-be40-f1cf8a2764ec)



## Linkler:
 * [Avail Resmi Websitesi](https://www.availproject.org/)
 * [Avail Resmi Twitter](https://twitter.com/AvailProject)
 * [Avail Resmi Discord](https://discord.gg/kkHAXZCNZa)



### Sistem Gereksinimleri
| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| ✔️CPU |	2+ vcpu|
| ✔️RAM	| 4+ GB |
| ✔️Storage	| 40+ GB SSD |
| ✔️UBUNTU | 20+ |

------------
MAdara ve rollapp kullanalar için duyuru...

NOT: tabiki madarayı once durduruyoruz işlemlerden sonra başlatıyoruz. vee rollapıda durdurun biraz bekleyin sonra başlatın

madaranın rpc değişmiş aynı rpcye hem madara hem rollapla bağlanmak sıkıntı olusturuyor gibi gorunuyor. değiştiricez zaten yeni madara kuranalrda rpc değişik eskilerde var bu sorun kontrol edin
```
appname=BURAYA-YUKARIDA-VERDİĞİNİZ-ROLLAPP-İSMİNİ-YAZINIZ
```
```
nano /root/.madara/app-chains/$appname/da-config.json
```
- goldberg yerine bunu yazıyoruz zaten böle ise elleşmiyoruz.....
```
wss://karnot-rpc.avail.tools:443/ws
```
------------------

#### Port açalım
```
ufw allow 4000
ufw allow 30333
ufw allow 9615
ufw allow 9944
```
## 💻Update ve gereklilikler
```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install curl build-essential pkg-config libssl-dev git wget jq make gcc chrony clang screen -y
```
```
apt-get install protobuf-compiler
```
## 🚧Rust kurulum

👉Not: soru sorucak `1` diyoruz...
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
```
source $HOME/.cargo/env
```
```
rustup toolchain install 1.75.0
```
```
rustup default 1.75.0
```
```
rustc --version
```
👉Not: çıktı böle olmalı `Version must be 1.75.0`


## 🚧Docker kurulumu

```
sudo apt install docker.io
```
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
```
sudo chmod +x /usr/local/bin/docker-compose
```
```
docker-compose --version
```

## 🚧Madara dosyalarını çekelim
👉Not: bağlantı koparsa uzun sürdüğünden sunucuya bağlanın screen -r madara yazıp screene tekrar girin ataçta kalmıs olabilir screen -ls yazdığınızda görurrsunuz önce screen -d madara sonra ctrl ad yap ve screen -r madara       yazın....
```
screen -S madara
```
```
git clone https://github.com/karnotxyz/madara-cli
```
```
cd madara-cli
```
Not: Uzun sürer bekleyeceksiniz. aşağıdaki kod uzun sürdüğünden hata verirse yada bağlantınız koaprsa diescreen açabılırısnız screen -S kurulum  çıkarakene ctrl ad girerkene screen -r kurulum ataçta kalmıssa screen -ls de gorunur screen -d kurulum yazarsanız ataçtan cıkar..
```
cargo build --release
```

## 🛠️init işlemi
```
./target/release/madara init
```
> * Ağınızın adını girin

> * mode Sovereign

> * Avail seçiyoruz

* Ekran da çıkanları kaydedin.
* buraya kadar yeni kullanıcı iseniz yapacaksınız.. çıkan avail cüzdana faucet alıcaksınız avail discorda faucet alabilmek için `gitppas 20 puana` sahip olmak gerekiyor.
* aşağıdaki kısım zaten `bir cüzdana sahipseniz import etmek için` kullanıcaksınız...

## DYM Rollapp ve madara aynıu cüzdanda sıkıntı çıkarıyor... 

NOT: ona göre aşağıyı yapıyorsunuz yoksa başlatalım adımına geçin.

👉Not: aşağıda `appname=BURAYA-YUKARIDA-VERDİĞİNİZ-ROLLAPP-İSMİNİ-YAZINIZ` kısımda yukarıda rollapınıza verdiğiniz ismi yazıcaksınız. örneğin benim corenode bu şekil olur `appname=corenode`
```
appname=BURAYA-YUKARIDA-VERDİĞİNİZ-ROLLAPP-İSMİNİ-YAZINIZ
```
```
nano /root/.madara/app-chains/$appname/da-config.json
```
* içersinde seed yazan kısımda `0x` li yeri silin tırnaklar kalsın cüzdanınızın kelimelerini buraya yapıstırın ve yine sondaki cüzdan adresini cüzdan adresinizle değiştirin `ctrl x y` enterla kaydedip çıkın.


## 🛠️Başlatalım


```
cd
```
```
./madara-cli/target/release/madara run
```

* çalışmaya başladıktan sonra bişeye dokunmuyoruz screen içersinde çalışıcak. rpclerden oturu hata alabilirsiniz durabilir. yine yukarıdaki kodla çalışır. screenden çıkmak için `ctrl a d` eğer screene girmek istersenizde `screen -r madara` diyelim screene giremiyorsunuz `screen -ls` yazıyoruz eğer screenimiz görunuyorsa ama ataç yazıyorsa `screen -d madara` daha sonra `screen -r madara` 

## 🛠️Explorer çalıştıralım..

* screende madara çalışıyor ctrl ad ile çıktık artık screende değiliz..
```
./madara-cli/target/release/madara explorer --host=$(wget -qO- eth0.me)
```
* explorer loglarını merak edenler için..
```
docker logs -f madara-explorer
```
* internet tarayıcısına aşağıdaki kısma ipnizi yazarak bağlanın exploreriniz açılacak
```
http://SUNUCU-İP-YAZ:4000
```

## 🏆PR işlemi..
👉NOT: hata yapmamak için dikat edin basit ama boşluk kaldı sırası kaçtı gibi saçmalıklardan sıkıntı cıkabilior...

* öncelikle aşağıdaki linkten repoyu forkluyoruz...

https://github.com/karnotxyz/avail-campaign-listing

* app_chains klasörüne girin. add file deyip create new file deyin. https://www.uuidgenerator.net/ sitesine gidip generate tılayın sayfanın yukarısında bir key cıkıcak kopyalayın ve new file dediğimizde githubumuzda gelen ekranda yazıp sonuna .json yazın
* daha sonra altaki kodları yapıstırıp kedinize göre düzenleyip yukarıdan commitle kaydedin. yukarda aldığımız uid sitesinden id yi aşağıda değiştireceksiniz
```
{
  "name": "rollap-isim",
  "logo": "logo-linkiniz",
  "rpc_url": "http://SUNUCU-İP-YAZ:9944",
  "explorer_url": "http://SUNUCU-İP-YAZ:4000",
  "metrics_endpoint": "http://SUNUCU-İP-YAZ:9615/metrics",
  "id": "aldığınız uidi yazın"
}
```

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/dbff5f4b-cd52-412a-be7b-c4dc5734489a)

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/259a747b-b047-43f0-983e-b725d5a41c06)

* sağ yukardan commit changes deyip kaydettik. sıra geldi pr atamaya. sol yukarıdan pull requeste tıklıyoruz new dıyoruz sonra create diyoruz. ✨️Adding yazıp daha sonra rollapp verdiğiniz adı yazıyorsunuz. ve commitliyorsunuz.
* bundan sonrası için 2 onaylama işlemi oluyor rpclerde sorun olduğu için 2ci onayı vermıyor. bekleyeceksiniz.
* günlük aksatmadan faucetlerinizi alın. eğer şöle bir resim gorurseniz pr atarken kendi dosyanıza girin ve son satırda boşluk olmadığından emin olun. kaydedip çıkın.

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/5bf09ef7-0088-45f4-b4ef-522a9125f323)

* bu durumdan oturu onaylamaz hatanın sebebi metin belgesinde yazıp json dıyoruz ya ondan zaten yapmıs olanlar yukarıda dediğim gibi dosyasına kendi github forkladığı repoda girsin boşluk varmı die baksın commet deyip kaydedip çıksın düzelmesi lazım.


### ✅ Logo yükleme

* zaten daha once rollapplar için yukledik. yuklemediyseniz kendi githubunuzda herhangi bir reponuza add file deyip logonuzu upload  edin sona tıklayın 
* mesela ben rollapp pr atmıstım dymensionun ister kendi reponuzdan isterseniz dymensionunkinden kendi logonuza tıklayıp logonuzu görün bele

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/f3bca48a-6026-4413-b875-6ab5b385bb36)

YADA

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/a1d70d40-dd43-4bd2-8bab-486f690acf7c)

* bulunduğunuz sayfanın linkinin sonuna gelin. sonuna şunu ekleyip enterlayın. `?raw=true`

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/b4e6e365-8378-4601-8a92-1f2612459303)

* artık bele görunecek link..

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/9d2ef66e-9f75-4b2c-abc0-eb0070120550)

* ekrandada böle logonuz gorunecek.

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/2640a771-3c66-4969-81ed-00de5d5f7246)

* tamamdır artık bu linki kullanabilirsiniz.



---------------------------

# TX kasma

//// ALINTIDIR /////

## Go (bu sürümü lazım)
```
ver="1.21.6"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```
## Dosyayı çekelim
```
git clone https://github.com/sarox0987/avail-madara
```
```
cd avail-madara
```
NOt: içerdeki localhsot kısmına kendi ipnizi yazın
```
nano rpc.json
```
Not: Ctrl+X Y Enter ile kaydedin
```
go mod tidy
```
```
screen -S tx
```
```
go run main.go
```
NOT: screenden cıkmak için ctrl ad girmek için screen -r tx
## Benden ekler :
1 - Json dosyasını Core-Node-Team'in yazdığı gibi kendi reponuzda oluşturun. Bilgisayarda yapıp update edince sorun veriyor.
```
2 - 9615 portunu açmayı unutan kurulumlar gördüm. O port açık değilse node çalışıyor ama github da birleştirme de sorun çıkıyor.
```
2 - Buraya arkadaşların sonradan eklediği alıntıyı muhakkak yapın. Onu yapmadan Tx olmuyor. Çoğu yerde testnet kısmında görünecek diye yazıyor. Benim deneyimlediğim kadarı ile Mainnet kısmında tx vs geliyor.
```
4 - Go version benim contabo hesabında bir türlü update olmadı. o yüzden hetzner'de yeniden kurmak zorunda kaldım.
```



