# Cortensor-Ag-Uzerinde-Coklu-Node-Kurulum-Rehberi--Phase-3---Devnet-4
  
NOTE :This guide is quoted from https://docs.aldebaranode.xyz/guide/testnet/cortensor/installation

🖥Minimum Sistem Gereksinimleri

➡️OS: Ubuntu
➡️CPU: 6
➡️Memory: 16GB
➡️Disk: 100GB

## 🌐 1. Cortensor Discord'a Katılın

Yeni katılımcılar için ilk adım Cortensor Discord sunucusuna girmektir:

🔗 https://discord.gg/cortensor

## 🚀 2. Yeni Cüzdan Oluşturun ve Token Alın

✅ EVM uyumlu yeni bir cüzdan (MetaMask önerilir) oluşturun. Her node için ayrı cüzdan kullanmanız gerekir.

📅 Token Satın Alın

Aşağıdaki Uniswap linkinden 4000 adet $COR token alın:

https://app.uniswap.org/explore/tokens/ethereum/0x8e0eef788350f40255d86dfe8d91ec0ad3a4547f

📈 Token Stake Edin

https://stake.cortensor.network adresine gidin ve Pool-3 (84 gün kilitli) havuza stake edin:

✉️ Her node için 4000 COR stake edilmelidir.

##📅 3. Whitelist Kaydı (Onboard Kanalı)

Stake işlemi tamamlandıktan sonra Discord'daki #📍onboard kanalına şu şekilde mesaj yazın:

![image](https://github.com/user-attachments/assets/f6399d76-a508-400f-8e6d-e011467f2a2c)


Kayıt tamamlandıktan sonra onay bekleyin. Whitelist onayı sonrasında node kurulumuna geçebilirsiniz.

🚨 Maksimum 25 node kurabilirsiniz. Her biri için ayrı bir cüzdan ve 4000 COR gereklidir.

🎯 Cortensor Phase 3 - Ödül Sistemi 
🪙 Stake Gereksinimi

![image](https://github.com/user-attachments/assets/186e7559-90ce-459e-b642-e3543c9da9b7)

🏆 Sıralamaya Göre Ödüller

![image](https://github.com/user-attachments/assets/cdab3de1-72db-4b50-8cfa-86cfb5e1cbc0)

💸 Ekstra Ödül Bonusları

![image](https://github.com/user-attachments/assets/0aecfdb8-7758-4d10-a2fa-6651e0e1e413)

⛔ Sıralama Sınırları (Adaletli Dağıtım için)

![image](https://github.com/user-attachments/assets/b4d8658f-fa61-4c27-b419-a283ddc21003)

📈 Örnek Hesaplama
Senaryo 1: 3 node, rol bonusu %10

![image](https://github.com/user-attachments/assets/b7c52e78-b30c-4dea-9011-facde81931aa)

📈 Senaryo 2: 5 node, rol bonusu %10

![image](https://github.com/user-attachments/assets/c55921e4-2a4c-4854-bf47-a8c8df08d17b)

### 🛠️ 5. Docker ile Kurulum (Devnet 4)

## Adım 1 - Docker'ı yükleyin

```bash 
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

```
Docker'ın apt deposunu kurun.

```bash 

# Docker GPG anahtarını ekle
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Docker deposunu ekle
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo ${UBUNTU_CODENAME:-$VERSION_CODENAME}) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


```
Docker paketlerini yükleyin.

```bash 
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

```
## Adım 2 - Yükleyici Deposunu Klonla
Yükleyici deposunu klonlamak için Git'i yükleyin.

```bash 
sudo apt-get install git -y
```

```bash 
git clone https://github.com/mrheed/cortensor-installer.git && cd cortensor-installer
```

## Adım 3 - Docker Compose'u yapılandırın

```bash 
 chmod +x build.sh
 ./build.sh

```
betiğini çalıştırın. Betik şunları yapacaktır:

Cortensor imajı için Dockerfile'ı oluşturun
Toplam düğüm sayınızı ayarlamanız için docker-compose.yaml dosyasını oluşturun


docker-compose.yaml dosyası oluşturulduktan sonra, cortensor bölümü için ortam değişkenlerini değiştirmeniz gerekir:

RPC_URL : Kendi düğümünüzü çalıştırarak veya Ankr, Alchemy veya Infura gibi bir servis sağlayıcıyı kullanarak elde edebileceğiniz ARB Sepolia RPC URL'niz.

PUBLIC_KEY : Madenci düğümünüzün EVM adresi.

PRIVATE_KEY : Madenci düğümünüzün EVM özel anahtarı.

ÖNEMLİ HATIRLATMA NODE UN DÜZELİ ÇALIŞMASI İÇİN  MINER YANİ EVM CUZDANINIZIN ARB SEPOLİA KISMINDA 2 ETH BULUNDURUN

**örnek : 1 node için çalışan örnek docker-compose.yaml  dosyasının içeriği **

```bash 
 version: "3.8"
services:
  cortensor-1:
    image: cortensor-image
    container_name: cortensor-1
    restart: unless-stopped
    environment:
      RPC_URL: "https://arb-sepolia.g.alchemy.com/v2/YOUR_KEY"
      ETH_RPC_URL: "https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY"
      PUBLIC_KEY: "0xCuzdanAdres1"
      PRIVATE_KEY: "0xPrivateKey1"
      LLM_HOST: "llm-1"
      LLM_PORT: "8091"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    extra_hosts:
      - "host.docker.internal:host-gateway"

  llm-1:
    image: cortensor/llm-engine-test-1
    container_name: cts-llm-1
    restart: always
    working_dir: /app
    environment:
      PORT: "8091"
      HOST: "0.0.0.0"
      CPU_THREADS: "4"
    command: ["/app/llava-v1.5-7b-q4.llamafile", "--host", "$$HOST", "--port", "$$PORT", "--nobrowser", "--mlock", "-t", "$$CPU_THREADS"]
    extra_hosts:
      - "host.docker.internal:host-gateway"
```
 
 **örnek : 2 node için çalışan örnek docker-compose.yaml  dosyasının içeriği **

```bash 
version: "3.8"
services:
  cortensor-1:
    image: cortensor-image
    container_name: cortensor-1
    restart: unless-stopped
    environment:
      RPC_URL: "https://arb-sepolia.g.alchemy.com/v2/YOUR_KEY"
      ETH_RPC_URL: "https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY"
      PUBLIC_KEY: "0xCuzdanAdres1"
      PRIVATE_KEY: "0xPrivateKey1"
      LLM_HOST: "llm-1"
      LLM_PORT: "8091"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    extra_hosts:
      - "host.docker.internal:host-gateway"

  llm-1:
    image: cortensor/llm-engine-test-1
    container_name: cts-llm-1
    restart: always
    working_dir: /app
    environment:
      PORT: "8091"
      HOST: "0.0.0.0"
      CPU_THREADS: "4"
    command: ["/app/llava-v1.5-7b-q4.llamafile", "--host", "$$HOST", "--port", "$$PORT", "--nobrowser", "--mlock", "-t", "$$CPU_THREADS"]
    extra_hosts:
      - "host.docker.internal:host-gateway"

  cortensor-2:
    image: cortensor-image
    container_name: cortensor-2
    restart: unless-stopped
    environment:
      RPC_URL: "https://arb-sepolia.g.alchemy.com/v2/YOUR_KEY"
      ETH_RPC_URL: "https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY"
      PUBLIC_KEY: "0xCuzdanAdres2"
      PRIVATE_KEY: "0xPrivateKey2"
      LLM_HOST: "llm-2"
      LLM_PORT: "8092"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    extra_hosts:
      - "host.docker.internal:host-gateway"

  llm-2:
    image: cortensor/llm-engine-test-1
    container_name: cts-llm-2
    restart: always
    working_dir: /app
    environment:
      PORT: "8092"
      HOST: "0.0.0.0"
      CPU_THREADS: "4"
    command: ["/app/llava-v1.5-7b-q4.llamafile", "--host", "$$HOST", "--port", "$$PORT", "--nobrowser", "--mlock", "-t", "$$CPU_THREADS"]
    extra_hosts:
      - "host.docker.internal:host-gateway"
```
** yani özetle sununuzun gücüne göre aynı sunucuda  birden fazla node çalıştırmak için docker-compose.yml düzenlmek yeterli. **

## Adım 4 - Düğümleri Çalıştırın
Çalışma dizininin yükleyici klasörü içerisinde olduğundan emin olun.!!!

Bu komutu kullanarak düğümlerinizi çalıştırın

```bash 
docker compose up -d
```

Tüm konteyner günlüklerini kontrol etmek için bu komutu çalıştırın
```bash
docker compose logs --tail 100 -f
```
Düğümleri durdurmak için bu komutu çalıştırın
```bash 
docker compose down
```
Tüm düğümleri silmek için bu komutu çalıştırın
```bash 
docker compose rm
```
## Yükseltme Adımı
En son binary sürümüne yükseltmek için, aşağıdaki komutu çalıştırmanız yeterlidir
```bash 
bash upgrade.sh
```
