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
