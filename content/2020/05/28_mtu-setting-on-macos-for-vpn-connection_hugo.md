---
title: "MTU setting on macOS for VPN connection"
Date: 2020-05-28T17:15:29+09:00
Category: misc
Slug: mtu-setting-on-macos-for-vpn-connection
Authors: Ryo KOBAYASHI
tags: 
- "mac"
---

## Work from home...

コロナの影響で，最近はずっと自宅からリモートワークをしている．
リモートワークで職場のネットワークにアクセスするには，VPN接続が必要なことがある．
VPNで接続しても10分程度で突然接続が切れてしまうことが多々あった．
しかし，この現象は同様の環境におかれている知人のところでは発生しない．知人は，使っているプロバイダ，マシン（Windows）などが異なる．
いくらかのネットの情報より，MTU（Maximum Transmission Unit）の設定が問題かもしれないということに至った．

## Estimate MTU value

macOS 10.15.4 (Catalina) では，`ping`コマンドでMTUの上限値を調べることができる．
```bash
$ ping -c 5 -D -s 1472 www.google.com
```
- `-s 1472`：通信バイトサイズを指定（ヘッダ分`+28`がこれに加わる）
- `-D`：通信パッケージを分割しない？　通信パッケージが大きすぎたらダメだとなってほしいので．
- `-c 5`：テスト通信の回数

ウチのWiFi環境では，VPN接続しない場合は，1472（つまり 1472+28=1500）で通信できた．

以下，VPN環境下での`ping`テスト結果：
```bash
$ ping -c 5 -D -s 1253 www.google.com
PING www.google.com (172.217.161.228): 1253 data bytes
ping: sendto: Message too long
ping: sendto: Message too long
Request timeout for icmp_seq 0
ping: sendto: Message too long
Request timeout for icmp_seq 1
^C
--- www.google.com ping statistics ---
3 packets transmitted, 0 packets received, 100.0% packet loss
[kobayashi@mbp-rk py3Dmol]$ ping -c 5 -D -s 1252 www.google.com
PING www.google.com (172.217.161.228): 1252 data bytes
76 bytes from 172.217.161.228: icmp_seq=0 ttl=55 time=34.951 ms
wrong total length 96 instead of 1280
76 bytes from 172.217.161.228: icmp_seq=1 ttl=55 time=35.473 ms
wrong total length 96 instead of 1280
76 bytes from 172.217.161.228: icmp_seq=2 ttl=55 time=34.116 ms
wrong total length 96 instead of 1280
^C
--- www.google.com ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 33.143/34.767/36.151/1.050 ms
```
このように，1280まではOKだが，1281以上だとパッケージ分割が必要となり，VPNの受付側が分割したパッケージは受け取らんというポリシーの場合には途中で接続が切れるということがある，のかな？

## MTU manual setting on macOS Catalina

1. 「システム環境設定...」>「ネットワーク」>「Wi-Fi」右下の「詳細...」
2. 「ハードウェア」パネルを開く
3. 「構成：」で，「手動」を選択
4. 「MTU：」で，「カスタム」を選択し，適切な値を記入（今の場合は `1280`）


## Conclusions

- `ping`を使ってMTUの上限値（最適値）を調べることができる
- 「システム環境設定」内でwifiのMTU値をカスタム設定できる
- しかし，残念なことに突然接続が切れる問題は解決していない...


## References

1. https://support.purevpn.com/how-to-change-mtu-value-on-mac
2. https://www.chrismacpherson.net/dev/configuring-vpn-mtu/

