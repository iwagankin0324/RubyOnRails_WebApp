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
```
