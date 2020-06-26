#### ディレクトリ構成

```
docker            
　|－　docker-compose.yml             
　|－　laravel                
　|－　mysql                  　
　|　　　|－　Dockerfile                 
　|　　　|－　my.cnf             
　|　　　∟　sql                 
　|　　　　　∟　init.sql                  
　|－　nginx                  
　|　　　|－　Dockerfile                　
　|　　　∟　　default.conf               
　∟　　php           
      　|－　Dockerfile            　
      　∟　　local.ini            
```                               
  #### Laravelをインストール
  最上位のディレクトリ内で以下のコマンドを実行します。
  ```
  git clone https://github.com/laravel/laravel.git laravel
  ```
  
  #### パッケージのインストール
  作成されたlaravelディレクトリに移動して、コンテナからcomposer installを実行します。
  ```
  cd laravel
  docker run --rm --interactive --tty --volume $PWD:/app composer install
  ```
  このコマンドは、Composerイメージからコンテナを一時的に作成し、Laravelに必要なパッケージをインストールします。
  Laravelディレクトリをコンテナ内にマウントすることで、Composerコンテナがcomposer.jsonファイルを読み込んで利用できるようになっています。
  コンテナは実行後に消滅しますが、ディレクトリをマウン トしているためホストOS上にインストールしたパッケージが残ります。公式のComposerイメージで紹介されている使用法です。
  
  **centosの場合**  
  **phpのstorageおよび.envファイルの権限を変更する。**
  
  ####  コンテナの起動とMySQLの接続確認
  それぞれのファイルを追加後、Laravelの設定ファイルを「.env」として作成します。
  その後、docker-compose.ymlファイルのあるdockerディレクトリに移動し、docker-composeでコンテナを起動します。
 ```
 cd laravel
 cp .env.example .env
 cd ..
 docker-compose up -d
 ```
 docker-compose up -dのコマンドを実行すると、各コンテナのイメージも作成されます。
 
 処理が完了したら、以下のコマンドで確認し、コンテナが立ち上がっていたら成功です。
 ```
 docker-compose ps
 ```
 
 次に、Laravelのアプリケーションキーを設定します。PHPコンテナ内でphp artisanコマンドを実行します。
 ```
 cd php
 docker-compose exec php php artisan key:generate
 ```
 Laravelの設定ファイル.envの「APP_KEY」にキーが設定されます。次のコマンドで設定をキャッシュに反映させます。
 ```
 docker-compose exec php php artisan config:cache
 ```
 http://localhost
 にアクセスして正常に動作しているか確認します。
 
 
####  MySQLの接続確認
最初に、Laravelのデータベースの設定を行います。.envファイルを次のように設定します。
```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=user
DB_PASSWORD=pass
```

DB_HOSTにはコンテナ名であるmysqlと記述します。続いて、config/database.phpファイルを設定します。
「'strict' => false」は、「mysql_native_password」の認証方式を利用するための設定です。
```
'host' => env('DB_HOST', 'mysql'),
'port' => env('DB_PORT', '3306'),
'database' => env('DB_DATABASE', 'laravel'),
'username' => env('DB_USERNAME', 'user'),
'password' => env('DB_PASSWORD', 'pass'),
```

以下のコマンドで設定をキャッシュに反映させます。
```
docker-compose exec php php artisan config:cache
```
PHPコンテナに入り、接続の確認をしていきます。
```
docker exec -it php /bin/bash
```
コンテナに入ったら、以下のコマンドでデータベースのマイグレーションを実行します。
```
php artisan migrate
```
続いてtinkerを起動します。
```
php artisan tinker
```
次のコマンドを実行して、マイグレーションを行ったデータが返ってきたらMySQLとの接続が成功しています。
```
\DB::table('migrations')->get();
```
「Ctrl + C」を押してtinkerを終了し、ロールバックを行います。
```
php artisan migrate:rollback
```
なお、PHPコンテナにログインすると、Dokcerfileで定義したユーザーとしてログインします。rootユーザーではないため、コマンドの実行が制限されています。
rootとしてログインしたい場合は、以下のようにuオプションに0を指定します。
```
docker exec -it -u 0 php /bin/bash
```
以上でDockerによるLaravelの実行環境の構築は終了です。


  引用
  https://qiita.com/yoshiplur/items/fa875e111f908cd8786c
