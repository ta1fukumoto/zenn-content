---
title: "Googleカレンダーのスケジュールから「以下の日程でご都合いかがでしょうか？」を自動生成するアプリを作りました"
emoji: "📅"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Nextjs, T3Stack, TypeScript, tRPC, Googleカレンダー]
published: true
---

## はじめに

この度、Google カレンダーと連携したご都合いかがでしょうかメーカー「UnSchedule」を作成しました。

https://unschedule.unreact.jp/?from=zenn

このアプリを使うと、Google カレンダーの空きスケジュールを取得して、以下のような日程調整用のテキストを生成することができます。

```
以下の日程でご都合いかがでしょうか。
10月25日（金）10:00〜18:00
10月28日（月）10:00〜14:00, 16:00〜18:00
10月29日（火）10:00〜18:00
```

この記事では、アプリを作成した背景やアプリの機能、アプリの実装についてなどを紹介したいと思います。

## なぜ「以下の日程でご都合いかがでしょうかメーカー」を開発したのか

弊社では、予定の管理には Google カレンダーを使っています。
社外とのやりとりをする際に、日程調整をする機会がちょくちょくあり、その際は Google カレンダーの空きスケジュールを確認して、「以下の日程でご都合いかがでしょうか？〜」テキストを作成して、チャットなどで提案しています。

日程調整の機会がそこまで多くないこともあり、TimeRex や Google カレンダーの予約スケジュール機能など、日程調整を楽にしてくれるアプリや機能は使っていません。個人的な感想もあるのですが、日程調整ツールの URL を送ると、少し偉そうだなと感じてしまい気が引けます。特に関係値があまりない社外とのやり取りの際は、テキストで提案する方が印象がいい気がします。（本当に個人の感想ですが。）

そういう話を社内でしていた時に、Google カレンダーの空きスケジュールを確認してテキストを作成するだけなら簡単にできそうだし、あったらシンプルに嬉しいなということで開発に取りかかりました。

## 以下の日程でご都合いかがでしょうかメーカーでできること

今回作成したアプリの正式名称は、「UnSchedule」といいます。

https://unschedule.unreact.jp/?from=zenn

「UnSchedule」の機能について紹介します。

基本的には、カレンダーの日付を選択するだけのシンプルな UI です。

![](https://storage.googleapis.com/zenn-user-upload/594eaec70b93-20241024.png)

アプリの機能を流れで説明します。

Google カレンダーから空きスケジュールを取得する必要があるので、まず Google アカウントでログインします。

![](https://storage.googleapis.com/zenn-user-upload/62edbcbf1355-20241024.png)

ログインが完了すると、すぐに使い始めることができます。

提案したい日程をカレンダーから選択します。

![](https://storage.googleapis.com/zenn-user-upload/db3dffcd35b0-20241024.png)

選択すると、選択した日付の予定を取得します。
予定の取得が完了すると、日程調整用テキストが自動で作成されます。作成されたテキストは編集することができるので、細かい調整も可能です。

![](https://storage.googleapis.com/zenn-user-upload/4cc6552a36d3-20241024.png)

「テキストをコピー」ボタンでテキストをクリップボードにコピーできます。

![](https://storage.googleapis.com/zenn-user-upload/310bc5d32a39-20241024.png)

テキストをコピーできたら、あとはチャットなどで相手にテキストを送信するだけです。

日程調整用テキストを作成するにあたり、就業時間設定のオプションも用意しています。

「始業時間・就業時間」を設定することで、指定した就業時間内で提案する時間を作成します。また、「休日設定」で土日祝を提案する日程に含めるかどうかを設定できます。

![](https://storage.googleapis.com/zenn-user-upload/27d72f890b0e-20241024.png)

以下の機能は、今回のリリースでは削りました。

- 複数の期間を設定する機能
- 昼休みなど固定の時間を除外するオプション
- 固定文「以下の日程でご都合いかがでしょうか。」のデフォルト変更オプション

不要な日付はざっくり指定した後に削除すれば良いし、昼休みなど固定で除外したい時間があるなら Google カレンダーに登録しておけば良いので、とりあえず削りました。固定文は、チャットで送る時に校正できるので、ひとまず削りました。

## アプリの実装

今回のアプリの開発には「T3 Stack」を採用しました。弊社の別のプロダクト（[UnTyping](https://untyping.jp?from=zenn)）で T3 Stack を使ってみていい感じだったので、引き続き
採用しました。

T3 Stack の簡単な説明については、[以前の記事](https://zenn.dev/taichifukumoto/articles/typing-game-untyping#untyping-%E3%82%92%E6%94%AF%E3%81%88%E3%82%8B%E6%8A%80%E8%A1%93%E3%82%B9%E3%82%BF%E3%83%83%E3%82%AF)をご覧ください。

今回、Google カレンダーの API を利用するということで、以下のライブラリを使用しました。

https://www.npmjs.com/package/googleapis

以下のコードで、Google カレンダーの予定を取得しています。

```ts
// Google カレンダー API クライアントを作成
const calendar = google.calendar({ version: "v3", auth: auth });

// タイムゾーンを取得：ログインしているユーザーの設定に合わせる
const timeZone = (await calendar.settings.get({ setting: "timezone" })).data.value;

const response = await calendar.freebusy.query({
  requestBody: {
    timeMin: startDate,
    timeMax: endDate,
    items: [{ id: "primary" }], // ログインしているユーザーのメインカレンダー
    timeZone,
  },
});

// 予定がある時間帯を取得
const busyTimes = response.data.calendars?.primary?.busy || [];
```

API の使い方は簡単でした。
タイムゾーンを合わせて、指定した期間で予定を取ってきています。`freebusy.query` を使ってカレンダーから情報を取得し、予定が入っている時間（busy）を返すようにしています。

`auth` には、Google の OAuth2 クライアントを渡します。

```ts
const auth = new google.auth.OAuth2({
  clientId: process.env.GOOGLE_CLIENT_ID,
  clientSecret: process.env.GOOGLE_CLIENT_SECRET,
});
```

上記の実装に加えて、Access Token や Refresh Token 周りの設定も必要です。

最初の実装だと、API 通信がちょっと遅いなと感じていたので、初回アクセス時に直近 30 日分の予定をあらかじめ取得するように実装しました。
そのため、直近 30 日分のテキストの作成は爆速になっています。直近 30 日以降を選択すると、ちょっと遅いのを感じることができると思います。（遅いのは多分 tRPC のせい）

## まとめ

今回は、Google カレンダーと連携したご都合いかがでしょうかメーカー「UnSchedule」について紹介しました。

https://unschedule.unreact.jp/?from=zenn

使っていただけると感じられると思いますが、地味に便利です。Google カレンダーで予定を管理されている方は、ぜひ使ってみてください。

最後までご覧いただきありがとうございました。
