---
title: "Dockerで作る完全な開発環境インフラ管理ツールの紹介"
emoji: "🐳"
type: "tech"
topics: ["docker", "infrastructure", "devops", "開発環境", "makefile", "zennfes2025infra"]
published: true
---

# Dockerで作る完全な開発環境インフラ管理ツールの紹介

こんにちは！私はぶんぶんです。
今日は、私が作った開発環境のインフラ管理ツールについて紹介したいと思います。

## なぜこのツールを作ったのか？

私も最初は開発のたびに、データベースやキャッシュ、検索エンジンなどのサービスを一つずつセットアップしていました。でも、これがとても大変で：

- 毎回同じ作業を繰り返す
- 設定ファイルを忘れる
- 環境の違いでエラーが起きる
- チームメンバーと環境が違う

これらの問題を解決するために、Dockerを使った統一的なインフラ管理ツールを作りました！

## このツールでできること 🎯

### データベースサービス
- **PostgreSQL 15** - メインのデータベース
- **MySQL 8.0** - バックアップ用のデータベース  
- **MongoDB 7** - ドキュメントデータベース

### キャッシュサービス
- **Redis 7** - 高速なメモリキャッシュ

### 検索サービス
- **Elasticsearch 8.11** - 全文検索エンジン
- **Kibana 8.11** - データの可視化ツール

### ストレージサービス
- **MinIO** - ファイル保存用のオブジェクトストレージ

### メッセージキュー
- **RabbitMQ 3** - 非同期処理用のメッセージキュー

## 私が作ったツールの特徴 ✨

### 1. Makefileで簡単管理
全部の操作をMakefileで統一しました：

```bash
# 全部のサービスを起動
make start

# データベースだけ起動
make start-db

# サービスを停止
make stop

# ログを確認
make logs
```

### 2. データの永続化
Dockerのデータボリュームを使って、データが消えないようにしました。コンテナを再起動してもデータは残ります。

### 3. ネットワーク設定
全部のサービスが同じネットワークで動くので、サービス名でアクセスできます：
- PostgreSQL: `postgres:5432`
- Redis: `redis:6379`
- Elasticsearch: `elasticsearch:9200`

## 実際の使い方 📋

### 1. プロジェクトをクローン
```bash
git clone https://github.com/webbleen/infra.git
cd infra
```

### 2. 全部のサービスを起動
```bash
make start
```

### 3. サービスが動いているか確認
```bash
make status
```

### 4. ログを確認
```bash
make logs
```

### 5. データベースに接続
```bash
# PostgreSQLに接続
make shell-postgres

# MySQLに接続  
make shell-mysql

# MongoDBに接続
make shell-mongodb
```

## 私が困ったことと解決方法 💡

### 1. ポートの競合
最初は、既に動いているサービスとポートが重複してエラーになりました。
**解決方法**: 使っていないポートを確認して、設定を変更しました。

### 2. データが消える
Dockerコンテナを削除すると、データも一緒に消えてしまいました。
**解決方法**: Dockerのデータボリュームを使って、データを永続化しました。

### 3. 設定ファイルが複雑
最初は設定ファイルが複雑で、何が何だか分からなくなりました。
**解決方法**: Makefileを使って、コマンドを統一して分かりやすくしました。

## クラウドでのデプロイ 🚀

### Railwayでのデプロイ
Railwayというクラウドプラットフォームでも使えるようにしました：

- ✅ **MinIO** - ファイル保存
- ✅ **RabbitMQ** - メッセージキュー
- ⚠️ **Elasticsearch** - 有料プランが必要
- ⚠️ **Kibana** - 有料プランが必要

### 他のクラウドでも使える
- **AWS**: RDS、ElastiCache、S3など
- **Google Cloud**: Cloud SQL、Memorystore、Cloud Storageなど
- **Azure**: Azure Database、Redis Cache、Blob Storageなど

## 私の開発ワークフロー 🔄

### 1. 新しいプロジェクトを始める時
```bash
# インフラサービスを起動
make start

# 必要なサービスだけ起動
make start-db
```

### 2. 開発中
```bash
# ログを確認
make logs

# データベースに接続して確認
make shell-postgres
```

### 3. 開発が終わった時
```bash
# サービスを停止
make stop

# データをバックアップ
make backup-postgres
```

## まとめ

このツールを作ることで、私の開発環境のセットアップがとても楽になりました。チームメンバーも同じ環境で開発できるようになって、バグの原因が環境の違いということがなくなりました。

特に、Makefileでコマンドを統一したことで、誰でも簡単に使えるようになったのが良かったです。

みなさんも、開発環境で困ったことがあったら、ぜひ試してみてください！分からないことがあったら、コメントで気軽に聞いてくださいね。

---

**参考リンク**
- [プロジェクトのGitHub](https://github.com/webbleen/infra)
- [私のZennプロフィール](https://zenn.dev/webbleen)
- [Docker公式ドキュメント](https://docs.docker.com/)
