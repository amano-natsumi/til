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

## Spring
### profiles
```
-Dspring.profiles.active config/application-drone-test.yml
```

```
    @Autowired
    private lateinit var environment: Environment
environment.activeProfiles
```

```
    @Test
    public void aaa() {
        byte[] key = new byte[32];
        new Random().nextBytes(key);
        System.out.println(new String(Hex.encodeHex(key)));
    }
```

### [WARN] (DispatcherServlet.java:1251) No mapping for GET /  のログを無視する
```yaml
logging.level.org.springframework.web.servlet.PageNotFound: ERROR
```
