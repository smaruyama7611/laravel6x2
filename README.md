        
 #ディレクトリ構成

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
                               
  
  最上位のdockerディレクトリ内で以下のコマンドを実行します。
  ```
  git clone https://github.com/laravel/laravel.git laravel
  ```                              

  参考
  https://qiita.com/yoshiplur/items/fa875e111f908cd8786c
