# はじめに
以下の書籍を勉強するためのリポジトリである。
- https://www.amazon.co.jp/はじめてつくるWebアプリケーション-〜Ruby-Railsでプログラミングへの第一歩を踏み出そう-江森-真由美/dp/4297134683

# 環境構築
MacOSのバージョン確認
```sh
% sw_vers                                                      
ProductName:		macOS
ProductVersion:		14.5
BuildVersion:		23F79
```

シェルの確認
```sh
% echo $SHELL
/bin/zsh
```

Homebrewをインストールするために必要となるコマンドラインツール「Command line tools for Xcode」をインストール  
ただ、本環境ではすでに存在している
```sh
% xcode-select --install
xcode-select: note: Command line tools are already installed. Use "Software Update" in System Settings or the softwareupdate command line interface to install updates
```

Homebrewの確認とインストール  
本環境にはないのでインストールを実施する
```sh
% brew -v
zsh: command not found: brew
```
以下のサイトにあるコマンドを実行する  
https://brew.sh/ja/
```sh
% /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
% brew -v
zsh: command not found: brew
```
バージョンが表示されないので、以下のコマンドを実行して解決させる  
https://qiita.com/TORAPPU/items/b2809083af3cf967d70f
```sh
% (echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/marugan/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
% brew -v
Homebrew 4.3.7
% brew upgrade
```

rbenvをインストール
```sh
% brew install rbenv

% echo 'export PATH="$HOME/.rbevn/bin:$PATH"' >> ~/.zshrc
% echo 'eval "$(rbenv init -)"' >> ~/.zshrc
% source ~/.zshrc

% rbenv -v
rbenv 1.2.0
```

Rubyをインストール
```sh
% rbenv install 3.1.3
==> Installed ruby-3.1.3 to /Users/marugan/.rbenv/versions/3.1.3

% rbenv global 3.1.3
% source ~/.zshrc

% ruby -v
```

Railsをインストール
```sh
% gem install -v 7.0.4 rails
% echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
% source ~/.zshrc
% rails -v                                               
Rails 7.0.4
```

Bundlerをインストール
```sh
% gem install -v 2.3.26 bundler
Fetching bundler-2.3.26.gem
Successfully installed bundler-2.3.26
Parsing documentation for bundler-2.3.26
Installing ri documentation for bundler-2.3.26
Done installing documentation for bundler after 0 seconds
1 gem installed

% bundler -v
Bundler version 2.3.26
```

ImageMagickをインストール
```sh
% brew install imagemagick
% convert
WARNING: The convert command is deprecated in IMv7, use "magick" instead of "convert" or "magick convert"
Version: ImageMagick 7.1.1-34 Q16-HDRI aarch64 22301 https://imagemagick.org
Copyright: (C) 1999 ImageMagick Studio LLC
License: https://imagemagick.org/script/license.php
Features: Cipher DPC HDRI Modules OpenMP(5.0) 

（略）
```

# Railsのお試し
## プロジェクト作成
作業用のディレクトリを作成し、Railsプロジェクトを作成  
そして、Railsルートディレクトリに移動し、サーバーを起動する
```sh
% mkdir myApp
% cd myApp
% rails _7.0.4_ new pdiary
% cd pdiary
% bin/rails server
```
コマンドを起動後に、`http://localhost:3000/`にアクセスする

各ディレクトリの役割を以下に示す
| ディレクトリ名 | 役割 |
| ---- | ---- |
| assets | 画像やCSSを格納するディレクトリ |
| channels | Webサーバとブラウザ間の双方向リアルタイム通信に関するものを格納するディレクトリ |
| controllers | コントローラ（MVCモデルのC）を格納するディレクトリ |
| helper | ビューでの処理を共通にまとめたもの（ヘルパー）を格納するディレクトリ |
| javascript | javascriptを格納するディレクトリ |
| jobs | サーバーで動作する一連の処理（ジョブ）に関するディレクトリ |
| mailers | メール送信機能に関することものを格納するディレクトリ |
| models | モデル（MVCモデルのM）を格納するディレクトリ |
| views | ビュー（MVCモデルのV）を格納するディレクトリ |

# scaffoldで日記投稿画面を作成
Railsのscaffold（スキャフォールド）機能を利用して、日記をデータベースに保存するための準備をする。
以下のコマンドを実行する。
bin/rails generate scaffold 名前 カラム名:データ型 カラム名:データ型 ...

```sh
% bin/rails generate scaffold Idea title:string description:text picture:string published_at:date
      invoke  active_record
      create    db/migrate/20240728092420_create_ideas.rb
      create    app/models/idea.rb
      invoke    test_unit
      create      test/models/idea_test.rb
      create      test/fixtures/ideas.yml
      invoke  resource_route
       route    resources :ideas
      invoke  scaffold_controller
      create    app/controllers/ideas_controller.rb
      invoke    erb
      create      app/views/ideas
      create      app/views/ideas/index.html.erb
      create      app/views/ideas/edit.html.erb
      create      app/views/ideas/show.html.erb
      create      app/views/ideas/new.html.erb
      create      app/views/ideas/_form.html.erb
      create      app/views/ideas/_idea.html.erb
      invoke    resource_route
      invoke    test_unit
      create      test/controllers/ideas_controller_test.rb
      create      test/system/ideas_test.rb
      invoke    helper
      create      app/helpers/ideas_helper.rb
      invoke      test_unit
      invoke    jbuilder
      create      app/views/ideas/index.json.jbuilder
      create      app/views/ideas/show.json.jbuilder
      create      app/views/ideas/_idea.json.jbuilder
```

上記でひとつのモデルに対して、データの削除・更新・参照するための雛形を生成された。
具体的には次が生成されます。
- Ideaをデータベースで管理するためのIdeaテーブルの定義
- Ideaを一覧で見る・投稿する・編集する・削除する・参照するための雛形
- テストする時に必要な雛形

scaffold機能では、雛形の生成するだけで、データベースにIdeaを管理するためのテーブルは作成されていない。
なのえ、生成されたテーブルの定義ファイルを利用して、Ideaを管理するテーブルをデータベース上に次に作成する。

```sh
 % bin/rails db:migrate
== 20240728092420 CreateIdeas: migrating ======================================
-- create_table(:ideas)
   -> 0.0007s
== 20240728092420 CreateIdeas: migrated (0.0007s) =============================
```

次のコマンドを実行して、ブラウザで`http://localhost:3000/ideas/`にアクセスする。
```sh
% bin/rails server
```



