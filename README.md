# PostgREST-installation-Japanese-memo

Postgresはインストールしている前提です。
OSはUbuntu

sudo su
apt update
# 2024-02-03現在 https://github.com/PostgREST/postgrest/releases/ で最新バージョンを確認
wget https://github.com/PostgREST/postgrest/releases/download/v12.0.2/postgrest-v12.0.2-linux-static-x64.tar.xz

# ダウンロードしたアーカイブを解凍
tar -xf postgrest-v12.0.2-linux-static-x64.tar.xz

# ダウンロードしたファイルを削除
rm postgrest-v12.0.2-linux-static-x64.tar.xz

# postgrest.confの設定
echo 'db-uri = "postgres://your_username:your_password@localhost:5432/your_database"' > postgrest.conf

# PostgRESTを起動
./postgrest postgrest.conf

# 動作テスト
http://localhost:3000/
