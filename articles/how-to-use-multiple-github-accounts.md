---
title: "複数のGitHubアカウントを使い分けたい時の設定方法とTips"
emoji: "🐙"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [GitHub]
published: false
---

# 概要

この記事では、複数の GitHub アカウントを使い分ける方法について解説しています。
仕事とプライベートで GitHub アカウントを使い分けたいなと思っている方のお役に立てれば幸いです。

この記事の説明は、「すでに GitHub を使用していて新たに 2 つ目の GitHub アカウントを追加したい」というユースケースにおける説明になっています。

それでは頑張っていきましょう！

## 動作環境

- macOS Big Sur
- M1 チップ
- シェル：zsh 5.8

# 秘密鍵・公開鍵を生成する

新しい GitHub アカウント用に秘密鍵・公開鍵を生成します。
ターミナルを起動し、以下のコマンドを順に実行します。

```shell:~
#.sshディレクトリに移動
% cd ~/.ssh
```

```shell:~/.ssh
#鍵を生成
% ssh-keygen -t rsa
```

既存の GitHub アカウントで公開鍵認証をしている場合は、ホームディレクトリの直下に`.ssh`ディレクトリが存在するはずです。
`ssh-keygen -t rsa`を実行すると、以下のような対話形式 3 つの質問に答えたのち、鍵が生成されます。

```shell:~/.ssh
% ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/(username)/.ssh/id_rsa):id_sub_rsa #既存のファイル名とは異なる任意のファイル名を指定する
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

GitHub を複数アカウントを使用する際には、1 つ目の質問が非常に重要です。
生成する鍵のファイル名は必ず既存のファイル名とは異なる名前をつけなければいけません。

1 つ目の質問では、「`id_rsa`というファイル名で鍵と生成していいですか？」と聞かれてます。
すでに`id_rsa`というファイル名で鍵を生成している場合は、そのファイルの中身が上書きされてしまいます。
そのため、ここでは既存の鍵のファイル名とは異なるファイル名を指定してあげる必要があります。

`id_sub_rsa`などの任意の名前をつけましょう。

パラフレーズの設定は、こだわりがなければスルーして OK です。

鍵が生成されていることを確認しましょう。

```shell:~/.ssh
% ls
id_rsa    id_rsa.pub    id_sub_rsa    id_sub_rsa.pub    known_hosts
```

生成された公開鍵の方を GitHub に渡す作業を忘れずに行っておきましょう。

[こちら](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)の記事が参考になります。

# `config`ファイルに設定を書き込む

`~/.ssh/config`ファイルがない場合は、以下のコマンドで空の`config`ファイルを作成します。

```shell:~/.ssh
% touch config
```

Vim で`config`ファイルに設定を書き込みます。Vim でファイルを開きましょう。

```shell:~/.ssh
% vi config
```

Vim で開いたら以下のように設定を記述します。
わかりやすいように、既存のアカウントを「メインアカウント」、新たに追加したいアカウントを「サブアカウント」としています。

```shell:~/.ssh/config
#メインアカウント
Host github #任意のホスト名
  HostName github.com
  IdentityFile ~/.ssh/id_rsa #メインアカウントの鍵のファイル
  User git
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes

#サブアカウント
Host github-sub #任意のホスト名
  HostName github.com
  IdentityFile ~/.ssh/id_sub_rsa #サブアカウントの鍵のファイル
  User git
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes
```

上記の設定での変数は 2 つです。

`Host`には、任意のホスト名を設定します。これはリモートとの接続を行う際に使用します。
`IdentityFile`には、アカウントごとの秘密鍵の場所を記述します。

これらの設定により、それぞれのアカウントでの公開鍵認証を行うことができます。
記述したら、`:wq`で保存して終了しましょう。

# GitHub との接続を確認する

それぞれの GitHub アカウントとの接続を確認しましょう。
ここで先ほど`~/.ssh/config`ファイルに記述した`Host`名を使用します。

`ssh -T [Host]`とすることで、それぞれのアカウントとの接続を確認できます。

```
#メインアカウントの接続を確認する
% ssh -T github
Hi [メインアカウントのユーザー名]! You've successfully authenticated, but GitHub does not provide shell access.

#メインアカウントの接続を確認する
% ssh -T github-sub
Hi [サブアカウントのユーザー名]! You've successfully authenticated, but GitHub does not provide shell access.
```

このようにそれぞれのアカウントのアカウント名が表示されて、接続がうまくいっていれば OK です。

# アカウントの切り替え

ターミナル上で GitHub のアカウントを切り替える際は、`git config --global`コマンドを使用します。

```shell
% git config --global user.name "[GitHubアカウント名]"
% git config --global user.email "[GitHubのメールアドレス]"
```

毎回これで切り替えるのは面倒なので、関数を作りましょう。

`~/.zshrc`ファイルに関数を書きます。
`vi ~/.zshrc`を実行し、Vim でファイルを開きます。

```shell:~/.zshrc
function gitmain() {
  git config --global user.name "[メインのGitHubアカウント名]"
  git config --global user.email "[メインのGitHubのメールアドレス]"
}

function gitsub() {
  git config --global user.name "[メインのGitHubアカウント名]"
  git config --global user.email "[メインのGitHubのメールアドレス]"
}
```

これで`gitmain`や`gitsub`と実行するだけで、アカウントを切り替えられるようになりました。もちろん関数名は、任意の名前に変えてもらって構いません。

アカウントが切り替わっているかどうかは

```shell
git config user.name
git config user.email
```

で確認することができます。

# リモートと接続する際の注意点

1 つの PC 上で複数の GitHub アカウントを使用している場合は、リモートと接続する際に注意点があります。

リモートとの接続を行う場合は、`~/.ssh/config`で設定した Host 名を用いて接続を行う必要があります。

通常リモートと接続する際は、`git remote add origin git@github.com:<アカウント名>/<リポジトリ名>.git`とします。
しかし、複数アカウントを使用している場合に`git@github.com`としてしまうと、「どっちの GitHub アカウントですか？」となってしまいます。

そこで、`~/.ssh/config`で設定した`Host`名を使用します。

```shell
#メインアカウントのリモートと接続
% git remote add origin git@github:<アカウント名>/<リポジトリ名>.git

#メインアカウントのリモートと接続
% git remote add origin git@github-sub:<アカウント名>/<リポジトリ名>.git
```

`git@[Host名]:~`とすることで、アカウントを区別してリモートと接続ができます。
リモートとの接続の際は気をつけてください。

以上が、GitHub アカウントを使い分ける際の全手順でした。

# 【Tips】ターミナル上でどちらの GitHub アカウントを使用しているか表示する

自分の設定を Tips として紹介します。
2 つのアカウントを切り替えて使用していると、どちらのアカウントを使っているのかわからなくなりそうだったので、常にターミナルに表示するようにしました。

ターミナルの見た目を変更するコードに書き加える方になりますので、詳しく知りたい方は[こちら](https://qiita.com/y-hirako0928/items/30b6f8d0162dbb8ca486)の記事が参考になります。

設定は`~/.zshrc`ファイルに書きます。

```shell:~/.zshrc
export PROMPT="
%F{green}[%~]%f <`git config user.name`>
=> %# "
RPROMPT='%*'
```

この設定を書くことで、ターミナルの見た目を以下の画像のように変更することができます。
![](https://storage.googleapis.com/zenn-user-upload/357f3601b6369907a42f8b78.png)

`` <`git config user.name`> ``という設定によりカレントディレクトリの隣に`<アカウント名>`と表示されるようになります。

このままでは、アカウントを切り替えても`<アカウント名>`の表示が切り替わらないので、先ほど作成したアカウントを切り替える関数にコードを追加します。

```diff shell:~/.zshrc
  function gitmain() {
    git config --global user.name "[メインのGitHubアカウント名]"
    git config --global user.email "[メインのGitHubのメールアドレス]"
+   source ~/.zshrc
  }

  function gitsub() {
    git config --global user.name "[メインのGitHubアカウント名]"
    git config --global user.email "[メインのGitHubのメールアドレス]"
+   source ~/.zshrc
  }
```

アカウントの切り替えと同時に毎回`~/.zshrc`ファイルを読み込ませることで、`<アカウント名>`の表示も切り替わります。

ぜひ試してみてください。

# まとめ

今回は、1 つの PC で GitHub アカウントを使い分ける方法について解説してみました。
仕事でもプライベートでも快適な開発を行っていきたいと思います！

最後まで読んでいただきありがとうございました。それではまた。
