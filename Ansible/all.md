# サーバーごとに1台づつ逐次実行

```
# serial: (最大同時実行台数)
- hosts: webs
  serial: 1
```

# 変数を確認
```
ansible {hostグループ名} -m debug -a "var=nginx_include_config_dir" -i inventories/beta
```
