# 初期設定の作業ログ

公式のガイド
https://spring.io/guides/gs/spring-boot-docker

https://start.spring.io/
Project: Gradle - Kotlin
Language: Java
Spring Boot: 3.4.1
Packaging: Jar
Java: 17
Dependencies:
- Spring Web
- Spring Boot DevTool

ダウンロードした zip をプロジェクトルートに配置

試しにビルドしてみる
```shell
./gradlew build
java -jar build/libs/sb-ci-lab-0.0.1-SNAPSHOT.jar # Spring Boot の実行が確認できる
```

イメージ作成
```shell
./gradlew bootBuildImage --imageName=ishikuro.info/sb-ci-lab
```

実行
```shell
docker run --rm -p 8080:8080 ishikuro.info/sb-ci-lab
```

docker compose 化する。Dockerfile の ARG, ENTRYPOINT は実際の開発作業時は使われなくなる。(デプロイ時には使われるはず)

docker-compose.yml
```yaml
services:
  app:
    build: .
    ports:
      - "8080:8080"
      - "5005:5005"
    volumes:
      - .:/app
    working_dir: /app
    environment:
      - SPRING_PROFILES_ACTIVE=dev
      - JAVA_TOOL_OPTIONS=-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005
    command: ./gradlew bootRun
    tty: true
    stdin_open: true
```

JAVA_TOOL_OPTIONS は公式ガイドの "Debugging the Application in a Docker Container" に記載されている。

----

# Eclipse で開発に関する考察

Eclipse でモダンな開発は難しいかもしれない。

- Gradle は Kotlin を推奨しているが、Eclipse では Kotlin プラグインがメンテナンスされておらず現行版は動作しない。
- Docker Plugin は Eclipse ではメンテナンスされていないようで、docker compose も GUI では操作できない。
- Spring Boot との統合が弱い。ホットリロード、プロパティ補完、REST API のテスト機能などクリティカルな部分が使いにくい。

モダン Java 開発では Spring Boot, Gradle, Kotlin, Docker を使うことが多い。いずれも IntelliJ IDEA ではデフォルトでサポートしている。VS Code もある程度適応できる。
