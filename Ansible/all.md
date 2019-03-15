# サーバーごとに1台づつ逐次実行

```
# serial: (最大同時実行台数)
- hosts: webs
  serial: 1
```
