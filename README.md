# PostgREST-installation-Japanese-memo  

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

#postgrest.confの設定   
nano postgrest.conf  
#####################  
#入力内容は    
#基本的なデータベース接続情報  
#your_username: PostgreSQLのユーザー名  
#your_password: PostgreSQLのパスワード  
#localhost: データベースが動作しているホスト名  
#5432: データベースのポート番号  
#your_database: 接続先のデータベース名  
db-uri = "postgres://your_username:your_password@localhost:5432/your_database"  
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
