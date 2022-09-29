# AWSフルコース第3回 2022/09/24

---

## 課題
- APサーバ、DBサーバ、構成管理ツールの名前とバージョン

| 種類 | 項目名 | バージョン |
| --- | --- | --- |
| APサーバ | Puma | 8.0.30 |
| DBサーバ | MySQL | 8.0.30 |
| 構成管理ツール | Bundler | 2.3.22 |

- APサーバを終了させる -> URLにアクセスできない
```
Ctrl + Cを入力
```

- APサーバを再開させる -> URLにアクセスできる。
```
bundle exec rails s
```

- DBサーバを終了させる -> DBにアクセスできない
```
sudo service mysqld stop
```

- DBサーバを再開させる -> DBにアクセスできる。
```
sudo service mysqld start
```

- DBサーバ状態確認
```
sudo service mysqld status
```

---

## Rails環境構築
1. サンプルコードのクローン

```
git clone https://github.com/yuta-ushijima/raisetech-live8-sample-app.git
```

2. MySQLのインストール
  - CLoud9のEC2インスタンスの容量追加
    - https://blog.proglus.jp/4574/
  - yumが使えればLinux、aptが使えればUbuntu
  - GPGキーの有効期限切れ対策
    - https://blog.katsubemakito.net/mysql/mysql-update-error-gpg

3. database.ymlを環境に合わせて書き換える
```
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: 'root'
  password: 'パスワード' # ここを書き換える

development:
  <<: *default
  database: raisetech_live8_sample_app_development
  socket: /var/lib/mysql/mysql.sock  # ここを書き換える
  #socket: /tmp/mysql.sock

```

4. 構成管理ツールbundlerのインストール
```
gem install bundler
```

5. 必要なGemのインストール
```
bundle install
```

6. DBを作成、マイグレーションを実行して起動する
```
bundle exec rails db:create
bundle exec rails db:migrate
bundle exec rails s
```

7. Blocked Hostのエラーが出るのでdevelopment.rbに下記を追記。追記するホスト名はエラー画面に表示されているものをコピーする。

config/environment/development.rb
```
config.hosts << "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.vfs.cloud9.ap-northeast-1.amazonaws.com"
```

8. webpackのエラーが出たら下記コマンドを実施。yarnが入って無ければyarnもインストール
    - https://qiita.com/NaokiIshimura/items/8203f74f8dfd5f6b87a0

```
npm install -g yarn
bundle exec rails webpacker:install 
```

9. もう一度APサーバを起動する
```
bundle exec rails s
```

