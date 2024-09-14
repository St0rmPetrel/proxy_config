# Instruction how to set up own VLESS sever and ShadowSocks

## What is VLESS and SS?

Read this [VLESS vs ShadowSocks](https://habr.com/ru/articles/839656/)

## How to set your own proxy server?

- Install ansible on your host.

Installation example for MacOS
```sh
brew install pipx
pipx install --include-deps ansible
ansible-playbook --version
```

- Install sshpass
 
If you don't want to bother with SSH keys, and want use just password
```sh
brew install sshpass
sshpass -V
```

- Download this repo

You can do `git clone`, or use [zip_download_link](https://github.com/St0rmPetrel/proxy_config/archive/refs/heads/main.zip).

- Create VPS (Virtual personal server)

You can choose whatever hosting provider u like, I use [vdsina](https://www.vdsina.com/).
Create there Debian or Ubuntu server, I use `Debian 12`. The sever should have public IPv4, it's important.

Also there is a good practice to change default SSH port to another, change root password, create ordinary user (to connect via SSH) and forbid to connect via SSH to root user. But this step is omitted for simplicity. 

- Create `inventory.yml` file

You can use my snippet, just fill `IPv4` and `PASSWORD` from your VPS.
```yml
myvps:
  hosts:
    vps_01:
      ansible_host: MY_VPS_IPv4
      ansible_password: MY_VPS_PASSWORD
```

Save this file in the root of the repo folder (which you downloaded on the previous step).

- Edit `playbook-proxy-setup.yml`

This step is recommended, but not required.

You can check latest version of xray here [Xray-core](https://github.com/XTLS/Xray-core).

Xray ShadowSocks port better take greater than 1000.

Be careful with VLESS camouflage domain, it should be something very reliable foreign site which will not block.

To check if it work use command (on the VPS)
```sh 
curl -v -L --tlsv1.3 --http2 https://YOU_CAMOUFLAGE_DOMAIN
```

if it return some html code, that mean every think is okay. Also if you have couple options of camouflage domain pick one
with smallest ping (on the VPS).

```sh
ping YOU_CAMOUFLAGE_DOMAIN
```

- Run ansible to configure your proxy server

```sh
ansible-playbook -i inventory.yml  playbook-proxy-setup.yml
```

- Connect clients

In logs of previous command in the end there will be two links, one for VLESS client and one for ShadowSocks. Save this links and connect to your server through them.

Here is example of good multiplatform client which can connect to VLESS and ShadowSocks [Hiddify](https://hiddify.com/). Just copy your links to clip board, push `new profile` in the Hiddify, push `Add from clipboard` and all done. 

## Sources

I used [Xray-ansible repo](https://github.com/pilosus/Xray-ansible/tree/main) as an example.
