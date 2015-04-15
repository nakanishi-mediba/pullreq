# 【表示確認】


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

    ** bashの場合は `echo 'eval "$(rbenv init -)"' >> ~/.bash_profile`

    * `` $ rbenv install 1.9.3-p545 ``
    * `` $ rbenv global 1.9.3-p545 ``
    * `` $ rbenv rehash ``

    * `` $ which ruby ``
 >/Users/hoge/.rbenv/shims/ruby
