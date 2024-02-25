---
author: Naka Rhythm
title: Stratis EVM Validator node
date: 2024-02-25
tags: 
    - stratis
categories:
    - node
showToc: true
---

Di artikel kali ini ak mau berbagi tutor bagaimana menjalankan node validator Stratis

# **Gambaran Program**

Dalam rangka perayaan peluncuran Testnet Stratis Auroria, Stratis menawarkan dua kampanye yang menawarkan peserta bagian dari hadiah sebesar 1.000.000 STRAX (StratisEVM).

Berikut adalah hal-hal yang bisa Anda dapatkan dari berpartisipasi dalam kampanye:

**Pahami Testnet**

Testnet Auroria mewakili lingkungan sandbox di mana pengembang dan pengguna dapat bereksperimen dengan fitur-fitur blockchain Stratis tanpa mengorbankan aset nyata.

**Persiapkan Untuk Staking**
Untuk berpartisipasi dalam kampanye staking, Anda perlu mengirim STRAX ke Kontrak Deposit Staking dan mengoperasikan node eksekusi, node konsensus, dan validator.

**Partisipasi Aktif**
Staking bukan hanya tentang memegang token; ini tentang berpartisipasi aktif dalam jaringan. Pastikan setup staking Anda online dan operasional untuk mendukung testnet, memaksimalkan peluang Anda untuk mendapatkan imbalan.

**Terlibat dengan Komunitas**
Komunitas Stratis adalah ekosistem yang hidup dari para penggemar blockchain, pengembang, dan inovator. Terlibat dengan komunitas Stratis melalui media sosial dan Server Discord resmi dapat meningkatkan pengalaman Anda dan memberikan wawasan berharga tentang apa yang akan datang untuk Stratis dan perjalanannya dengan StratisEVM.

Informasi lebih lanjut tentang program insentif:[klik](https://www.stratisplatform.com/2024/02/07/500k-strax-airdrop-staking-quick-start-guide/)

# **Apa yang dibutuhkan?**

pertama kalian butuh spek vps minimal seperti di bawah, bisa beli di Digital ocean ataupun Contabo.

| Nama | Deskripsi |
| ------ | ----------- |
| OS | 64-bit Linux |
| CPU | 4+ cores @ 2.8+ GHz |
| Memori | 8GB+ RAM |
| Penyimpanan | SSD minim 1TB |
| Jaringan | 8 MBit/sec broadband |

lalu 20,000 STRAX Testnet dan akun tesnet

# **Langsung gas wae sam**

###### **Install dependensi**

```toml
sudo apt update && sudo apt upgrade -y
sudo apt install git build-essential curl jq wget tar ufw -y
```

###### **konfigurasi port**

```toml
sudo ufw allow ssh
sudo ufw allow 30303/tcp
sudo ufw allow 13000
sudo ufw allow 12000
sudo ufw enable
```

```toml
sudo ufw reload
```

###### **install golang**

```toml
VER="1.21.5"
wget "https://go.dev/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"

echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
source ~/.bash_profile
```
###### **buat ~/go/bin dir**

```toml
cd $HOME
mkdir -p go/bin
```

# **konfigurasi dan eksekusi klient (geth)**

###### **get geth**

```toml
cd $HOME &&\
wget https://github.com/stratisproject/go-stratis/releases/download/0.1.1/geth-linux-amd64-5c4504c.tar.gz &&\
tar -xzf geth-linux-amd64-5c4504c.tar.gz &&\
rm -r geth-linux-amd64-5c4504c.tar.gz &&\
mv ./geth ./go/bin
```

###### **buat file servis geth**

```toml
sudo tee /etc/systemd/system/geth.service > /dev/null <<EOF
[Unit]
Description="geth client"
After=network-online.target

[Service]
User=$USER
ExecStart=$(which geth) --auroria --http --http.api eth,net,engine,admin --datadir=$HOME/.geth --authrpc.addr=127.0.0.1 --authrpc.jwtsecret=jwtsecret --syncmode=full
Restart=always
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

###### **Start geth**

```toml
sudo systemctl daemon-reload
sudo systemctl enable geth
sudo systemctl restart geth
journalctl -fu geth -o cat
```

# **konfigurasi Beacon Client**

###### **Get beacon-chain**

```toml
cd $HOME
wget https://github.com/stratisproject/prysm-stratis/releases/download/0.1.1/beacon-chain-linux-amd64-0ebd251.tar.gz
tar -xzf beacon-chain-linux-amd64-0ebd251.tar.gz
rm -r beacon-chain-linux-amd64-0ebd251.tar.gz
mv ./beacon-chain ~/go/bin
```

###### **buat file servis**

```toml
tee /etc/systemd/system/beacon-chain.service > /dev/null <<EOF
[Unit]
Description="Beacon client"
After=network-online.target

[Service]
User=$USER
ExecStart=$(which beacon-chain) --auroria --datadir=$HOME/.beacon-chain --execution-endpoint=http://localhost:8551 --jwt-secret=jwtsecret --accept-terms-of-use
Restart=always
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

###### **Run beacon**

```toml
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl restart beacon-chain
journalctl -fu beacon-chain -o cat
```

# **Buat Keys and Import**

###### [goto](https://auroria.launchpad.stratisevm.com/en/overview)

{{< callout 
    title="Research"
    content="konfirmasi sampe muncul “Generate key pair”, masukan addressmu. [detail image](https://genznodes.dev/images/guide/stratis/stratis-gen-key-1.png)"
    icon="fas fa-atom"
    color="linear-gradient(95deg, #9198e5, #e66465)"
>}}

###### **Get Deposit CLI**

```toml
cd $HOME &&\
wget https://github.com/stratisproject/staking-deposit-cli/releases/download/0.1.0/staking-deposit-cli-linux-amd64.zip &&\
unzip staking-deposit-cli-linux-amd64.zip &&\
tar -C ~/go/bin -xzf staking-deposit-cli-linux-amd64/staking-deposit-cli-linux-amd64.tar.gz &&\
rm -rf staking-deposit-cli-linux-amd64 staking-deposit-cli-linux-amd64.zip
```

###### **Generate keys and deposit-data**

{{< callout 
    title="Research"
    content="Change [YOUR_WALLET_ADDRESS] with your address"
    icon="fas fa-atom"
    color="linear-gradient(95deg, #9198e5, #e66465)"
>}}

```toml
deposit new-mnemonic --num_validators 1 --chain auroria --eth1_withdrawal_address [YOUR_WALLET_ADDRESS]
```

nanti diminta buat password dan pastikan copas phrase.

###### **Get Validator CLI**

```toml
cd $HOME &&\
wget https://github.com/stratisproject/prysm-stratis/releases/download/0.1.1/validator-linux-amd64-0ebd251.tar.gz &&\
tar -xzf validator-linux-amd64-0ebd251.tar.gz &&\
rm -rf validator-linux-amd64-0ebd251.tar.gz &&\
mv ./validator ~/go/bin 
```

###### **Import validator key**

```toml
validator accounts import --keys-dir=/home/stratis/validator_keys/keystore-m*.json --auroria
```

###### **Ikuti**

```toml
Enter a wallet directory (default: /home/stratis/.eth2validators/prysm-wallet-v2):
./validator_keys
```

Download or copy your validator_keys folder to local.

# **Deposit**

[^1]: [Claim testnet token](https://auroria.faucet.stratisevm.com/)
[^2]: [Click continue on](https://auroria.launchpad.stratisevm.com/en/generate-keys)
[^3]: Upload deposit deposit_data-xxx.json
[^4]: Connect wallet,Click Continue and Confirm tx.
[^5]: [Check tx hash](https://auroria.beacon.stratisevm.com/tx/[tx_hash]#overview)

# **Check deposit progress**

###### Check your pubkey

```toml
cat ./validator_keys/deposit* | jq '.[].pubkey'
```

###### Track progress deposit
[Go to](https://auroria.beacon.stratisevm.com/validator/[YOUR_PUBKEY])

# **Run validator*

###### [^1]:Create password file
{{< callout 
    title="Research"
    content="Change “YOUR_WALLET_PASSWORD” with your password"
    icon="fas fa-atom"
    color="linear-gradient(95deg, #9198e5, #e66465)"
>}}

```toml
echo "YOUR_WALLET_PASSWORD" >> ./validator_keys/pwd.txt
```

###### [^2]: Create service file

{{< callout 
    title="Research"
    content="Change --suggested-fee-recipient=YOUR_ADDRESS with you address"
    icon="fas fa-atom"
    color="linear-gradient(95deg, #9198e5, #e66465)"
>}}

```toml
sudo tee /etc/systemd/system/validator.service > /dev/null <<EOF
[Unit]
Description=validator node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which validator) --auroria --datadir=$HOME/.validator --wallet-password-file=$HOME/validator_keys/pwd.txt --wallet-dir=$HOME/validator_keys --suggested-fee-recipient=YOUR_ADDRESS --accept-terms-of-use
Restart=always
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

###### [^3]:Run validator

```toml
sudo systemctl daemon-reload
sudo systemctl enable validator
sudo systemctl restart validator
journalctl -fu validator -o cat
```

# **Validator Active**

Be sure to keep your node validator running.

Once the deposit is confirmed and the validator is active.[You will see different logs from the validator node as well on](https://auroria.beacon.stratisevm.com/validator/[YOUR_PUBKEY]).