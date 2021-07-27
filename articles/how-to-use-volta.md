---
title: "Node.jsのバージョン管理にVoltaを推したい"
emoji: "⚡️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [volta, node, npm, javascript]
published: true
---

# Volta とは

![](https://storage.googleapis.com/zenn-user-upload/7451975bdb29712ddd32766b.png)

**Volta**とは、**JavaScript ツールマネージャー**です。
タイトルでは Node.js のバージョン管理としていますが、 npm・yarn のバージョン管理も行うことができます。

[公式サイト](https://volta.sh/)では、「The Hassle-Free JavaScript Tool Manager（手間のかからない JavaScript ツールマネージャー）」と紹介されています。

チームの Node.js のバージョン管理を Volta に統一したところ非常に DX が上がり、Volta の恩恵を感じています。
この記事では、開発者の Volta 人口を増やすべく Volta の紹介と使用方法について解説していきたいと思います。

なかなか詳しく解説している日本語のソースはないので、公式ドキュメントを読んで適宜和訳しています。
公式ドキュメントを読むのがめんどくさいという人に読んでいただいて Volta を導入していただけると嬉しいです。

それではいきましょう！

# Volta の概要

Volta の 3 つの特徴を見てみましょう。
公式サイトで紹介されている 3 つの特徴についてみていきます。

![](https://storage.googleapis.com/zenn-user-upload/2477ecd909b7465b5ad543d0.png)

## ⚡️ 速い｜ Fast

> Install and run any JS tool quickly and seamlessly! Volta is built in Rust and ships as a snappy static binary.
> （あらゆる JS ツールを迅速かつシームレスにインストールして実行できます。Volta は Rust で作られており、洗練された静的なバイナリとして出荷されます。）

Volta は Rust 製であるため、他のバージョン管理ツールより動作が高速です。
速いのはいいことですね。

## ⚡️ 堅実｜ Reliable

> Ensure everyone in your project has the same tools—without interfering with their workflow.
> （ワークフローを妨げることなく、プロジェクトに参加する全員が同じツールを使えるようにする。）

これが 1 番恩恵を感じている部分です。
`volta pin`というコマンドを使うことで、簡単にプロジェクトメンバーの Node.js や npm のバージョンを揃えることができます。
使用方法については記事の後半で詳しく解説していますので、今は一旦置いておきます。

## ⚡️ ユニバーサル｜ Universal

> No matter the package manager, Node runtime, or OS, one command is all you need: `volta install`.
> （パッケージマネージャ、Node ランタイム、OS を問わず、必要なのは`volta install`という 1 つのコマンドだけです。）

Windows・Mac・Linux。どの OS でも作動します。
Volta はみんなが使っていないとその力を発揮しませんので、どの OS でも動くことは大事なことです。

# イントロダクション｜ Introduction

[公式のイントロダクション](https://docs.volta.sh/guide/)をみていきます。
基本的には公式ドキュメントの和訳です。

## 特徴｜ Features

特徴についてはさらっといきます。

- スピード ⚡️
- プロジェクト単位でのシームレスなバージョン切り替え
- Windows およびすべての Unix シェルを含む、クロスプラットフォームのサポート
- 複数のパッケージマネージャのサポート
- 安定したツールのインストール（ノードのアップグレード毎に再インストールする必要はありません）
- サイト固有のカスタマイズのための拡張性フック

## なぜ Volta を使うのか？｜ Why Volta?

> Volta は、Node エンジンを一度選択すれば、あとは気にする必要はありません。プロジェクトを切り替えても、Node を手動で切り替える必要はありません。定期的に再インストールしたり、なぜ動作しなくなったのかを調べたりすることなく、ツールチェーンに npm パッケージバイナリをインストールすることができます。

### Node エンジンをすばやくセットアップして切り替える

> 特定のバージョンのノードをフェッチして使用できます。

```shell:Terminal
$ volta install node@14
```

> ツールの応答性にすぐに気付くはずです。あなたの開発時間は貴重です！ JavaScript 開発者はきびきびとしたツールに値します。 🙂

確かにインストールは高速だと感じました。

### 共同作業者のための再現可能な環境

> Volta を使用すると、1 つのコマンドでプロジェクトのノードエンジンとパッケージマネージャーを選択することができます。

```shell:path/to/your/project
$ volta pin node@12
```

> Volta は Node エンジンの正確なバージョンを package.json に保存しているので、選択したものを git にコミットすることができます。それ以降は、プロジェクト・ディレクトリ内で Node を実行するたびに、Volta は自動的にあなたが選択したバージョンと同じバージョンの Node に切り替わります。同様に、あなたの共同研究者もそれぞれの開発マシンに Volta をインストールすることで同じことができます。

これが Volta の素晴らしい点ですよね。
全員が Volta を使っていることが条件ですが、環境作った人が`volta pin`コマンドでバージョンを固定するだけで、そのほかのメンバーは何も考えずに`npm install`とかを実行できるわけです。
バージョンを確認する手間が省けるのは、非常にありがたいです。

### インストール、そして忘れる

> Volta では、お気に入りのパッケージ・バイナリをコマンドライン・ツールとしてインストールすることができ、開発プロジェクトに支障をきたす心配がありません。さらに良いことに、これらのツールはインストール時に特定の Node エンジンに固定され、あなたが明示的に指示しない限り変更されることはありません。これは、一度動作したツールは動作し続けることを意味します。

```shell:Terminal
$ npm install -g surge
$ surge -h
```

このようにグローバルインストールしたものも、プロジェクトごとにバージョンが記憶され、バージョンを気にする必要がなくなります。

# Volta をインストールしよう ｜ Getting Started

それでは Volta をインストールしましょう。

## 一旦 Node.js・npm を削除する

インストールする前に、現在使用中の Node バージョン管理ツールを削除し、一旦 Node・npm を使用できない状態にする方が望ましいです。
少し怖いかもしれませんが、勇気を出して削除していきましょう。

ツールの削除の部分に関してはパターンが多すぎる（nodebrew, NVM, nodist, ...）ので、削除方法については割愛させていただきます。ご自身の環境に合わせて調べてみてください！

> 私は nodebrew を使用していたので、[こちら](https://offlo.in/blog/node-version-nvm.html)の記事を参考に削除を行いました。

## Volta をインストールする

[公式の Getting Started](https://docs.volta.sh/guide/getting-started)に従いインストールを行います。
OS は Mac・Linux（WSL 含む）であれば、以下のコマンドで簡単にインストール可能です。

以下のコマンドを実行しましょう。

```shell:Terminal
$ curl https://get.volta.sh | bash
```

コマンドは`| bash`となっていますが、zsh・fish を使用されている方でも問題なくインストール可能です。そのまま実行しましょう！
自動でパスも通してくれるのでありがたいです。

Windows の方は、Windows インストーラーが用意されているのでダウンロードして実行し指示に従います。

:::message
Windows の場合、Volta の使用にあたって必要な設定があるようなので[公式サイト](https://docs.volta.sh/guide/getting-started)の指示に従って設定を行いましょう。
:::

## インストールされたことを確認する

`.zshrc`ファイルなどのシェルの設定ファイルを`cat`コマンドなどで覗いてみて、以下のパスが書き込まれていれば OK です。

```shell:~/.zshrc
︙
export VOLTA_HOME="$HOME/.volta"
export PATH="$VOLTA_HOME/bin:$PATH"
```

一度ターミナルを再起動し、`volta`コマンドが使えるかどうかを確認します。

```shell:Terminal
$ volta --version

> 1.0.4
```

Volta のバージョンが表示されればインストールは完了です！

# Volta を理解しよう｜ Understanding Volta

> Volta の仕事は、`node`、`npm`、`yarn`などの JavaScript コマンドラインツールや、JavaScript パッケージの一部として提供される実行ファイルを管理することです。
> パッケージマネージャーと同様に、Volta はあなたのカレントディレクトリに基づいて、あなたがどのプロジェクトで作業しているかを追跡します。Volta のツールチェーンに含まれるツールは、あなたが特定のバージョンのツールを使用しているプロジェクトにいることを自動的に検出し、あなたにとって最適なバージョンのツールへのルーティングを行います。

改めて、Volta は JavaScript コマンドラインツールのバージョン管理ツールであることが書かれています。
何度も書いていますが、Volta は自動でバージョンの切り替えを行ってくれます。

## ツールチェーンの管理｜ Managing your toolchain

> Volta ツールチェーンによって管理されるツールは、`volta install`と`volta uninstall`の 2 つのコマンドで制御します。

と書かれていますが、`uninstall`は挙動が微妙なようです。基本的には`install`だけで事足ります。

### Node エンジンのインストール

> ツールチェーンにツールをインストールするは、インストールされたバージョンはそのツールの*デフォルトバージョン*となります。別のバージョンを使用するように Node のバージョンを固定したプロジェクトディレクトリ内で作業している場合を除き、デフォルトで Volta が使用するバージョンになります。

> たとえば、特定のバージョンをインストールすることで、Node のデフォルトバージョンを選択できます。

```shell:Terminal
# 特定のバージョンをインストールする
$ volta install node@14.15.5
```

> 正確なバージョンを指定しなかった場合は、Volta はリクエストに一致する適切なバージョンを選択します。

```shell:Terminal
# 14系の安定板がインストールされる
$ volta install node@14
```

> `latest`で最新版をインストールすることもできます。また、バージョンを完全に省略すると、Volta は最新の LTS リリースを選択しインストールします。

```shell:Terminal
# 最新バージョンをインストールする
$ volta install node@latest

# NodeのLTSリリースがインストールされる
$ volta install node
```

> これらのコマンドのいずれかを実行すると、PATH 環境（または Windows の Path）で Volta によって提供されるノード実行可能ファイルは、デフォルトで選択したバージョンのノードを自動的に実行します。

> 同様に、`volta install npm`および`volta install yarn`をそれぞれ使用して npm および Yarn パッケージマネージャーのバージョンを選択できます。これらのツールは、選択したノードのデフォルトバージョンを使用して実行されます。

同様に npm や Yarn もインストール可能です。

## プロジェクトの管理｜ Managing your project

> Volta を使用すると、共同作業者のチームまたはコミュニティのプロジェクトに使用する開発ツールを標準化できます。

### Node エンジンの固定

> `volta pin`コマンドを使用すると、プロジェクトの Node エンジンとパッケージマネージャーのバージョンを選択できます。

```shell:Terminal
# Nodeのバージョンを固定
$ volta pin node@14.17

# npmのバージョンを固定
$ volta pin npm@6.14
```

> Volta はこれを`package.json`に保存するため、選択したツールをバージョン管理にコミットできます。

```json:package.json
"volta": {
  "node": "14.17.3",
  "npm": "6.14.13"
}
```

> このようにして、Volta を使用してプロジェクトに取り組むすべての人が、選択したものと同じバージョンを自動的に取得します。

```shell:Terminal
node --version # 14.17.3
npm --version # 6.14.13
```

つまり、`volta pin`コマンドで、プロジェクトのバージョンを指定することでき、Volta の設定が記述された`package.json`をチームで共有することで、全員の Node や npm、Yarn のバージョンを揃えることができるというわけです。

### プロジェクトツールの使用

> ツールチェーン内のスマートツールは、Node とパッケージマネージャーの実行可能ファイルだけではありません。ツールチェーン内のパッケージバイナリも現在のディレクトリを認識し、現在のプロジェクトの構成を尊重します。

> たとえば、Typescript パッケージをインストールすると、コンパイラの実行可能ファイル（`tsc`）がツールチェーンに追加されます。

```shell: Terminal
$ npm install --global typescript
```

> 参加しているプロジェクトに応じて、この実行可能ファイルはプロジェクトで選択されたバージョンの TypeScript に切り替わります。

```shell:Terminal
cd /path/to/project-using-typescript-3.9.4
tsc --version # 3.9.4

cd /path/to/project-using-typescript-4.1.5
tsc --version # 4.1.5
```

このように、Node や npm・Yarn などのパッケージマネージャーだけではなく、それを介してインストールするパッケージバイナリも Volta の監視対象です。そのため、自動でプロジェクトごとにバージョンを切り替えてくれます。
これって結構革新的じゃないですか？

### 安全性と利便性

> Volta のツールチェーンは、あなたがどこにいるのかを常に把握しています。そのため、あなたが使うツールはあなたが作業しているプロジェクトの設定を常に優先します。つまり、プロジェクトを切り替えてもインストールされているソフトウェアの状態を変更する心配はありません。

> さらに、Volta はツールを実行するたびにそのトラックをカバーし、npm または Yarn スクリプトがツールチェーンの内容を決して認識しないようにします。

> これら 2 つの機能を組み合わせることで、Volta はグローバルパッケージの問題を解決します。言い換えれば、Volta はグローバルパッケージインストールの利便性を提供しますが、危険はありません。

> 例えば、`npm i -g typescript`で TypeScript を安全にインストールし、コンソールから直接`tsc`を呼び出せる便利な機能を楽しむことができます。

> Enjoy!

グローバルインストールですら、安全に使用することができるようになると言っていますね。
すごいよ、Volta。

# Volta のコマンド｜ Volta Commands

`volta`コマンドラインバイナリのリファレンスは以下の通りです。（公式コマンドリファレンスは[こちら](https://docs.volta.sh/reference/)）

```shell
# 利用方法
volta [FLAGS] [SUBCOMMAND]

# FLAGS
    --verbose   # 詳細な診断を有効にします
    --quiet     # 不要な出力を防ぎます
-v, --version   # Voltaの現在のバージョンを表示します
-h, --help      # ヘルプ情報を表示します


# SUBCOMMANDS
fetch          # ローカルマシンにツールをフェッチ（取り込み）します
install        # ツールをツールチェーンにインストールします
uninstall      # ツールをツールチェーンからアンインストールします
pin            # プロジェクトのランタイムやパッケージマネージャーを固定します
list           # カレントツールチェーンを表示します
completions    # Voltaコンプリートを生成します
which          # Voltaが呼び出す実際のバイナリを特定します
setup          # 現在のユーザー/シェルに対してVoltaを有効にします
help           # メッセージまたは指定されたサブコマンドのヘルプを表示します
```

## Volta のよく使うコマンドの紹介

最後によく使うコマンドについて紹介しておきます。

### `volta install`

`volta install`は、ツールのデフォルトバージョンを設定します。ローカルに指定されたバージョンがキャッシュされていない場合は、そのツールのフェッチから行います。
使い方は`volta install [FLAGS] <tool[@version]>`です。

インストールの際の、バージョンの指定方法は上記でも説明しましたが、以下にまとめておきます。

```shell:Terminal
# 特定のバージョンを指定してインストール
$ volta install node@14.17.3

# 特定のメジャーバージョンの安定板をインストール
$ volta install node@14

# ツールのLTSをインストール
$ volta install node

# 最新版をインストール
$ volta install node@latest
```

### `volta pin`

`volta pin`コマンドは、選択したバージョンのツールを使用するようにプロジェクトの`package.json`ファイルを更新します。
このコマンドは、Node やパッケージマネージャー（npm・Yarn）にのみ使うことができます。
使い方は、`volta pin [FLAGS] <tool[@version]>`です。

```shell:Terminal
# Nodeのバージョンを固定
$ volta pin node@14.17

# npmのバージョンを固定
$ volta pin npm@6.14 # or volta pin yarn@1.19
```

上記の`volta pin`コマンドをプロジェクトディレクトリで実行すると、`package.json`に以下の設定が書き込まれます。

```diff json:package.json
{
  ...,
+ "volta": {
+   "node": "14.17.3",
+   "npm": "6.14.13" //or "yarn": "1.19.2"
+ }
}
```

この`package.json`を GitHub などを使ってチームで共有することで、全員が同じバージョンの Node や npm を使用することができます。

例えば上記の設定が書かれた`package.json`があるプロジェクトで`npm -v`を実行するとします。
ローカルマシンに`6.14.13`のバージョンの npm がキャッシュされている場合は、`6.14.13`と表示されます。
ローカルマシンにキャッシュされていない場合は、`6.14.13`のインストールから始まり、インストールが終わると`6.14.13`と表示されます。

このように、ローカルにそのバージョンを持っていればそのバージョンに自動で切り替わり、持っていなければバージョンのインストールから始まります。

これが非常に便利で、バージョンを気にせずに`npm install`を実行しても勝手にバージョンが揃うため、`package-lock.json`に差異が生じなくなります。（npm の 6 系と 7 系では、`package-lock.json`の内容が大きく変わります。）

もちろん個人のバージョン管理にも非常に便利なのですが、この恩恵を最大限に受けるにはチームのメンバー全員に Volta を入れてもらわないといけないんですよね。
チームメンバーに Volta の良さを語って乗り換えてもらいましょう！

### `volta list`

`volta list`コマンドは、インストールされている Node ランタイム、パッケージマネージャーおよびバイナリを含むパッケージを検査し表示します。
使い方は`volta list [FLAGS] [OPTIONS] [tool]`です。

以下に`volta list`コマンドの使い方をまとめます。

```shell
# 利用方法
volta list [FLAGS] [OPTIONS] [tool]

# Flags
-c, --current # 現在のアクティブなツールを表示します
              # このフラッグがデフォルトです
-d, --default # デフォルトツールを表示します
    --verbose # 詳細な診断を有効にします
    --quiet   # 不要な表示を防ぎます
-h, --help    # ヘルプ情報を表示します

# OPTIONS
--format <format>   # 出力形式を指定します
                    # 有効な値は `human` or `plain` です
                    # デフォルトは `human` で、それ以外の場合は `plain` です

# ARGS
<tool>  # リスト表示したいツールを指定します（node, npm, yarn またはその他のバイナリ）
        # 全て表示したい場合は `all` を指定します
```

よく使うのは、シンプルな`volta list`とか、`volta list all`とかですね。
`volta list`で、そのプロジェクトで使用されるツールのバージョンを確認できます。
`volta list all`では、Volta で管理しているツールを一覧で見ることができます。

# まとめ

結構な長文になってしまいましたが、みなさんに Volta の魅力が伝わればと思い執筆しました。
JavaScript をメインに開発をしている方にとっては最適解だと思っています。

Volta は使う人が増えるほど、その恩恵を享受できるものだと考えているので、ぜひみなさんの開発チームでも導入を検討してみてください。

最後まで読んでいただきありがとうございました。ではまた。
