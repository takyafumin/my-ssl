自己証明書作成
==============

自己証明書の作成手順です

* 自己認証局(CA)を用いた自己証明書の作成
* 自己証明書の作成

自己認証局(CA)を用いた自己証明書の作成
--------------------------------------

### 証明書の作成

以下のコマンドを実行し、証明書を作成する
```bash
docker run --rm \
-e SSL_SUBJECT="hoge.example.com" \
-e SSL_DNS="hoge.example.com" \
-v `pwd`:/certs \
paulczar/omgwtfssl
```

作成されるものは以下の通り

* ca-key.pem
* ca.pem
* ca.srl
* cert.pem, サーバ証明書
* key.csr, サーバ証明書要求(CSR)
* key.pem, サーバ証明書秘密鍵
* openssl.cnf
* secret.yaml

### ブラウザへの自己認証局証明書インポート

`ca.pem`をブラウザにインポートする

### 検証用Webサーバ(Dockerコンテナ)起動

```bash
echo "Hello world!" > index.html

docker run --rm -d \
--name www \
-p 80:80 \
-p 443:443 \
-v `pwd`/default.conf:/etc/nginx/conf.d/default.conf \
-v `pwd`/index.html:/usr/share/nginx/html/index.html \
-v `pwd`/cert.pem:/etc/nginx/server.crt \
-v `pwd`/key.pem:/etc/nginx/server.key \
nginx:latest
```
