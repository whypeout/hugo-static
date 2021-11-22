---
type: "simplepg"
author: "toni"
title: "Github HTTPS dengan NETRC, Access Token dan VSCODE.dev"
date: 2021-11-22T09:20:20+07:00
tags: ["github"]
---

## NETRC

`~/.netrc` secara sederhana, `touch ~/.netrc && chmod 600 ~/.netrc && nano ~/.netrc`

```
# https login menggunakan plain-text password
machine github.com
login username
password katasandi
```

## ACCESS TOKEN

access token dapat digunakan sebagai pengganti password, dapat dibuat dg expire-date dan permission tertentu.

Login -> Account Settings -> Developer Settings -> Personal Access Tokens -> Generate New Token

token note: namatoken
expiration: 7days
ceklis: public_repo
klik: generate token
save token ke text file

```
# https login menggunakan access token
machine api.github.com
login username
password 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
```

## Menambahkan SSH-KEY ke GITHUB

generate ssh private & public key

```
# bisa tambahkan password, atau dikosongkan saja
ssh-keygen -t rsa -b 4096 -C "toni@gmail.com"
ssh-keygen -t ed25519 -C "toni@gmail.com"

# jalankan ssh-agent daemon
eval $(ssh-agent -s)

# tambahkan ssh-key ke ssh-agent
# ssh-agent akan membuat otentikasi key yang memiliki password, untuk memasukkan password 1x saat menambahkan key ke ssh-agent saja
ssh-add ~/.ssh/id_ed25519
```

Menambahkan public key ke akun github

Login github -> Account Settings -> SSH & GPG Keys -> SSH Keys (New Keys) ->
- beri nama ssh key. batagor
- Paste-kan id_rsa.pub
- klik add ssh key
- ketikkan password jika diminta confirmation


## Github SSH over HTTPS

pada beberapa kasus, koneksi keluar menuju port 22 (ssh) di blok oleh firewall.
sebagai alternatif, github menyediakan otentikasi menggunakan SSH over HTTPS.

```
# Test login menggunakan SSH over HTTPS
ssh -T -p 443 git@ssh.github.com
Hi whypeout! You've successfully authenticated, but GitHub does not provide shell access.

# buat host config untuk github, `nano ~/.ssh/config`
Host github.com
Hostname ssh.github.com
Port 443
User git

# test koneksi lagi
ssh -T git@github.com
Hi whypeout! You've successfully authenticated, but GitHub does not provide shell access.
```

## Mengedit menggunakan VSCODE.dev

Visual Studio merupakan IDE/text-editor yang sedang naik daun. ada beberapa versi dari VSCODE ini,
- VSCODE yg dpt diinstall di pc/laptop client. support linux, windows & mac
- VSCODE server yg dpt diinstall di server/headless pc/laptop. untuk dpt diakses dari mana saja (lokal & internet)
- VSCODE.dev yg dpt langsung melakukan edit file di repository github (terkoneksi), seperti VSCODE server tapi di host pada server yg dimiliki oleh microsoft, kita tinggal pakai

contoh memakai vscode.dev: [whypeout/hugo-static](vscode.dev/github.com/hugo-static)
akan ada tahapan otentikasi, seperti: login ke akun github, memberikan otorisasi ke vscode.dev untuk mengakses infomasi github, dll


sumber:
- [https://gist.github.com/technoweenie/1072829](https://gist.github.com/technoweenie/1072829)
- [https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
- [https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- [https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
- [https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port#enabling-ssh-connections-over-https](https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port#enabling-ssh-connections-over-https)
- [https://github.com/cdr/code-server](https://github.com/cdr/code-server)