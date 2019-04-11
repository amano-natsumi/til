# 文字列を暗号化

```ruby
require 'openssl'
# 暗号化するデータ
data = ''

# パスワード
pass = 'pass'

# salt
salt = OpenSSL::Random.random_bytes(8)

# 暗号化器を作成する
enc = OpenSSL::Cipher.new('AES-256-CBC')
enc.encrypt

# 鍵とIV(Initialize Vector)を PKCS#5 に従ってパスワードと salt から生成する
key_iv = OpenSSL::PKCS5.pbkdf2_hmac_sha1(pass, salt, 2000, enc.key_len + enc.iv_len)
key = key_iv[0, enc.key_len]
iv = key_iv[enc.key_len, enc.iv_len]

# 鍵とIVを設定する
enc.key = key
enc.iv = iv

encrypted_data = ''
encrypted_data << enc.update(data)
encrypted_data << enc.final

# バイト列の状態なので、base64でエンコードする
Base64.strict_encode64(encrypted_data) + '#' + Base64.strict_encode64(salt)


# 復号
data = Base64.strict_decode64(encrypted_data)
salt = Base64.strict_decode64(salt)

dec = OpenSSL::Cipher.new("AES-256-CBC")
dec.decrypt

key_iv = OpenSSL::PKCS5.pbkdf2_hmac_sha1(pass, salt, 2000, dec.key_len + dec.iv_len)
key = key_iv[0, dec.key_len]
iv = key_iv[dec.key_len, dec.iv_len]

dec.key = key
dec.iv = iv

# 復号化する
decrypted_data = ""
decrypted_data << dec.update(data)
decrypted_data << dec.final

p decrypted_data

```


## Faraday でクライアント証明書 を使用する

```ruby
f = Faraday.new(url: "https://example.bom") do |faraday|
    faraday.request :url_encoded
    faraday.adapter Faraday.default_adapter
end

p12 = OpenSSL::PKCS12.new(File.read(Rails.root.join('.key', 'test.p12')), 'password')
connection = f
connection.ssl.client_cert = p12.certificate
connection.ssl.client_key = p12.key

response = connection.get
```
