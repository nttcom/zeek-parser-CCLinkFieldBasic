# Zeek-Parser-CCLinkFieldBasic

English is [here](https://github.com/nttcom/zeek-parser-CCLinkFieldBasic/blob/main/README_en.md)

## 概要

Zeek-Parser-CCLinkFieldBasicとは[CC-Linkファミリー](https://www.cc-link.org/ja/cclink/index.html)のCC-Link IE Field Basicを解析できるZeekプラグインです。

## 使い方

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
/opt/zeek/bin/zeek
```

本リポジトリをローカル環境に `git clone` します。

```
~$ git clone https://github.com/nttcom/zeek-parser-CCLinkFieldBasic.git
~$ cd ~/zeek-parser-CCLinkFieldBasic/analyzer/ 
```

ソースコードコンパイルして、オブジェクトファイルを以下のパスにコピーします。

```
~$ spicyz -o cc_link_basic.hlto cc_link_basic.spicy cc_link_basic.evt
~$ # cc_link_basic.hltoが生成されます
~$ cp cc_link_basic.hlto /opt/zeek/lib/zeek-spicy/modules/
```

同様にZeekファイルを以下のパスにコピーします。

```
~$ cd ~/zeek-parser-CCLinkFieldBasic/scripts/
~$ cp main.zeek /opt/zeek/share/zeek/site/
```

最後にZeekプラグインをインポートします。

```
~$ tail /opt/zeek/share/zeek/site/local.zeek
...省略...
@load cc_link_basic
```

本プラグインを使うことで `cclink-ief-basic.log` が生成されます。

```
~$ zeek -Cr zeek-parser-CCLinkFieldBasic/testing/Traces/cc_link_ief.pcap local.zeek
```

## ログのタイプと説明

本プラグインはCC-Link IE Field Basicの全ての関数を監視して`cclink-ief-basic.log`として出力します。

| フィールド | タイプ | 説明 |
| --- | --- | --- |
| ts | time | 最初に通信した時のタイムスタンプ |
| uid | string | ユニックID |
| id.orig_h | addr | 送信元IPアドレス |
| id.orig_p | port | 送信元ポート番号 |
| id.resp_h | addr | 宛先IPアドレス |
| id.resp_p | port | 宛先ポート番号 |
| pdu | string | プロトコルの関数名 |
| cmd | string | cyclic 或は - |
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

## 関連ソフトウエア

本プラグインは[OsecT](https://github.com/nttcom/OsecT)で利用されています。
