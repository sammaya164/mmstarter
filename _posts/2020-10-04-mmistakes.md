---
title: "#04_JekyllのMinimal Mistakesテーマをインストールする"
categories: "ラズパイ"
---

## Jekyllとは
* ホームページ作成ツールの一種
* データベースやCGIを必要としない(静的)
* Rubyで書かれている
* Markdown記法が使える
* プログラムコードのシンタックスハイライトが使える

## 準備としてBundlerをインストールしておく
1. パッケージを更新しておく

    ```shell
$ sudo apt update
$ sudo apt upgrade
    ```
1. RubyGems(gem)を使えるようにするため、ruby-devをインストールする

    ```shell
$ sudo apt install ruby-dev
    ```
1. Bundlerをインストールする

    ```shell
$ sudo gem install bundler
    ```
1. バージョンを確認する

    ```shell
$ bundler -v
    ```

## JekyllのMinimal Mistakesテーマをインストールする手順

1. 適当な名前のディレクトリを作成する  
   (このディレクトリ内で一つのサイトを作成することになる)

    ```shell
$ mkdir ~/mysite
    ```
1. ディレクトリ内へ移動する

    ```shell
$ cd ~/mysite
    ```
1. Gemfileを編集する  
   (`bundle init`でフォーマットを作成することもできる)

    ```shell
$ vi Gemfile
    ```

    ```ruby
source "https://rubygems.org"
gem "jekyll"
gem "minimal-mistakes-jekyll"
    ```
1. インストールする

    ```shell
$ bundle install
    ```
1. バージョンやインストールされた場所などを確認する

    ```shell
$ bundle info jekyll
$ bundle info minimal-mistakes-jekyll
    ```

## 参考

Minimal Mistakesのインストール方法  
[https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
