q
# Zeek-Parser-CCLinkFieldBasic

## Overview

Zeek-Parser-CCLinkFieldBasic is a Zeek plug-in that can analyze CC-Link IE Field Basic of the [CC-Link family](https://www.cc-link.org/ja/cclink/index.html).

## Usage

### Manual Installation

Before using this plug-in, make sure that Zeek, Spicy is installed.

````
# Check Zeek
~$ zeek -version
zeek version 5.0.0

# Check Spicy
~$ spicyz -version
1.3.16
~$ spicyc -version
spicyc v1.5.0 (d0bc6053)

# The path of zeek in this manual is based on the following output
~$ which zeek
/opt/zeek/bin/zeek
````

`git clone` this repository to your local environment.

```
~$ git clone https://github.com/nttcom/zeek-parser-CCLinkFieldBasic.git
~$ cd ~/zeek-parser-CCLinkFieldBasic/analyzer/ 
```

Source code compile and copy the object files to the following path.

```
~$ spicyz -o cc_link_noip.hlto cc_link_noip.spicy cc_link_noip.evt
~$ # cc_link_noip.hlto will be generated
~$ cp cc_link_noip.hlto /opt/zeek/lib/zeek-spicy/modules/
```

Similarly, copy the Zeek file to the following path.

```
~$ cd ~/zeek-parser-CCLinkFieldBasic/scripts/
~$ cp main.zeek /opt/zeek/share/zeek/site/
```

Finally, import the Zeek plugin.

```
~$ tail /opt/zeek/share/zeek/site/local.zeek
... Omit ...
@load cc_link_noip
```

This plug-in generates `cclink-ie.log`.

```
~$ zeek -Cr zeek-parser-CCLinkFieldBasic/testing/Traces/cc_link_ief.pcap local.zeek
```

## Log type and description

This plug-in monitors all functions of CC-Link IE Field Basic and outputs them as `cclink-ie.log`.

| Field | Type | Description |
| --- | --- | --- |
| ts | time | timestamp of the first communication |
| uid | string | unique ID for this connection |
| id.orig_h | addr | source IP address |
| id.orig_p | port | source port number |
| id.resp_h | addr | destination IP address  |
| id.resp_p | port | destination port number   |
| pdu | string | protocol function name |
| cmd | string | cyclic or - |
| number | int | number of packet occurrence |
| ts_end | time | timestamp of the last communication |

An example of `cclink-ie.log` is as follows

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

## Other Software

This plug-in is used by [OsecT](https://github.com/nttcom/OsecT).