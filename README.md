# pullreq

tokuten-aws
====

## Overview
会員特典環境構築及び各種ツールのリポジトリです。

## Description
主に開発環境構築（Mac）の手順について記載しています。  
「Install」「事前設定」は、それぞれについて未実施の方が実施してください。

## Install
* XCodeをインストール
    * App Storeで検索してインストール、その後起動すること。
* javaをインストール
    * <https://www.java.com/ja/download/index.jsp>
* Virtual Boxをインストールする。（これが仮装マシン）
    * <https://www.virtualbox.org/wiki/Downloads>
* Vagrantをインストールする。（仮装マシンに開発環境を構築するもの）
    * <http://www.vagrantup.com/downloads.html>
* HomeBrewをインストールする。（ソフトウェアの導入を単純化するもの）
    * `` [local]$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" ``
* ターミナルで以下のコマンドを実行する。
    * `` [local]$ brew install git ``
* rbenv導入
    * `` [local]$ brew uninstall ruby-build ``
    * `` [local]$ brew uninstall rbenv ``
    * `` [local]$ brew uninstall ruby ``

    * `` [local]$ brew install rbenv ruby-build ``
    * `` [local]$ eval "$(rbenv init -)"  ``
    * `` [local]$ echo 'eval "$(rbenv init -)"' >> ~/.zshrc ``

    * `` [local]$ rbenv install 1.9.3-p545 ``
    * `` [local]$ rbenv global 1.9.3-p545 ``
    * `` [local]$ rbenv rehash ``

    * `` [local]$ which ruby ``
 >/Users/hoge/.rbenv/shims/ruby

## 事前設定
* Git SSH公開鍵設定
    * `` [local]$ ssh-keygen -t rsa `` 
* 上記で作成した公開鍵をGitに設定する。
    * `` [local]$ cat ~/.ssh/id_rsa.pub | pbcopy ``で公開鍵がコピーされるので、下記に貼り付けする
    * <https://github.com/settings/ssh>


### ■ 開発環境の構築実施
ローカルの開発環境に仮装マシンを入れて、そこでソフトウェアを動かすようにします。  
これにより、他の人の作業に影響を与えずに開発を行えるようになります。

1. tokuten-awsをcloneする。（Git リポジトリのコピーを取得すること）  
（/path/toは任意のフォルダを指しています。各々でパスを読み替えて下さい。）
    * `` [local]$ mkdir -p /path/to ``
    * `` [local]$ cd /path/to ``
    * `` [local]$ ssh-add ~/.ssh/id_rsa ``
    * `` [local]$ git clone git@github.com:mediba-system/tokuten-aws.git ``

2. サブモジュールの初期化
    * `` [local]$ cd /path/to/tokuten-aws ``
    * `` [local]$ git submodule init ``
    * `` [local]$ git submodule update ``

3. 各サブモジュールを最新の状態にする
    * `` [local]$ cd /path/to/tokuten-aws/kittyhawk ``
    * `` [local]$ git checkout master ``
    * `` [local]$ cd /path/to/tokuten-aws/coupy ``
    * `` [local]$ git checkout master ``

4. vagrant-omnibusをインストール
    * `` [local]$ vagrant plugin install vagrant-omnibus ``

5. vagrantを起動
    * `` [local]$ cd /path/to/tokuten-aws/kittyhawk ``
    * `` [local]$ vagrant up ``

6. vagrant　プロビジョニング
    * `` [local]$ vagrant provision ``

* MySQLインストールの際、エラーが出た場合はゲストOSで以下を実行した後、  
ホストOSで再度プロビジョニング
    * `` [local]$ vagrant ssh``
    * `` [vagrant]$ sudo yum install -y mysql-server --enablerepo=remi``
    * `` [vagrant]$ source /etc/profile.d/rbenv.sh``
    * `` [vagrant]$ exit``

7. ホストOSのhostsファイルを編集  
    * `` [local]$ sudo vi /etc/hosts`` 

 >&#035; 0821資産  
 >20.0.0.11       local.tokuten.auone.jp  
 >20.0.0.11       api.local.tokuten.auone.jp  
 >20.0.0.11       apc.local.mediba.jp  

 >&#035; CDN  
 >20.0.0.11       cdn-img.local.auone.jp  
 >20.0.0.11       cdn.local.tokuten.auone.jp  

 >&#035; coupy  
 >20.0.0.11       cms.local.tokuten.mediba.jp  


8. ゲストOS（仮装マシン）のhostsファイルを編集  
  ※ターミナルを別で開く    
    * `` [local]$ vagrant ssh``
    * `` [vagrant]$ sudo vi /etc/hosts ``

 >127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4  
 >::1         localhost localhost.localdomain localhost6 localhost6.localdomain6  

 >127.0.0.1       local.tokuten.auone.jp  
 >127.0.0.1       api.local.tokuten.auone.jp  
 >127.0.0.1       apc.local.mediba.jp  

 >&#035; CDN  
 >127.0.0.1       cdn-img.local.auone.jp  

 >&#035; coupy  
 >127.0.0.1       cms.local.tokuten.mediba.jp  

9. ゲストOSのmysqlでDB作成

    * `` [vagrant]$ mysql -uawsuser -pawspassword cms < /var/www/coupy/doc/ddl/cdb.cms.ddl``
    * `` [vagrant]$ mysql -uawsuser -pawspassword event < /var/www/coupy/doc/ddl/edb.event.ddl``
    * `` [vagrant]$ mysql -uawsuser -pawspassword resource < /var/www/coupy/doc/ddl/rdb.resource.ddl``
  
    * `` [vagrant]$ mysql -uawsuser -pawspassword cms < /var/www/coupy/doc/dml/cdb.cms.dml``
    * `` [vagrant]$ mysql -uawsuser -pawspassword event < /var/www/coupy/doc/dml/edb.event.dml``
    * `` [vagrant]$ mysql -uawsuser -pawspassword resource < /var/www/coupy/doc/dml/rdb.resource.dml``

10. ゲストOSの初期化処理
    * ``[vagrant]$ cd /var/www/coupy/coupy/protected/commands/shell``
    * ``[vagrant]$ sudo sh -x init.sh ``

11. vagrantのリロード、ホストOSのディレクトリの権限変更
    * ``[local]$ vagrant reload ``
    * ``[local]$ chmod -R 777 /path/to/tokuten-aws/coupy/coupy/assets ``

12. ホストOSのVagrantfileの56行目あたりに追記
    * ``[local]$ vi  /path/to/tokuten-aws/Vagrantfile``

 >config.vm.synced_folder ".", "/vagrant", :owner=>"apache", :group=>"apache"

13. 画面が表示されればOK  
<http://local.tokuten.auone.jp>（ユーザ：md／パスワード：1206）  
<http://cms.local.tokuten.mediba.jp>（ユーザ：kitada@mediba.jp／パスワード：kitada）  


### その他  
#### vagrantコマンド  
* 起動  
    * ``vagrant up``  

* 状態確認    
    * ``vagrant status``  

* 停止
    * ``vagrant halt``

* 破棄
    * ``vagrant destroy``

* 仮想マシンにログイン
    * ``vagrant ssh``

#### DBのスキーマ変更

* _開発チームは、スキーマ更新が実施された場合は、制作チームに共有をお願いします。_

1. coupyリポジトリを最新版に更新
```
[local]$ cd /path/to/tokuten-aws/coupy
[local]$ git checkout master
[local]$ git pull origin master
```
2. 仮想マシンにログイン
```
[local]$ vagrant ssh
```
3. スキーマを適用
```
[vagrant]$ mysql -u root resource < /vagrant/coupy/doc/ddl/rdb.resource.ddl
```
4. データを投入
```
[vagrant]$ mysql -u root resource < /vagrant/coupy/doc/dml/rdb.resource.dml
```
5. スキーマキャッシュをクリア
```
[vagrant]$ sudo service httpd restart
```

<dl>
<dt><li></li>abc</dt>
<dd>ABC</dd>
</dl>
