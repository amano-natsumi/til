# docker-compose
## シェルに入る & docker-compose.yml で設定されているportを開ける

```
docker-compose run --service-ports --rm web bash
```

## 使用していないコンテナを削除
```
docker system prune
docker system prune --volumes // ボリュームも消す
docker system prune -a // キャッシュも消す

```
