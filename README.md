ディレクトリ構成
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

Laravelのインストール
    最上位のdockerディレクトリ内で以下のコマンドを実行します。
    git clone https://github.com/laravel/laravel.git laravel
    dockerディレクトリにlaravelというディレクトリが作成され、Git HubからLaravelのプロジェクトがクローンされます。


パッケージのインストール
    作成されたlaravelディレクトリに移動して、コンテナからcomposer installを実行します。
    cd laravel
    docker run --rm --interactive --tty --volume $PWD:/app composer install

  このコマンドは、Composerイメージからコンテナを一時的に作成し、Laravelに必要なパッケージをインストールします。
  Laravelディレクトリをコンテナ内にマウントすることで、Composerコンテナがcomposer.jsonファイルを読み込んで利用できるようになっています。コンテナは実行後に消滅しますが、ディレクトリをマウン トしているためホストOS上にインストールしたパッケージが残ります。公式のComposerイメージで紹介されている使用法です。
