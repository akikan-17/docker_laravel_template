# docker_laravel_template

```bash:git clone
$ git clone https://github.com/akikan-17/docker_laravel_template.git
```

## Docker環境の再構築
***
> 上記でgitからクローン出来たら該当ディレクトリに移動し、dockerでbuild
```bash:
$ cd docker_laravel_template
$ docker compose up -d --build
```

> buildできたらappコンテナへ移動しvendorディレクトリへライブラリ群をインストール
```bash:
$ docker compose exec app bash
[app] $ composer install
```

> composer install時は.env環境変数ファイルは作成されないため.env.exampleを元にしてコピー
```bash:
[app] $ cp .env.example .env
```

> .envにAPP_KEY=の値が無いため、アプリケーションキーを生成
```bash:
[app] $ php artisan key:generate
```

> public/storageからstorage/app/publicへのシンボリックリンクを張る
```bash:
[app] $ php artisan storage:link
```

> storage、bootstrap/cacheはフレームワークからファイル書き込みが発生するため、書き込み権限を付与
```bash:
[app] $ chmod -R 777 storage bootstrap/cache
```

> 最後にマイグレーションを実行
```bash:
[app] $ php artisan migrate
```

> appコンテナから抜ける
```bash:
[app] $ exit
```

[http://127.0.0.1:8080へアクセス](http://127.0.0.1:8080/)  
Laravelの初期画面が表示されればOK
