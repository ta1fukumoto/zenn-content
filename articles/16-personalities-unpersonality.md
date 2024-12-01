---
title: "性格診断テスト（16Personalities）の相性をチェックして盛り上がろう🥳"
emoji: "🎨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [React, TypeScript, Nextjs]
published: true
---

## はじめに

皆さんは、性格診断テスト 16Personalities をご存知でしょうか？「MBTI 診断」というほうが通りがいいかもしれません。最近ちょっと流行っている性格診断です。（もう最近の範囲は超えているかもしれませんが。）

この度、16Personalities の性格診断結果を使って相性をチェックできるツール「UnPersonality」を作成しました。

https://unpersonality.unreact.jp?from=zenn

16Personalities は、巷では MBTI などの通称で知られています。ちなみに僕は ESTJ（幹部）です。今回作成した「UnPersonality」では、自分と相手の 16 タイプの相性をチェックすることができます。

この記事では、なぜ「UnPersonality」を開発したのか、「UnPersonality」の機能、実装について紹介していきたいと思います。

## そもそも 16Personalities とは？

16Personalities は、個人の性格を 16 種類のタイプに分類する性格診断テストです。MBTI（Myers-Briggs Type Indicator）という性格理論をベースにしており、以下の 4 つの軸で性格を判定します。

- エネルギーの方向：外向型（E） vs 内向型（I）
- 情報の取得方法：現実型（S） vs 直観型（N）
- 判断の仕方：論理型（T） vs 感情型（F）
- 生活態度：計画型（J） vs 探索型（P）

これらの組み合わせによって、ESTJ（幹部）や INFP（仲介者）といった 16 種類の性格タイプが決定されます。各タイプには、その性格の特徴を表す呼び名が付けられています。
たとえば、先ほど私が当てはまると書いた ESTJ は「幹部」という名称がつけられており、

- E（外向型）：外部との関わりからエネルギーを得る
- S（現実型）：具体的な事実や経験を重視する
- T（論理型）：論理的な判断を好む
- J（計画型）：計画的に物事を進めたい

という特徴を持ちます。

16Personalities の診断は、公式サイトで無料で受けることができます。診断は質問に答えていく形式で 10 分程度で終わります。質問の数はちょっと多めですが、質問数が多い分結果は結構信憑性あると思うので、まだやったことないという方は、この機会にぜひやってみてください！面白いですよ 😁

https://www.16personalities.com/ja

:::details 16Personalities と MBTI は違う？
巷では 16Personalities = MBTI として認識しがちですが、実は 16personalities は MBTI 診断とは、別物らしいです。開発にあたって調べていて知りました。

そのため、この記事でいう 16Personalities は MBTI ではない、ということだけ留意していただければと思います。

一応 MBTI 協会の注意喚起を添付しておきます。

> https://www.mbti.or.jp/attention/

また、英語の記事にはなりますが、16Personalities 　公式の説明についても添付しておきます。

> https://www.16personalities.com/articles/our-theory

:::

## 自分と相手の相性をチェックしたい！

弊社では、一時期この 16Personalities の診断が流行ったことがあり、社員やインターン制のデータを集めて、個人の性格や関係性を考察していました。（何をやっているんだという感じですが、みんなそういうの考察するのが好きだったので。）

各タイプごとの関係性や相性については、日本語のブログや海外の文献に目を通して調べました。

16Personalities のデータを集めて思ったのが、自分と相手をスマホでさっとチェックできるツールがあったら、友達とこの話題になった時に盛り上がりそうだなということです。

そこで、システムとしては簡単に作れそうだと思ったので、サクッと開発して見ようという話になりました。

開発に伴って調べてみると、最近ではマッチングアプリで登録することで相性の参考にしたり、企業の面接で聞かれ採用の参考にしたりすることもあるらしいです。知らなかったんですけど、実は弊社でも採用の参考にしたことがありました。

このように意外と相性をチェックする需要はあるんじゃないかなと思っています。

## 16Personalities の相性をチェックできるツール「UnPersonality」

今回作成した 16Personalities の相性をチェックできるツールの正式名称は「UnPersonality」といいます。

https://unpersonality.unreact.jp?from=zenn

「UnPersonality」の機能について紹介していきます。

:::message
機能の紹介の前に、断っておきたいことがあります。
今回、相性チェックするにあたり、様々な文献を見て相性表を作成しました。しかし、これが絶対の相性表という確証はありません。信憑性は高いものになっていますが、あくまでエンタメとして楽しんでいただけると幸いです。
:::

### 相性チェック機能

メインの機能は、自分と相手の 16 タイプを選択して、相性をチェックする機能です。

![](https://storage.googleapis.com/zenn-user-upload/deca68c9240a-20241107.png)

「16 タイプを選択」ボタンをクリックすると、タイプを選択できるダイアログが開きます。

![](https://storage.googleapis.com/zenn-user-upload/f2f5499a6cc3-20241107.png)

自分と相手の 16 タイプを選択した状態で、「相性をチェック」ボタンをクリックします。

![](https://storage.googleapis.com/zenn-user-upload/2e48447909d8-20241107.png)

すると、相性チェックの結果が表示されます。

![](https://storage.googleapis.com/zenn-user-upload/77cdd9459119-20241107.png)

相性は 5 段階になっており、それぞれの相性についての説明文も用意されています。

- 🥳 最高！
- 😊 いい感じ
- 🙂 普通
- 😐 微妙
- 🤮 最悪...

説明文に関しては、いろいろな文献を参考に作成していますが、エンタメ程度で考えていただけると幸いです。文章を読んでみんなでワイワイしていただけると幸いです。

また、チェックしたら X でシェアしていただけると嬉しいです！

### 相性一覧機能

また、相性の一覧を確認するための機能もあります。

導線は改善予定なのですが、以下の URL で書く 16Personalities ごとの相性一覧を確認できます。（ハンバーガーメニューにリンクはあります。）

https://unpersonality.unreact.jp/types

例えば、ESTJ（幹部）の詳細を開くと、ESTJ の詳細情報を見ることができます。

![](https://storage.googleapis.com/zenn-user-upload/7bb756f7d7b7-20241107.png)

各タイプごとの特徴も簡単に載せています。

![](https://storage.googleapis.com/zenn-user-upload/9a9ccfcb5ce9-20241107.png)

スクロールすると、相性の一覧があります。5 段階評価の相性を一覧で確認することができます。

![](https://storage.googleapis.com/zenn-user-upload/fdffa69b01e9-20241107.png)

PC だと以下のように表示されます。

![](https://storage.googleapis.com/zenn-user-upload/6e1b408654c6-20241107.png)

### これから実現したいこと

今後、実現したいと考えていることは以下です。

- ログイン機能
- 自分の周りの人の 16Personalities を登録できる機能
- サイトを回遊させるための導線の整理

現在は、ひとまず相性をチェックする機能だけにフォーカスしてリリースしました。

これからは、ログインできるようにして相性のチェック履歴を残せるようにしたり、自分の周りの人の 16Personalities を登録できるようにしたりしたいと思っています。
また、相性をチェックした後の導線がおざなりなので、その辺は整理したいと考えています。

## 「UnPersonality」のアーキテクチャ

今回のアプリの開発には「T3 Stack」を採用しました。弊社の別のプロダクト（[UnTyping](https://untyping.jp?from=zenn)）で T3 Stack を使ってみていい感じだったので、引き続き
採用しました。ただ、現状はログイン機能は使っておらず、API も特にないです。そのため、ただの Next.js アプリです。（これから T3 Stack が活躍してくれるはず）

T3 Stack の簡単な説明については、[以前の記事](https://zenn.dev/taichifukumoto/articles/typing-game-untyping#untyping-%E3%82%92%E6%94%AF%E3%81%88%E3%82%8B%E6%8A%80%E8%A1%93%E3%82%B9%E3%82%BF%E3%83%83%E3%82%AF)をご覧ください。

## 実装で工夫したところ

### データの高速な取得

16 タイプの説明文や相性データは、DB（データベース）を使用せずにソースコードに直接ハードコーディングする形で実装しました。

```ts:一覧で使用する説明文の持ち方の例
const personalityDescription: {
  [key in PersonalityTypeValue]: {
    main: string;
    friend: string;
    lover: string;
    A: number;
    T: number;
    total: number;
    ranking: number;
  };
} = {
  INTJ: {
    main: "INTJ（建築家）は、戦略的で理性的な頭脳派だ！常に現状に満足せず、改善を追求する性質を持っていて、論理的に考え、計画を実行するのが得意だぞ！自立心が強く、一人で行動するのも得意で、周りに流されずに自分の道を進むことができる。だけど、感情や人間関係に対して少し不器用なところも。ときには冷たく見られることもあるけれど、その厳しさが自分や周囲を向上させ、成功へと導く力になるんだ！困難な状況でも、冷静に戦略的な判断ができるぞ！",
    friend:
      "建築家は、知的で深い関係を築ける友達を求める！表面的な付き合いには興味がなく、知性や誠実さ、自己改善への意欲を持つ人との友情を大事にする。友達とは、精神的に刺激し合い、新しいアイデアや挑戦を一緒に楽しむ関係を築くんだ！友達になったあとは、自主性を尊重しつつ、深い洞察と知的な会話でお互いを高め合う、頼れる存在になるよ！",
    lover:
      "建築家は、戦略的に恋愛に取り組むタイプだ！パートナーには、知的で正直な関係を求め、論理的に恋愛を進めていく。感情的なつながりを築くのはちょっと苦手だけど、パートナーと深い会話が楽しめる知的な関係になれれば、最高のパートナーシップが築けるぞ！",
    A: 1.79,
    T: 1.91,
    total: 3.7,
    ranking: 11,
  },
  INTP: {
    main: "INTP（論理学者）は、ユニークな視野を持つ知的探究者だ！好奇心が強く、想像力も豊かで、いつも頭の中で新しいアイデアや疑問を追求していくぞ！ 論理的に考えて、複雑な問題を解決する才能もバッチリ！でも、人間関係はちょっと苦手で、相手の気持ちにどう対応すればいいか困ることもあるんだ…。ときには考えすぎて行動に移せないこともあるけど、そのおかげで独創的な解決策を見つけ出せる力を持ってる！困難な状況でも、冷静に適切な解決策を導き出すことができるぞ！",
    friend:
      "論理学者は、知的な刺激を与えてくれる友達を求める！新しいアイデア・難しい問題を話すのが大好き！時には自分のアイデアに異議を唱えてくれる人を尊重するぞ！ 多くの友達よりも、少数の気の合う仲間を大切にするタイプで、表面的な会話や人付き合いにはあまり興味がない。友達とは知識を交換し合って、論理的な会話でつながっていくんだ！友達になったら、独創的な視点を与えて、相手の可能性を引き出す頼れる存在になるよ！",
    lover:
      "論理学者は、知的な探究心を持って恋愛にも取り組むよ！パートナーに対しては、知的で刺激的な会話や一緒に成長できる関係を求めるんだ。論理的に物事を考える性格だから、恋愛でも計画的に進めるタイプ。ただ、感情的な部分は少し苦手で、パートナーとの感情的なやりとりに困ることがあるかも。でも、知的でお互いに学び合えるような関係になれれば、素晴らしいパートナーシップを築けるよ！",
    A: 2.67,
    T: 4.52,
    total: 7.19,
    ranking: 3,
  },
  // ...省略
};
```

これにより、DB へのアクセスが不要になり、相性チェックのレスポンスが高速になりました。

### データ構造の工夫

16 タイプのデータは、先ほどの例のように INTJ や INTP のアルファベット 4 文字をキーにしたオブジェクト形式で定義しました。

これにより、反復処理を減らし、より直接的にデータにアクセスできるようにしました。

```ts
// 例えば、以下のように type を受け取ってキーアクセスができる
const obj = personalityDescription[type];
```

## まとめ

16Personalities の相性をチェックできるツール「UnPersonality」を紹介させていただきました。
「UnPersonality」は、16Personalities 診断の結果を使って、2 つの性格タイプ間の相性を手軽にチェックできるツールです。

職場の同僚や友人との会話のネタとして、気軽に使っていただけたら嬉しいです。（相性の判定はあくまでエンタメとして捉えていただければと思います。）

ぜひ「UnPersonality」を使って、周りの人との相性をチェックしてみてください！

https://unpersonality.unreact.jp?from=zenn

最後まで読んでいただきありがとうございました。