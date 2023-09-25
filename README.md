# Zeek-Parser-CCLinkFieldBasic

English is [here](https://github.com/nttcom/zeek-parser-CCLinkFieldBasic/blob/main/README_en.md)

## 概要

Zeek-Parser-CCLinkFieldBasicとは[CC-Linkファミリー](https://www.cc-link.org/ja/cclink/index.html)のCC-Link IE Field Basicを解析できるZeekプラグインです。

## インストール

### パッケージマネージャーによるインストール

このプラグインは[Zeek Package Manger](https://docs.zeek.org/projects/package-manager/en/stable/index.html)用のパッケージとして提供されています。

以下のコマンドを実行することで、本プラグインは利用可能になります。
```
zkg refresh
zkg install zeek-parser-CCLinkFieldBasic
```

### マニュアルインストール

本プラグインを利用する前に、Zeek, Spicyがインストールされていることを確認します。
```
# Zeekのチェック
~$ zeek -version
zeek version 5.0.0

# Spicyのチェック
~$ spicyz -version
1.3.16
~$ spicyc -version
spicyc v1.5.0 (d0bc6053)

# 本マニュアルではZeekのパスが以下であることを前提としています。
~$ which zeek
/usr/local/zeek/bin/zeek
```

本リポジトリをローカル環境に `git clone` します。

```
~$ git clone https://github.com/nttcom/zeek-parser-CCLinkFieldBasic.git
```

## 使い方

### パッケージマネージャーによるインストールの場合

以下のように本プラグインを使うことで `cclink-ief-basic.log` が生成されます。

```
zeek -Cr /usr/local/zeek/var/lib/zkg/clones/package/zeek-parser-CCLinkFieldBasic/testing/Traces/cclink_ief_basic_only.pcap zeek-parser-CCLinkFieldBasic
```

### マニュアルインストールの場合

ソースコードをコンパイルして、オブジェクトファイルを以下のパスにコピーします。

```
~$ cd ~/zeek-parser-CCLinkFieldBasic/analyzer
~$ spicyz -o cc_link_basic.hlto cc_link_basic.spicy cc_link_basic.evt
~$ # cc_link_basic.hltoが生成されます
~$ cp cc_link_basic.hlto /usr/local/zeek/lib/zeek-spicy/modules/
```

同様にZeekファイルを以下のパスにコピーします。

```
~$ cd ~/zeek-parser-CCLinkFieldBasic/scripts/
~$ cp main.zeek /usr/local/zeek/share/zeek/site/cc_link_basic.zeek
```

最後にZeekプラグインをインポートします。

```
~$ tail /usr/local/zeek/share/zeek/site/local.zeek
...省略...
@load cc_link_basic
```

本プラグインを使うことで `cclink-ief-basic.log` が生成されます。

```
~$ cd ~/zeek-parser-CCLinkFieldBasic/testing/Traces
~$ zeek -Cr cclink_ief_basic_only.pcap /usr/local/zeek/share/zeek/site/cc_link_basic.zeek
```

## ログのタイプと説明

本プラグインはCC-Link IE Field Basicの全ての関数を監視して`cclink-ief-basic.log`として出力します。

| フィールド | タイプ | 説明 |
| --- | --- | --- |
| ts | time | 最初に通信した時のタイムスタンプ |
| uid | string | ユニークID |
| id.orig_h | addr | 送信元IPアドレス |
| id.orig_p | port | 送信元ポート番号 |
| id.resp_h | addr | 宛先IPアドレス |
| id.resp_p | port | 宛先ポート番号 |
| pdu | string | プロトコルの関数名 |
| cmd | string | `cyclic` または `-` |
| number | int | パケット出現回数 |
| ts_end | time | 最後に通信した時のタイムスタンプ |

`cclink-ief-basic.log` の例は以下のとおりです。

```
#separator \x09
#set_separator  ,
#empty_field    (empty)
#unset_field    -
#path   cclink-ief-basic
#open   2023-05-27-00-52-06
#fields ts      uid     id.orig_h       id.orig_p       id.resp_h       id.resp_p       pdu     cmd     number  ts_end
#types  time    string  addr    port    addr    port    string  string  int     time
1655284124.953994       CIAp8bugKIZRVpAYk       172.16.134.129  61450   172.16.134.128  61450   cyclicDataRes   -       222     1655284149.499713
1655284124.859924       Ckkc3929guO41BnpSa      172.16.134.128  61450   172.16.134.255  61450   cyclicDataReq   cyclic  222     1655284149.392238
#close  2023-05-27-00-52-06
```

## 関連ソフトウェア

本プラグインは[OsecT](https://github.com/nttcom/OsecT)で利用されています。
