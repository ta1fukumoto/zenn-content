---
title: "T3 Stackでプログラミングに特化したタイピングゲームを作りました！あなたは何点出せますか？"
emoji: "👨‍💻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Nextjs, T3Stack, TypeScript, tRPC]
published: true
---

## はじめに

T3 Stack でプログラミングに特化したタイピングゲーム「UnTyping」を作成しました。
この記事では、開発に使用した技術スタックについて触れながら、タイピングゲームのどの部分でどの技術を使ったのか解説していきたいと思います。

## 「UnTyping - エンジニア向けプログラミングタイピングゲーム　」とは？

「UnTyping」とは、プログラミングを練習するために特化したタイピングゲームです。エンジニアを主なターゲットにしています。

https://untyping.jp?from=zenn

そもそも、なぜ UnTyping を作ったのかというと、プログラミングのコードベースで遊べるタイピングゲームで「これは面白いな」と思えるものがなかったからです。
タイピングゲームといえば「寿司打」だと思いますが、寿司打ではプログラミングで頻出の記号や数字などが練習できません。そこで、プログラミングのコードベースで練習できるタイピングゲームはないか探してみても、なかなか面白いものがありませんでした。やはり練習を続けるには、タイピングゲームの面白さは必要だと思っています。そこで、自分たちでプログラミングのコードをベースに練習できるプログラミングゲームを作ろう！ということになりました。

使用した技術の説明の前に「UnTyping」がどのようなタイピングゲームなのか触れておきます。

### コース（言語）を選んでタイピングゲームを遊べる

UnTyping では、コースを選んでタイピングゲームを遊ぶことができます。

![](https://storage.googleapis.com/zenn-user-upload/e15a3013ebc5-20240911.png)

現在は、

- JavaScript
- TypeScript
- React
- Python

の 4 コースから選んで遊ぶことができます。コースはこれから追加予定（追加時期未定）です。

ゲームをやるからには目標スコアが必要だと思うので、みなさんどのくらいか遊んでみてください。

#### 目標スコア一覧

**2000 点〜**
通常の社会人レベル

**3000 点〜**
通常のエンジニアが到達できるスコア

**4000 点〜**
タイピングの速いエンジニアが到達できるスコア
4000 点を超えるあたりからタイピングが速い人だと思います

**5000 点〜**
タイピングのかなり速いエンジニアが到達できるスコア
寿司打だと 18,000 円食べられる人くらいの感覚です

**6000 点〜**
寿司打で 20,000 円食べられるか食べられないかくらいの人が到達できるスコア
私はこのくらいのレベルです

**7000 点〜**
寿司打で 20,000 円以上は余裕で食べている人くらいのスコア
世界ランキング首位を争えるレベルです。

**8000 点〜**
まだ誰も到達したことない雲の上のスコア
いつか辿り着きたいです！

### タイピングゲームのスコアを記録できる

UnTyping には、タピイングゲームのスコアを記録できる機能があります。ログインすることで、ゲームのスコアを記録可能です。
ログインすると、マイページでコースごとに記録を確認することができます。グラフで記録を見ることができるので、直感的に成長を実感できます。

![](https://storage.googleapis.com/zenn-user-upload/4751b8596d73-20240911.png)

記録の詳細では、ベストスコアやプレイ時間などを一覧で確認することもできます。

![](https://storage.googleapis.com/zenn-user-upload/1d82ecdfac0d-20240911.png)

### 世界ランキングに参加できる

世界ランキング機能で、ゲームのスコアを競い合うことができます。ログインすると、世界ランキングに参加可能です。世界ランキング上位を目指しましょう！

![](https://storage.googleapis.com/zenn-user-upload/721f5f3dd4b2-20240911.png)

## UnTyping を支える技術スタック

それでは、UnTyping を開発を支える技術スタックについて触れたいと思いっます。

今回のタイピングゲームは「**T3 Stack**」を採用しました。

https://create.t3.gg/

**T3 Stack** は、モダンなウェブアプリケーション開発のための技術スタックの組み合わせです。（T3 Stack の **T3** は、TypeScript、tRPC、Tailwind の頭文字から来ているらしい）

T3 Stack の内訳やそのほか採用した技術について紹介します。

### TypeScript

https://www.typescriptlang.org/

言わずもがなの **TypeScript** です。近年は Web 開発におけるデファクトスタンダードだと思っています。強力な型チェックと VS Code との連携でいつもお世話になっています。

今回のタイピングゲームのコードは、バックエンド・フロントエンド含めて全て TypeScript で開発しています。

### Next.js

https://nextjs.org/

**Next.js** もみなさんご存知かと思います。React をベースとしたフルスタックフレームワークです。もはや Next.js でできないことはないのでは、というほど進化したフロントエンド開発ではファーストピック間違いなしのフレームワークです。

今回ルーティングには、App Router を採用しました。

### tRPC

https://trpc.io/

**tRPC** は、TypeScript を利用した型安全な API を構築するためのツールです。
TypeScript でスキーマやコード生成なしに API を構築し API クライアントを生成することができます。バックエンドとフロントエンドの両方を TypeScript で記述することができて、その両方で型情報を共有することが可能です。このバックエンドとフロントエンドで型情報を共有できるのがめっちゃ便利で、ほとんど型を書く必要がなく推論で型安全に開発することができます。

API（正確には procedure ）の実装から、サーバーサイド・クライアントサイドでのデータの取得・ Mutation まで、すべて tRPC に乗っかって実装しています。バリデーションのスキーマ定義は zod を使っています。zod のスキーマはフロントでも使用しています。

以下は実際の実装の一部です。

```ts:zod の schema 定義
export const createResultSchema = z.object({
  courseId: z.string(),
  score: z.number(),
  numberOfTyping: z.number(),
});
```

```ts:API（procedure）の実装
export const resultRouter = createTRPCRouter({
  // 結果を記録
  create: protectedProcedure.input(createResultSchema).mutation(({ ctx, input }) => {
    const { courseId, score, numberOfTyping } = input;

    const userId = ctx.session.user.id;
    return ctx.db.result.create({
      data: {
        courseId,
        score,
        numberOfTyping,
        createdBy: {
          connect: { id: userId },
        },
      },
    });
  }),

// 省略...
});
```

```ts:フロントでAPIを呼び出す
  // ユーザー名を更新
  const createResult = apiFront.result.create.useMutation();
```

### Tailwind CSS

https://tailwindcss.com/

Tailwind CSS は、ユーティリティファーストの CSS フレームワークです。事前に定義されたクラスを使って、高速でデザインを組み上げることができます。
正直普通の CSS は使いたくなくなるほどに気に入っています。最新の Tailwind CSS であれば、普通の CSS でできることは基本的には再現可能なので使ったことがない方はぜひ取り入れてみてください！

今回のタイピングゲームでもほとんど全てのスタイリングは Tailwind CSS で実装しました。グラフ部分のみ Recharts を使っています。

### Prisma

https://www.prisma.io/

Prisma は、次世代の ORM です。データベース操作を簡素化し、型安全性を確保する強力な ORM です。フロントエンド出身のエンジニアからすると、生の SQL を書く必要がなく直感的に DB 操作を行うことができるのでとても助かります。 （SQL はある程度知っている必要はありますが。）

Prisma Migrate による宣言型データモデリングやマイグレーションシステム、Prisma Studio による DB の GUI 表示・編集もめっちゃ便利です。

たとえば、タイピングゲームの結果を保存するためのテーブルは以下のように prisma で定義しています。

```prisma:schema.prisma
model Result {
    id             Int      @id @default(autoincrement()) // 一意の識別子
    courseId       String // コースID
    score          Int // スコア
    numberOfTyping Int // タイピング回数
    createdAt      DateTime @default(now()) // 作成日時
    createdBy      User     @relation(fields: [createdById], references: [id])
    createdById    String

    @@index([courseId]) // `courseId` フィールドに対するインデックス
    @@index([createdById]) // `createdById` フィールドに対するインデックス
}
```

### NextAuth.js

https://next-auth.js.org/

**NextAuth.js**は、Next.js アプリケーションに簡単に認証機能を追加できるライブラリです。

今回はタイピングゲームのログイン部分に使用しました。
エンジニアがターゲットということで Google によるログインだけにしています。（もはやほとんどのサービスこれだけでいいと思っている）

NextAuth.js で Google ログインを使うのは簡単で、コード上は `authOptions.providers` に以下の設定を追加するだけです。

```ts
export const authOptions: NextAuthOptions = {
  // 省略...

  // GoogleProvider を追加する
  providers: [
    GoogleProvider({
      clientId: env.GOOGLE_CLIENT_ID,
      clientSecret: env.GOOGLE_CLIENT_SECRET,
    }),
  ],
};
```

この設定をするだけで、DB との連携なども隠蔽されておりフロントからログイン・ログアウトする際の実装は以下の関数を使うだけで OK でした。（DB には Account や Session を追加するためのテーブルをあらかじめ用意する必要はあります。）

```ts:ログインの実装
import { signIn } from 'next-auth/react';

// 省略...

const handleClickLogin = () => {
  void signIn('google');
};
```

```ts:ログアウトの実装
import { signOut } from 'next-auth/react';

// 省略...

const handleClickLogout = () => {
  void signOut();
};
```

### Supabase

https://supabase.com/

Supabase とは、Firebase の代替サービスとされているオープンソースの BaaS (Backend As A Service) です。

今回は Supabase の DB 機能を使用しました。PostgresSQL の RDB で、T3 Stack のプロジェクトではよく使われている印象があります。

### Vercel

https://vercel.com/

Vercel は Next.js の開発元であり、モダンなウェブアプリケーションのデプロイと運用に特化したクラウドプラットフォームです。

今回のタイピングゲームのデプロイ先として Vercel を採用しました。Next.js の開発元が運用するシステムなので、Next.js との相性は抜群です。

そのほかにも React Hook Form など細かいライブラリはありますが、UnTyping を支える主な技術は上記の通りです。

## 改善したいところ

ひとまず、リリース前は漕ぎ着けましたが、まだまだ改善の余地がある部分が残っています。

### API のパフォーマンス改善

今回は、初めて tRPC を採用しましたが、普段の API を使用するより重い気がします。

原因が tRPC の実装なのか、Supabase のスペックなのかはまだわかっていません。今回 Supabase のスペックは、1 番安いものにしました。
ゲーム自体に問題はないので重要度は低いですが、原因は知りたいところです。

### SNS でシェアする機能（2024/09/13 にリリース済）

この手のサービスによくある SNS でシェアする機能がまだできていないので、これから実装予定です。
つけようとは思っていますが、あの機能ってどのくらいの方が使われているのでしょうか？🤔

#### 【2024/09/13 追記】

思っていた以上に反響があり、X でも少しだけ盛り上がっているので、X でシェアできる機能を追加しました！
みなさんプレイありがとうございます！いいスコアが出たらぜひポストしてみてください！

![](https://storage.googleapis.com/zenn-user-upload/5181d19eb920-20240913.png)

![](https://storage.googleapis.com/zenn-user-upload/8b3f94c09612-20240913.png)

### procedure の設計

procedure が増えてコードが長くなってしまっているので、ファイルを分けるなりのリファクタリングをしたいです。
tRPC にはエンドポイントという概念がないので、命名も難しく悩ましいところです。

### まとめ

今回は、プログラミングに特化したタイピングゲームを作成した話について、T3 Stack の技術に触れながら紹介してみました。

みなさん、ぜひランキング上位を目指してゲームを遊んでみてください！

https://untyping.jp?from=zenn
