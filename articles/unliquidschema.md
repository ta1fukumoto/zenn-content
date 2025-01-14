---
title: "Shopifyのセクション開発を楽にするWebアプリを開発しました【UnLiquidSchema】"
emoji: "💧"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Nextjs, T3Stack, tRPC, Liquid]
published: true
---

## はじめに

この度、Shopify のセクション開発サポートツール「UnLiquidSchema」を作成しました。

https://unliquidschema.unreact.jp/?from=zenn

「UnLiquidSchema」は、Shopify のセクション開発の際に Liquid の schema をノーコードで生成できるツールです。
この記事では、「UnLiquidSchema」の機能やリリースした背景、実装について紹介していきたいと思います。

## Shopify のセクション開発で感じていた課題

弊社では、普段から Shopify のセクションやアプリを Liquid で開発する機会が多くあります。
その際、以下のようなの課題を感じていました。

### schema の settings はうろ覚え

Shopify のセクション開発で肝になるのが、Liquid の schema の settings 定義です。実際にセクションを開発する際に、実現したいセクションの設定項目は頭にあるのですが、正確な settings の記述方法はうろ覚えであることが多いです。

例えば、schema の settings の type やその type で必要な設定項目（プロパティ）はなんとなくは覚えているけど、正確に書こうと思うと調べたりコピペしたりというようなイメージです。（まあ覚えればいいんですけど。）

### JSON エラーが起きた時に見つけにくい

Shopify 管理画面のテーマのコード編集のエディタで作業する際の課題で、schema 内の JSON でエラーが起きた際にエラーの箇所が特定しづらいです。VSCode などを使って作業している場合は、そこまで問題ではないです。しかし、ちょっとした修正であれば、直接テーマのコード編集でやっちゃうので、そういう際に schema の JSON でエラーが起きると、コンマの位置などの間違い探しをする羽目になります。

## UnLiquidSchema とは

今回、上記の課題を解決できるツールとして「UnLiquidSchema」を開発してリリースしました。

https://unliquidschema.unreact.jp/?from=zenn

「UnLiquidSchema」を使うことで、Shopify のセクション開発で必要となる schema の settings をノーコードで生成することができます。

「UnLiquidSchema」の主な特徴は以下の通りです。

- GUI 操作で直感的に settings を作成できる
- type を選択すると、type に応じた入力項目が自動で表示される
- ログインすることで過去のコードを履歴として残せる

スクリーンショットと一緒に、UnLiquidSchema の使い方について紹介していきます。

## UnLiquidSchema の使い方

使い方を詳しく説明していきます。[UnLiquidSchema](https://unliquidschema.unreact.jp/?from=zenn) を開くと、以下の画面が表示されます。

![](https://storage.googleapis.com/zenn-user-upload/63a0d408f619-20250113.png)

### コードを生成する

メインの機能であるコード生成の流れは以下の通りです。

1. type を選択する
2. type に応じた設定項目を入力する
3. コードを生成する

コード生成についてさらに詳しく手順を紹介します。

「settings 定義」で、schema の settings を作成してコードを生成します。
まず、「type」を選択します。「type」はオートコンプリート形式になっているので、文字を入力して type を絞り込むこともできます。

![](https://storage.googleapis.com/zenn-user-upload/2a5dc9429ed3-20250113.png)

type を選択すると、type に応じた設定項目の入力欄が表示されます。任意の値を入力します。

![](https://storage.googleapis.com/zenn-user-upload/2c0fbae6bc36-20250113.png)

「settings を追加」で、settings を増やすことができます。

![](https://storage.googleapis.com/zenn-user-upload/380f818c99c7-20250113.png)

設定したい settings の入力が完了したら、「コードを生成」ボタンをクリックします。

![](https://storage.googleapis.com/zenn-user-upload/ab081d56e7bd-20250113.png)

必須項目の入力が問題なければ、settings のコードが生成されます。

![](https://storage.googleapis.com/zenn-user-upload/ad4fe218ca47-20250113.png)

「コードをコピー」ボタンで生成された settings のコードをクリップボードにコピーすることができます。あとは、コピーしたコードを手元の Liquid ファイルに貼り付けるだけです。

![](https://storage.googleapis.com/zenn-user-upload/7e9d599fbd2b-20250113.png)

基本的な使い方は以上です。

コード生成の際に便利な機能として、「元に戻す・やり直す」や「定義をリセット」も用意しています。

![](https://storage.googleapis.com/zenn-user-upload/af845e257bf9-20250113.png)

また、settings の順番は矢印ボタンで入れ替えることができます。

![](https://storage.googleapis.com/zenn-user-upload/abb83e38dddb-20250113.png)

### コード生成履歴

もう一つのメイン機能として、「settings コード履歴」があります。ログインすることで、settigns のコード生成を履歴として保存することができます。コード履歴から、過去に生成したコードをコピーしたり、過去のコードを元に新規コードを作成したりすることができます。

![](https://storage.googleapis.com/zenn-user-upload/71bccb94f872-20250113.png)

ハンバーガーメニュー内の「ログイン」からログインできます。

![](https://storage.googleapis.com/zenn-user-upload/6ace6e87bcfd-20250113.png)

今のところ、Google ログインのみ対応しています。

![](https://storage.googleapis.com/zenn-user-upload/4f38f2422457-20250113.png)

## UnLiquidSchema の実装

今回のアプリの開発には「T3 Stack」を採用しました。弊社の別のプロダクト（[UnTyping](https://untyping.jp/?from=zenn) や [UnSchedule](https://unschedule.unreact.jp/?from=zenn)）で T3 Stack を使ってみていい感じだったので、引き続き採用しています。

T3 Stack の簡単な説明については、[以前の記事](https://zenn.dev/taichifukumoto/articles/typing-game-untyping#untyping-%E3%82%92%E6%94%AF%E3%81%88%E3%82%8B%E6%8A%80%E8%A1%93%E3%82%B9%E3%82%BF%E3%83%83%E3%82%AF)をご覧ください。

## まとめ

今回は、Shopify のセクション開発をサポートするツール「UnLiquidSchema」を紹介させていただきました。

https://unliquidschema.unreact.jp/?from=zenn

Liquid でセクション開発をされる方に、使っていただけれると嬉しいです！（リリースしたばかりなんので不具合があった場合はご連絡ください。）

最後までご覧いただきありがとうございました。
