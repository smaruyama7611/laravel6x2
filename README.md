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
  
  centosの場合
  phpのstorageおよび.envファイルの権限を変更する。
  
  ####　コンテナの起動とMySQLの接続確認
  それぞれのファイルを追加後、Laravelの設定ファイルを「.env」として作成します。
  その後、docker-compose.ymlファイルのあるdockerディレクトリに移動し、docker-composeでコンテナを起動します。
 ```
 cd laravel
 cp .env.example .env
 cd ..
 docker-compose up -d
 ```
 docker-compose up -dのコマンドを実行すると、各コンテナのイメージも作成されます。

  引用
  https://qiita.com/yoshiplur/items/fa875e111f908cd8786c
