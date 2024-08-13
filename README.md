# Setup Wireguard VPN (Client Side)

## 1. Generate Keypair

Well-known configuration paths:
 - **Linux**: `/etc/wireguard`
 - **mac with homebrew**: `/opt/homebrew/etc/wireguard`

Generate keypair

```sh
wg genkey | tee privatekey-filename | wg pubkey > publickey-filename
```

## 2. Write and Apply Configuration

Fill below configuration and save as `.conf` file to configuration path. Usually, named as `wg[n].conf` (like `wg0.conf`)

```conf
[Interface]
Address = [vpn-ip @ this]
PrivateKey = [privatekey-[target] @ this]

[Peer]
PublicKey = [publickey-[target] @ server]
AllowedIps = [vpn network ip]/24
Endpoint = [public endpoint @ server]
PersistentKeepalive = 5
```

For example:

```conf
[Interface]
Address = 10.0.1.11
PrivateKey = iFdkwo349102J4Jd4J5d23Jdij23JKfdsd23DMad3d3=


[Peer]
PublicKey = Jij2F3JKfdsd5d23Jddk49102J4wo3d3id4d3J23DMa=
AllowedIps = 10.0.1.0/24
Endpoint = 223.130.200.219
PersistentKeepalive = 5
```

> [!NOTE]  
> PrivateKey, PublicKey has made by random-type, Endpoint is ip of naver.com

## 3. Start and Establish Network

### i. Start Instantly

```sh
wg-quick up [config-name]
```

If there is `wg0.conf`:

```sh
wg-quick up wg0.conf
```

### ii. Register Service

#### Linux

```sh
sudo systemctl enable wg-quick@[config-name]
sudo systemctl start wg-quick@[config-name]
```

#### macOS

Modify and apply [plist configuration](./config/com.wireguard.wg[n].plist) to `/Library/LaunchDaemons`.

```zsh
sudo chmod 644 /Library/LaunchDaemons com.wireguard.wg[n].plist
sudo launchctl enable system/com.wireguard.wg[n].plist
sudo launchctl bootstrap system /Library/LaunchDaemons/com.wireguard.wg0.plist
sudo launchctl print system/com.wireguard.wg0
```
