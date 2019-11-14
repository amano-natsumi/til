## log
```
@slf4j
```

をつけたクラスに　log インスタンスがセットされる


## Jackson
jsonをプリント

```
ObjectMapper mapper = new ObjectMapper()
        .enable(SerializationFeature.INDENT_OUTPUT);

String json = mapper.writeValueAsString(prefStation);

System.out.println(json);
```
