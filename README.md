# Spring Boot のサンプルプロジェクト

初期設定のログは INIT_WORKLOG.md に記載しています。

## IDE

### IntelliJ IDEA

そのままで Gradle, Spring Boot, Docker のほとんどの機能が使えます。

### VS Code

Dev Container を使用できます。必要な拡張機能が自動で導入され、環境を汚さず作業可能です。

1. VS Code に "Dev Containers" 拡張機能をインストール
2. プロジェクトを開き、`Ctrl+Shift+P`（macOSでは`Cmd+Shift+P`）を押して、"Dev Containers: Reopen in Container" を選択

### Eclipse

使用できません。INIT_WORKLOG.md を参照してください。

## コマンド等

IntelliJ IDEA では Gradle タスクの実行やテストができるため docker compose を無理に使用する必要はありません。以下のコマンドは IDE の支援無しに試す場合に有用です。

### 実行、開発

```shell
docker compose up -d
docker compose exec app ./gradlew bootRun
```
- ポート 8080 で Spring Boot のアプリケーションが起動します。
- ポート 5005 でデバッグ用のポートが開放されます。

### テスト

```shell
docker compose exec app ./gradlew test
```

### Docker イメージのビルド

```shell
docker compose exec app ./gradlew build
docker build --build-arg JAR_FILE=build/libs/\*.jar -t ishikuro.info/sb-ci-lab .
```

# To Do
- [ ] CI/CD パイプラインを作成する
