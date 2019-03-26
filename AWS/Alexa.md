# Alexa

## ASK
### install

```
npm install -g ask-cli
```

### init

```
ask init
```

### GUIで作成したスキルのマニフェストファイルを修正

```
ask api get-skill -s skill_id
ask api update-skill -s skill_id -f ./tmp.json
ask api get-skill-status -s skill_id
```

## プロアクティブAPI
https://developer.amazon.com/ja/docs/smapi/proactive-events-api.html
- イベント受信用lambda を作成
- マニフェストjson更新  
この項目を追加

```
"permissions": [
  {
    "name": "alexa::devices:all:notifications:write"
  }
],
"events": {
  "publications": [
    {
      "eventName": "AMAZON.MessageAlert.Activated"  // 使いたいスキーマ
    }
  ],
  "subscriptions": [
    {
      "eventName": "SKILL_PROACTIVE_SUBSCRIPTION_CHANGED"  // 受信したい通知
    }
  ],
  "endpoint": {
    "uri": "arn:aws:lambda:us-east-1:xxx:function:xxx:Release_0"  // 作成したlambda arn
  }
}
```
このコマンドを使う `ask api update-skill`
- プロアクティブAPIを使う場合は一度スキルを削除して再度追加すると、通知サブスクライブがonになる
