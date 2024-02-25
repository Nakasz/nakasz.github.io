---
author: Naka Rhythm
title: Voi Network Node
date: 2024-02-25
thumbnail:
    src: 'images/12.jpeg'
    alt: 'Stratis'
    object_position: '50% 100%'
    height: 600px
images: ['images/12.jpeg']
tags: 
    - stratis
categories:
    - node
showToc: true
---

# What is  Voi?

**The Consensus**

Voi a layer-1 blockchain solution that starts with Algorand’s technology. Algorand offers a proven consensus mechanism that will allow us to innovate in more impactful areas and get to mainnet faster with significantly lower capital requirements. Leveraging this consensus also allows us to invest more resources in those contributing real value to the network.

**Wallet Protocol**

Voi uses a new wallet technology that we call VoiID. VoiID solves the primary pain points felt when using blockchain applications:

01) Multi-step transaction approval process
02) Complex private key and identification method
03) Requirement to hold the native token in your wallet to use the blockchain

Voi creates a user experience similar to web2, contributing to Voi's goal of bringing the blockchain to mainstream audiences.

An interactive game to establish a solid foundation with the support of our Voiagers and vital applications.

Selebihnya, kalian bisa baca [di sini](https://www.voi.network/) atau [di sini](https://docs.google.com/document/d/1SSpf3QcJc0gZt7Ilrnx6FptMK6ohAeph_1pc1c5zKwI/edit)

# Apa yang dibutuhkan?

Pertama, kalian harus isi form [d sini](https://docs.google.com/forms/d/e/1FAIpQLSddXtHs281rOMlFTK0KmmDmJQn5f1rUFk89N9XCO8uxZBYCwg/viewform)

Kedua, kalian butuh spek vps minimal dn yang direkomendasikan kek gini:

| Minimum Requirements	 | Recommended Requirements |
| ------ | ----------- |
| 4 CPU cores | 8 CPU cores |
| 8 GB RAM| 16 GB RAM |
| 100 GB storage | 100 GB storage |
| 100 Mbps network | 1 Gbps network ||

Detail on [read ths](https://voinetwork.github.io/voi-swarm/)

ketiga, penyimpanan (optional) nah, di atas kan butuh storage 100GB, aku mau buat alokasi disk 100GB buat node voi ini. gimana caranya?

```toml
fallocate -l 100G voi.img
```
lalu format ur file

```toml
mkfs.ext4 voi.img
```
mount ur file :

```toml
mkdir voi
sudo mount -o loop voi.img voi
```
finnaly cek makek command ini:

```toml
df -h
```

# yok dah

## Setting up a new Voi Node

**New to voi?**

```toml
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/VoiNetwork/voi-swarm/main/install.sh)"
```

**Using an Existing Account/Address with Mnemonic?**

For installing with an existing account or mnemonic, use this method. After installation, [update your name and GUID](https://voinetwork.github.io/voi-swarm/updating/telemetry/#getting-your-telemetry-status) for health rewards if setting up on a new server.

```toml
export VOINETWORK_IMPORT_ACCOUNT=1
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/VoiNetwork/voi-swarm/main/install.sh)"
```

**Installing Without Wallet Setup (advanced)?**

Selengkapnya kalian bisa baca [di sini](https://voinetwork.github.io/voi-swarm/installation/installation-advanced/) 


# Tamat

jika ada yg mau ditanyakan, silahkan kalian bertanya di replyku atau di discord voi [join here](https://discord.gg/voi-network-1055863853633785857)

{{< callout 
    title="Research"
    content="You must run this node for 3-6 months, so think about it first before you proceed to the next steps 🍸"
    icon="fas fa-atom"
    color="linear-gradient(95deg, #9198e5, #e66465)"
>}}



