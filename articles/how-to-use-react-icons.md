---
title: "React でアイコンを使うなら React Icons がおすすめ"
emoji: "❤️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [react]
published: true
---

# 概要

みなさんは React のプロジェクトでアイコンが必要な際はどのように実装していますか？

SVG の埋め込み？それとも Font Awesome？
私は、主に「React Icons」という React 用のライブラリを使用しています。

React Icons は、豊富な種類のアイコンが揃っておりシンプルで使いやすいため、大変重宝しています。

この記事では、React Icons の紹介とその使い方について解説しています。

それでは、早速みていきましょう。

# React Icons とは

**React Icons** は、Font Awesome や Material、Codicons（VSCode のアイコン）などのアイコンを簡単に利用することができる React 用のライブラリです。
npm パッケージとして提供されているため、`npm install`を使用してプロジェクトに導入します。

公式サイト 👇
https://react-icons.github.io/react-icons/

GitHub リポジトリ 👇
https://github.com/react-icons/react-icons

React Icons には、あらかじめ Font Awesome や Material など有名なアイコンライブラリのアイコンがまとめられています。
React Icons を導入することで、それらのアイコンを簡単にインポートして使用することが可能になります。

# React Icon の使い方

それでは早速 React Icons を導入して、使ってみましょう！

## React Icons のインストール

React Icons は、`npm install`で導入します。

```shell:~/path/to/your/project
$ npm install react-icons --save
```

:::message

プロジェクトが大きい場合は、オプションを使用することができます。Gatsbyjs や Meteorjs などで推奨されているようです。

```shell
$ npm install @react-icons/all-files --save
```

:::

## React Icons の基本的な使い方

それでは何かアイコンを表示してみましょう。
なんでも良いので、React の開発環境を作ってください。

以下が React Icons のサンプルコードです。（TypeScript, Tailwind CSS を使用しています。）
Font Awesome の GitHub アイコンを表示してみます。

```tsx:Text.tsx
import { FaGithub } from "react-icons/fa"

const Test: React.VFC = () => {
  return (
    <h1 className="text-6xl p-10">
      GitHub <FaGithub />
    </h1>
  )
}

export default Test
```

アイコンを使いたい時は`react-icons/xx`からアイコンコンポーネントを名前付きインポートして使用します。
Font Awesome のアイコンを使いたい時は、`xx`の部分を`fa`として、`react-icons/fa`からインポートします。
アイコンライブラリに応じて`xx`の部分は変わります。（基本的には、アイコンコンポーネント名の最初の 2 文字と同じです。）

アイコンを表示させたい場所で、`<FaGithub />`のようにコンポーネントの形で使用します。アイコンコンポーネントの名前は公式サイトで確認することができます。
アイコンのサイズなどのカスタマイズをしたい場合は、React Icons が用意している`IconContext`を使用します。`IconContext`の使い方は、記事の後半で解説しています。（簡単なカスタマイズ方法もあるので合わせて紹介しています。）

上記のサンプルコードの表示は次の画像のようになります。

![](https://storage.googleapis.com/zenn-user-upload/21c79b360fe4cb58a8360626.png)

GitHub のアイコンが表示されいますね。React Icons の基本的な使い方はこれだけです。

## React Icons の探し方

まずは、[React Icons の公式サイト](https://react-icons.github.io/react-icons)で使いたいアイコンを探します。

![](https://storage.googleapis.com/zenn-user-upload/40ee6c631e44d6027cf60e46.png)

検索方法は 2 通りあります。

- カテゴリ（アイコンライブラリ）から探す
- キーワードで探す

### カテゴリ（アイコンライブラリ）から探す

例えば、「Font Awesome のあのアイコンが使いたいんだよ〜」というように、あらかじめ使いたいアイコンが決まっている場合はそのアイコンライブラリから探します。

Font Awesome のアイコンが使いたければ、左のサイドバーにある「Font Awesome」をクリックしてください。Font Awesome の使用可能なアイコンが一覧表示されます。

![](https://storage.googleapis.com/zenn-user-upload/1759e333fc7f5916282d1bb7.png)

アイコンライブラリごとにアイコンが格納されているファイルが違うので、import パスも書かれています。
Font Awesome のアイコンを使いたい時は`react-icons/fa`からインポートすると良いです。

使いたいアイコンをクリックすると、アイコンコンポーネント名をコピーできます。

### キーワードで探す

サイドバーにある検索窓にキーワードを入力することで、アイコンを検索することができます。
キーワード検索では、アイコンライブラリを跨いで検索結果を表示してくれます。

画像は「github」で検索した結果になります。

![](https://storage.googleapis.com/zenn-user-upload/ad96a195e135fff1d89d1a9f.png)

さまざまなアイコンライブラリの GitHub アイコンが表示されています。この中から好みのアイコンを選択して使用することになります。

キーワード検索では、インポートパスが表示されません。
インポートするときは、使用するアイコンコンポーネント名の最初の 2 文字のフォルダからインポートすると良いです。
例えば、Ant Design Icons のアイコンである`AiFillGithub`を使用したい時は、`react-icons/ai`からインポートして使うといった感じです。

## アイコンのカスタマイズ

アイコンのサイズや色などは`IconContext`によってカスタマイズか可能です。

まず、`import { IconContext } from "react-icons"`で`IconContext`をインポートします。
カスタマイズしたいアイコンコンポーネントを`<IconContext.Provider>...</IconContext.Provider>`でラップします。

```tsx:Test.tsx
import { IconContext } from 'react-icons' //IconContextをインポート
import { FaGithub } from 'react-icons/fa'

const Test: React.VFC = () => {
  return (
    //IconContext.Providerでカスタマイズしたいアイコンをラップする
    <IconContext.Provider>
      <h1 className="text-6xl p-10">
        GitHub <FaGithub />
      </h1>
    </IconContext.Provider>
  )
}

export default Test
```

これでカスタマイズの準備は万端です。
`IconContext.Provider`の`value`に渡すことでカスタマイズが可能です。
GitHub の`README.md`にカスタマイズ可能な値がまとめられています。

| キー（Key） | デフォルト値（Default） | 備考                                                   |
| ----------- | ----------------------- | ------------------------------------------------------ |
| `color`     | `undefined` (inherit)   |                                                        |
| `size`      | `1em`                   |                                                        |
| `className` | `undefined`             |                                                        |
| `style`     | `undefined`             | サイズとカラーを上書きできます                         |
| `attr`      | `undefined`             | 他の属性で上書きされます                               |
| `title`     | `undefined`             | アクセシビリティ向上のためアイコンの説明を追加できます |

それでは、例として`color`と`size`を変更してみましょう。

```tsx:Test.tsx
import { IconContext } from 'react-icons'
import { FaGithub } from 'react-icons/fa'

const Test: React.VFC = () => {
  return (
    //valueにcolorとsizeの値を指定して渡す
    <IconContext.Provider value={{ color: '#ccc', size: '100px' }}>
      <h1 className="text-6xl p-10">
        GitHub <FaGithub />
      </h1>
    </IconContext.Provider>
  )
}

export default Test
```

これを保存して、表示を確認してみると画像のように変わっているはずです。

![](https://storage.googleapis.com/zenn-user-upload/05f1cf331fc31e016f375f62.png)

色が薄いグレーになり、サイズが大きくなっています。
こちらが公式で紹介されているカスタマイズ方法になります。

でも少しめんどくさくないですよね。

## もっと簡単にカスタマイズできない？

カスタマイズできるとはいえ、わざわざインポートしてカスタマイズするのは正直少しめんどくさい。

ということで、簡単にカスタマイズできる方法を見つけたので紹介します。

直接アイコンコンポーネントに`props`を渡します。

少しアイコンコンポーネントの型定義の話をします。
アイコンコンポーネントの定義を覗いてみると、各アイコンコンポーネントは`IconType`が型アノテーションされています。

さらに`IconTyep`の型定義を除いてみると、以下のような定義がありました。

```ts:react-icons/lib/cjs/iconBase.d.ts
...
export interface IconBaseProps extends React.SVGAttributes<SVGElement> {
    children?: React.ReactNode;
    size?: string | number;
    color?: string;
    title?: string;
}
export declare type IconType = (props: IconBaseProps) => JSX.Element;
...
```

`props`の定義に`size`と`color`と`title`がある！
つまり、アイコンコンポーネントではこれらを直接設定できるということですよね。

ということで、設定してみましょう。
`size`と`color`をカスタマイズしてみましょう。

```tsx:Test.tsx
import { FaGithub } from 'react-icons/fa'

const Test: React.VFC = () => {
  return (
    <h1 className="text-6xl p-10">
      GitHub <FaGithub size={100} color={'#ccc'} />
    </h1>
  )
}

export default Test
```

`size`は`string | number`で定義してありました。
`number`型を渡すと`size`の単位は`px`になるようです。上記の`size={100}`という指定では、アイコンのサイズは`100px`になっています。
`string`型も指定できるので、`2rem`や`50px`のように単位もつけてサイズを指定することも可能です。

![](https://storage.googleapis.com/zenn-user-upload/600030cb83170db5cdf46970.png)

実は、もっと型定義を遡ると、`React.SVGAttributes<SVGElement>`を継承しているので、`className`や`id`なんかも指定可能です。`style`によるインライン CSS の指定も可能でした。
ぜひ試してみてください。

# まとめ

今回は、React Icons を紹介してみました。

React Icons はさまざまなアイコンライブラリがまとまっていますので、大体のアイコンが見つかるので愛用しているライブラリです。
ぜひみなさんも React を使用するプロジェクトでアイコンが必要になったときに React Icons の使用を検討してみてください。

最後まで読んでいただきありがとうございました。ではまた。
