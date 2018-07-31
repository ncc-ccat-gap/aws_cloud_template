# SoftEtherのインストールと設定

参考
[AWSにSoftEther VPNServerで簡単にVPN接続しよう](https://dev.classmethod.jp/cloud/aws/rk-2018-02-12-softethervpn/)

## VPCを作成し、ec2インスタンスを起動

(aws cloudformation利用)

## install 

```Bash
sudo su -
timedatectl set-timezone Asia/Tokyo
yum -y update
yum -y install git gcc
cd ~/
curl -L -O http://jp.softether-download.com/files/softether/v4.27-9668-beta-2018.05.29-tree/Source_Code/softether-src-v4.27-9668-beta.tar.gz
tar zxvf softether-src-v4.27-9668-beta.tar.gz
cd vpnserver
yes 1 | make
cd ..
mv vpnserver /opt/vpnserver
vi /etc/systemd/system/vpnserver.service
chmod 755 /etc/systemd/system/vpnserver.service
systemctl daemon-reload
systemctl start vpnserver
systemctl enable vpnserver
```

## SoftEther設定

```Bash
/opt/vpnserver/vpncmd
```

対話型コンソール

```Bash
vpncmd コマンド - SoftEther VPN コマンドライン管理ユーティリティ
SoftEther VPN コマンドライン管理ユーティリティ (vpncmd コマンド)
Version 4.24 Build 9651   (Japanese)
Compiled 2017/10/23 01:52:32 by yagi at pc33
Copyright (c) SoftEther VPN Project. All Rights Reserved.
 
vpncmd プログラムを使って以下のことができます。
 
1. VPN Server または VPN Bridge の管理
2. VPN Client の管理
3. VPN Tools コマンドの使用 (証明書作成や通信速度測定)
 
1 - 3 を選択: 1

接続先の VPN Server または VPN Bridge が動作しているコンピュータの IP アドレスまたはホスト名を指定してください。
'ホスト名:ポート番号' の形式で指定すると、ポート番号も指定できます。
(ポート番号を指定しない場合は 443 が使用されます。)
何も入力せずに Enter を押すと、localhost (このコンピュータ) のポート 443 に接続します。
接続先のホスト名または IP アドレス:#空のままでEnter

サーバーに仮想 HUB 管理モードで接続する場合は、仮想 HUB 名を入力してください。
サーバー管理モードで接続する場合は、何も入力せずに Enter を押してください。
接続先の仮想 HUB 名を入力:#空のままでEnter
VPN Server "localhost" (ポート 443) に接続しました。
 
VPN Server 全体の管理権限があります。
 
VPN Server>

VPN Server>HubCreate main
# パスワード入力を求められるので入力してください。
VPN Server>HUB main
VPN Server/main>UserCreate a.okada /Group:none /REALNAME:none /NOTE:none
# グループ、本名、説明の入力は任意です。設定しない場合は[none]と入力してください。
VPN Server/main>UserPasswordSet a.okada
# パスワード入力を求められるので入力してください。
```

```Bash
VPN Server/main>IPsecEnable /L2TP:yes /L2TPRAW:no /ETHERIP:no /DEFAULTHUB:main
# 事前共有キーの入力を求められるので任意の文字列を入力してください。
VPN Server/main>SecureNatEnable
VPN Server/main>Dhcpset /Start:192.168.30.10 /End:192.168.30.200 /Mask:255.255.255.0 /Expire:7200 /GW:192.168.30.1 /DNS:192.168.30.1 /DNS2:none /Domain:none /Log:yes /PushRoute:"10.0.0.0/255.255.0.0/192.168.30.1"
VPN Server/main>exit
```
