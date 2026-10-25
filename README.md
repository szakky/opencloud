# OpenCloud on Proxmox VE

## 💡 選定理由

従来運用していた Nextcloud（PHP製）において報告された深刻な PHP の脆弱性問題をきっかけに、PHP に依存しないストレージサービスへの移行を検討しました。

調査の結果、Go 言語で開発されている **OpenCloud (opencloud-eu)** を採用しました。

- **PHP 脆弱性の影響を受けない**  
  PHP ランタイムを使用しないため、PHP 特有の脆弱性リスクを根本から回避できます（[公式発表](https://opencloud.eu/de/news/kritische-php-sicherheitsluecke-opencloud-ist-nicht-betroffen)）。
- **軽量・高速なパフォーマンス**  
  Go 言語のマイクロサービス構成により、従来の PHP 製（ownCloud / Nextcloud）と比較してメモリ消費量が少なく、非常に高速に動作します。

## 🛠 環境・技術スタック

- **ホスト OS**: Proxmox VE 8.x
- **ゲスト OS**: Ubuntu Server 22.04 LTS (Unprivileged LXC)
- **コンテナ実行環境**: Docker Engine / Docker Compose
- **アプリケーション**: OpenCloud (`opencloudeu/opencloud:latest`)

## 🚀 起動手順

`docker-compose.yaml` が配置されているディレクトリ（`/opt/opencloud`）で実行します。

```bash
# 1. データ・設定用ディレクトリを作成
mkdir -p opencloud-data opencloud-config

# 2. 所有権をコンテナ実行ユーザー (UID/GID 1000) に変更
# ※ OpenCloud コンテナ内プロセス (UID 1000) にホスト側の書き込み権限を与えるため
chown -R 1000:1000 opencloud-data opencloud-config

# 3. コンテナを起動
docker compose up -d
```

## 🔍 動作確認とログ

```bash
# コンテナのステータス確認
docker compose ps

# ログの確認
docker compose logs -f
```
