# PostgREST-installation-Japanese-memo  

## インストール  
Postgresはインストールしている前提です。  
OSはUbuntu  

sudo su  
apt update  
#2024-02-03現在 https://github.com/PostgREST/postgrest/releases/ で最新バージョンを確認  
wget https://github.com/PostgREST/postgrest/releases/download/v12.0.2/postgrest-v12.0.2-linux-static-x64.tar.xz  

#ダウンロードしたアーカイブを解凍  
tar -xf postgrest-v12.0.2-linux-static-x64.tar.xz  

#ダウンロードしたファイルを削除  
rm postgrest-v12.0.2-linux-static-x64.tar.xz  

### postgrest.confの設定   
nano postgrest.conf  
#####################  
#https://postgrest.org/en/stable/tutorials/tut0.html#step-4-create-database-for-api  
#入力内容は    
#基本的なデータベース接続情報  
#your_username: PostgreSQLのユーザー名  
#your_password: PostgreSQLのパスワード  
#localhost: データベースが動作しているホスト名  
#5432: データベースのポート番号  
#your_database: 接続先のデータベース名  
db-uri = "postgres://your_username:your_password@localhost:5432/your_database"  
#PostgRESTが使用するデータベースのスキーマを指定します。  
#db-schemas = "<your_exposed_schema>"
#スキーマ名 "public" の例  
db-schema = "public"  
#匿名ユーザーのロール名を指定します。  
#ロールは適切な権限を設定  
#非推奨だがロール名はユーザー名で全権移譲でもＯＫ!  
db-anon-role = "<your_anon_role>"  
#サーバーがリッスンするポート  
#1つのデータベースにつき1ポート必要です  
server-port = 3000  
#####################


#PostgRESTを起動  
./postgrest postgrest.conf  

#動作テスト  
http://localhost:3000/  

#停止方法  
PostgRESTが実行されているターミナルで、Ctrl + Cを押してプロセスを中断  

## デーモン登録（作成中）  
https://postgrest.org/en/stable/integrations/systemd.html  

### PostgRESTの設定ファイルの作成  
# postgrest.confの設定を参照  
#/etc/postgrest/config へ設置  
### サービスファイルの作成  
#/etc/systemd/system/postgrest.service　へ設置  
[Unit]  
Description=REST API for any PostgreSQL database  
After=postgresql.service  

[Service]  
User=postgrest  
Group=postgrest  
ExecStart=/bin/postgrest /etc/postgrest/config  
ExecReload=/bin/kill -SIGUSR1 $MAINPID  

[Install]  
WantedBy=multi-user.target  

## 強制停止  
調子が悪い時は強制停止。。。  
私の使っている激安、貧弱サーバーの場合はお世話になり事が多い。。。  
pgrep postgrest  
kill <ID>  

それでもダメならPostgreSQLを再起動  
systemctl restart postgresql  
