# nginx cookbookのpassengerレシピのCentOS7対応のデモ



## 必要なもの

  - [VirtualBox](https://www.virtualbox.org)
  - [Vagrant](https://www.vagrantup.com)
  - [ChefDK](https://downloads.chef.io/chef-dk)

## 起動

```
berks vendor cookbooks
vagrant up
```

## デモプログラム(ruby)

  1. vagrant sshなどで、ゲストOSにログイン

  1. コードをダウンロード、セットアップ

  ```bash
  sudo mkdir -p /var/www/app
  sudo chown vagrant /var/www/app
  cd /var/www/app
  git clone https://github.com/phusion/passenger-ruby-websocket-demo.git
  cd passenger-ruby-websocket-demo
  sudo yum install -y rubygem-bundler
  bundle install
  ```

  1. /etc/nginx/conf.d/passenger.confを編集。末尾に追加

  ```
  server {
          server_name ruby.passenger.local;
          listen 80;
          root /var/www/app/passenger-ruby-websocket-demo/public;
          passenger_enabled on;
          passenger_app_env  development;
  }
  ```

  1. nginx再読み込み

  ```bash
  sudo systemctl restart nginx.service
  ```

  1. ホストOSのhostsファイルを編集

  ```
  192.168.33.10 ruby.passenger.local
  ```

  1. ブラウザでアクセス

  http://ruby.passenger.local


## デモプログラム(nodejs)

  1. vagrant sshなどで、ゲストOSにログイン

  1. コードをダウンロード、セットアップ

  ```bash
  sudo mkdir -p /var/www/app
  sudo chown vagrant /var/www/app
  cd /var/www/app
  git clone https://github.com/phusion/passenger-nodejs-websocket-demo.git
  cd passenger-nodejs-websocket-demo
  npm install
  ```

  1. /etc/nginx/conf.d/passenger.confを編集。末尾に追加

  ```
  server {
          server_name nodejs.passenger.local;
          listen 80;
          root /var/www/app/passenger-nodejs-websocket-demo/public;
          passenger_enabled on;
          passenger_sticky_sessions on;
          passenger_app_env  development;
  }
  ```
  
  1. nginx再読み込み

  ```bash
  sudo systemctl restart nginx.service
  ```

  1. ホストOSのhostsファイルを編集

  ```
  192.168.33.10 nodejs.passenger.local
  ```

  1. ブラウザでアクセス

  http://nodejs.passenger.local


※うまく接続できないときは、一度ゲストOSを再起動すればつながることがあります。
