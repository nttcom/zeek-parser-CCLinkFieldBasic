spicy_cc_link_basic
=================================

spicy_cc_link_basicとはSpicyで作成したZeekのパーサです。

OsecTで利用されています。

以下の様なログを生成できます。

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
