# Spring Boot のサンプルプロジェクト

初期設定のログは INIT_WORKLOG.md に記載しています。

## Eclipse で開発

File > Import > Git > Repositories from GitHub
"ishikuro-shunsuke/sb-ci-lab" > Search > Finish


## 実行、開発

```shell
docker compose up -d
```
- ポート 8080 で Spring Boot のアプリケーションが起動します。
- ポート 5005 でデバッグ用のポートが開放されます。

## テスト

```shell
docker compose exec app ./gradlew test
```

## Docker イメージのビルド

```shell
docker compose exec app ./gradlew build
docker build --build-arg JAR_FILE=build/libs/\*.jar -t ishikuro.info/sb-ci-lab .
```

# To Do
- [ ] CI/CD パイプラインを作成する