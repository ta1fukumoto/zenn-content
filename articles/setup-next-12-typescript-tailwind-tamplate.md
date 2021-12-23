---
title: "Next.js 12 × TypeScript × Tailwind CSS のテンプレートを作成する"
emoji: "😙"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [nextjs, tailwindcss, typescript, eslint, prettier]
published: true
---

# この記事について

この記事では、Next.js プロジェクトを 0 から立ち上げる際の手順を説明しています。
`create-next-app` で TypeScript の Next.js プロジェクトを作成し、Tailwind CSS を導入していきます。

またこの記事は、[株式会社 UnReact のアドベントカレンダー](https://qiita.com/advent-calendar/2021/unreact) 24 日目の記事です。

## 前提条件

- Next.js に関する基礎的な知識がある
- Tailwind CSS に関する基礎的な知識がある
- Volta で Node.js / npm をインストールしている

Volta についてはこちらの記事をご覧ください。（絶対に Volta が必要だというわけではありません。）

https://zenn.dev/taichifukumoto/articles/how-to-use-volta

# Next.js プロジェクトの立ち上げ

まずは、`create-next-app` でプロジェクトを立ち上げます。

## Node.js と npm のバージョン確認

まず、Node.js と npm のバージョンを確認します。
Node.js は安定版を仕様します。（2021 年 12 月現在では `v16`）
大事なのは npm のバージョンの方で `7系` 以上の新しいバージョンが望ましいです。（2021 年 12 月現在では、`8系` が最新）

```shell:Teminal
# Node.js のバージョン確認
node -v

# npm のバージョン確認
npm -v
```

バージョンが低い場合は、`volta install node` もしくは `volta install npm` で最新の安定板をインストールしましょう。

## `create-next-app`

Next.js プロジェクトの立ち上げには `create-next-app` を使用する。

プロジェクトを作成するディレクトリに移動する。

```shell:Terminal
cd ./path/to/your/prj/dir
```

移動したディレクトリで以下のコマンドを実行する。

```shell:Terminal
npx create-next-app --ts --use-npm next-app-name
```

`--ts` オプションは、TypeScript でプロジェクトを立ち上げる際につけます。
`--use-npm` オプションは、プロジェクトを npm で立ち上げるためにつけます。私は npm 派なので、明示的に npm を使用するように指定します。もちろんオプションを外して yarn を使っていただいて構いません。
`next-app-name` は、プロジェクトの名前です。ルートディレクトリの名前になります。

実行が完了すると、以下のようなファイル構成の Next.js プロジェクトが作成されます。

```:プロジェクトの構成
.
├── pages
├── public
├── styles
├── .eslintrc.json
├── .git
├── .gitignore
├── README.md
├── next-env.d.ts
├── next.config.js
├── node_modules
├── package-lock.json
├── package.json
└── tsconfig.json
```

## `volta pin` で Node.js / npm のバージョンを固定

開発するメンバーの Node.js と npm のバージョンを揃えるために `volta pin` でバージョンを固定します。
以下のコマンドを実行しましょう。（バージョンは 2021 年 11 月の場合）

```shell:Terminal
# Node.js のバージョンを固定
volta pin node@16

# npm のバージョンを固定
volta pin npm@8
```

`package.json` に以下のように追記されていれば OK です。

```diff_javascript:package.json
{
  "name": "next-template-v12",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "12.0.4",
    "react": "17.0.2",
    "react-dom": "17.0.2"
  },
  "devDependencies": {
    "@types/react": "17.0.37",
    "eslint": "7.32.0",
    "eslint-config-next": "12.0.4",
    "typescript": "4.5.2"
- }
+ },
+ "volta": {
+   "node": "16.13.0",
+   "npm": "8.1.4"
+ }
}
```

## ソースコードを `src` 配下に移動

`pages` や `styles` などソースコードを `src` に入れます。このタイミングで `components`・`type`・`hooks`・`lab` ディレクトリも作成します。

```:移動後
.
├── public
├── src
│   ├── components
│   ├── hooks
│   ├── lib
│   ├── pages
│   ├── styles
│   └── types
├── .eslintrc.json
├── .git
├── .gitignore
├── README.md
├── next-env.d.ts
├── next.config.js
├── node_modules
├── package-lock.json
├── package.json
└── tsconfig.json
```

## `src/components` の整備

`src/components` 内は、`layout`・`uiGroup`・`uiParts` に分けてコンポーネントを作成していきます。
こちらは各々のディレクトリパターンによって構成が変わると思いますので、深くは言及しません。

```:src/components/にフォルダを追加
.
├── public
├── src
│   ├── components
│   │   ├── layout
│   │   ├── uiGroup
│   │   └── uiParts
│   ├── hooks
│   ├── lib
│   ├── pages
│   ├── styles
│   └── types
├── .eslintrc.json
├── .git
├── .gitignore
├── README.md
├── next-env.d.ts
├── next.config.js
├── node_modules
├── package-lock.json
├── package.json
└── tsconfig.json
```

## `tsconfig.json` の設定

`tsconfig.json` に追加の設定を書き込みます。
以下の設定を追記してください。

```diff json:tsconfig.json
{
  "compilerOptions": {
+   "baseUrl": ".",
+   "paths": {
+     "@/*": ["./src/*"]
+   },
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve"
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx"],
  "exclude": ["node_modules"]
}
```

`baseUrl` の設定により、プロジェクト内の import パスを絶対パスで書くことができます。（相対パスによる `..` 地獄を防ぐ）
`paths` の設定により、よく使うパスにエイリアスをつけます。上記の設定により、`./src/components` を `@/components` と書くことができるようになります。

## ESLint と Prettier の設定

Next.js の 11 系よりデフォルトで ESLint が統合されているので、ESLint と Prettier の設定を行います。

### ESLint の設定

ESLint プラグインの追加や `.eslintrc.json` の設定を行います。

#### ESLint プラグインの追加

TypeScript 関連の ESLint プラグインをインストールします。
以下のコマンドを実装します。

```shell:Terminal
npm install -D @typescript-eslint/parser @typescript-eslint/eslint-plugin

# インストールがうまくいかない場合
npm i -D --legacy-peer-deps @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

次に Import 文を Lint してくれるプラグインをインストールします。

```shell:Terminal
npm i -D eslint-plugin-import eslint-plugin-unused-imports

# インストールがうまくいかない場合
npm i -D --legacy-peer-deps eslint-plugin-import eslint-plugin-unused-imports
```

インストールがうまくかない場合は、`--legacy-peer-deps` オプションをつけて実行してみてください。

インストールが完了したら、`.eslintrc.json` ファイルを以下のように書き換えます。

```diff json:.eslintrc.json
{
  "root": true,
  // parser を設定
  "parser": "@typescript-eslint/parser",
  // tsconfig.json のパスを設定
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "extends": [
    "next/core-web-vitals",
    "eslint:recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking"
  ],
  "plugins": [
    "@typescript-eslint",
    "import",
    "unused-imports"
  ],
  "rules": {
    // あった方が良さそうなルール
    "semi": "error",
    "no-console": "warn",
    "no-var": "error",
    // import 文の順序に関するルール
    "sort-imports": 0,
    "no-unused-vars": "warn",
    "import/order": [
      "error",
      {
        "groups": [
          "builtin",
          "external",
          "internal",
          ["parent", "sibling"],
          "object",
          "type",
          "index"
        ],
        "newlines-between": "always",
        "pathGroupsExcludedImportTypes": ["builtin"],
        "alphabetize": { "order": "asc", "caseInsensitive": true }
      }
    ],
    // 使用されていない import に関するルール
    "unused-imports/no-unused-imports": "error",
    "unused-imports/no-unused-vars": [
      "warn",
      { "vars": "all", "varsIgnorePattern": "^_", "args": "after-used", "argsIgnorePattern": "^_" }
    ]
  }
}
```

#### `.eslintignore` の設定

`.eslintignore` ファイルで、ESLint の対象外とするファイルを設定します。
以下のコマンドで設定ファイルを作成します。

```shell:Terminal
touch .eslintignore
```

作成したファイルの中に以下の設定を書きます。

```:.eslintignore
node_modules/
```

上記の設定により、`node_modules` ディレクトリ内のファイルは ESLint の対象外となります。

### Prettier の設定

Prettier の設定をしていきます。

#### Prettier のインストール

Prettier をインストールします。以下のコマンドを実行しましょう。

```shell:Terminal
npm install -D prettier eslint-config-prettier
```

#### `.prettierrc.json` の設定

`.prettierrc.json` を作成し、に Prettier の設定を記述します。

```shell:Terminal
# .prettierrc.json ファイルを作成
touch .prettierrc.json
```

```json:.prettierrc.json
{
  "printWidth": 100, // 1行の最大文字数を100文字に
  "tabWidth": 2, // タブのサイズを半角スペース 2 つに
  "semi": true, // 末尾セミコロンあり
  "trailingComma": "all", // 末尾のカンマあり
  "singleQuote": true, // シングルクォーテーションに統一
  "arrowParens": "always", // Arrow 関数の引数に () を付ける
}
```

こちらは好みですので、プロジェクトに合わせて適当にカスタマイズしてください。

#### ESLint と Prettier の連携

デフォルトで生成されている `.eslintrc.json` に Prettier の設定を追記し、バッティングしないように連携させます。
`.eslintrc.json` を以下のように書き換えましょう。

```diff json:.eslintrc.json
{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "extends": [
    "next/core-web-vitals",
    "eslint:recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
+   "prettier"
  ],
  "plugins": [
    "@typescript-eslint",
    "import",
    "unused-imports"
  ],
  "rules": {
    "semi": "error",
    "no-console": "warn",
    "no-var": "error",
    "sort-imports": 0,
    "no-unused-vars": "warn",
    "import/order": [
      "error",
      {
        "groups": [
          "builtin",
          "external",
          "internal",
          ["parent", "sibling"],
          "object",
          "type",
          "index"
        ],
        "newlines-between": "always",
        "pathGroupsExcludedImportTypes": ["builtin"],
        "alphabetize": { "order": "asc", "caseInsensitive": true }
      }
    ],
    "unused-imports/no-unused-imports": "error",
    "unused-imports/no-unused-vars": [
      "warn",
      { "vars": "all", "varsIgnorePattern": "^_", "args": "after-used", "argsIgnorePattern": "^_" }
    ]
  }
}
```

### ESLint と Prettier の npm スクリプト

ESLint と Prettier の npm スクリプトを設定します。
デフォルトでは、`lint` というコマンドのみ設定されています。
自動で修正してくれるコマンドと自動で整形してくれるコマンドを足します。

```diff json:package.json
{
  ...,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
-   "lint": "next lint"
+   "lint": "next lint --dir src",
+   "lint:fix": "next lint --dir src --fix",
+   "format": "prettier --write --ignore-path .gitignore './**/*.{js,jsx,ts,tsx,json}"
  },
  ...
}
```

`npm run lint` を実行すると、`src` ディレクトリ内のファイルの Error や Warning を調べてくれます。
`npm run lint:fix` を実行すると、`src` ディレクトリ内のファイルの Error や Warning を調べて、自動で修正してくれます。
`npm run format` を実行すると、`.gitignore` ファイルに記述されたファイル以外の全てのファイルをフォーマットしてくれます。

コミットする前に、これらの npm スクリプトを走らせることで、コードの品質を保つことができます。

### VS Code の設定を共有

最後に VS Code で保存時に自動で ESLint や Prettier が走るように VS Code の設定を共有します。
設定は、`.vscode/settings.json` に記述します。設定はお好みで変更してください。

```shell:Terminal
# .vscode/settings.json を作成
mkdir .vscode && touch .vscode/settings.json
```

```json:.vscode/settings.json
{
  "files.exclude": {
    "**/node_modules": true // node_modules を表示しない
  },
  "editor.tabSize": 2, // インデントのサイズを 2 に設定
  "editor.defaultFormatter": "esbenp.prettier-vscode", // デフォルトのフォーマッタを Prettier に設定
  "editor.formatOnSave": true, // 保存時にフォーマッタを走らせる
  "editor.codeActionsOnSave": {
    "source.organizeImports": true, //保存時に import を整理する
    "source.fixAll.eslint": true // 保村時に ESLint で修正する
  },
  "[markdown]": {
    "files.trimTrailingWhitespace": false // Markdown ファイルの末尾の空白を取り除く
  }
}
```

`.vscode` ディレクトリを作ったので同時に推奨拡張機能の設定もしておきます。
こちらもお好みで変更してください。

```shell:Terminal
# .vscode/extensions.json ファイルを作成
touch .vscode/extensions.json
```

作成したファイルに以下の設定を書きます。

```json:.vscode/extensions.json
{
  "recommendations": [
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "streetsidesoftware.code-spell-checker",
    "bradlc.vscode-tailwindcss",
    "csstools.postcss"
  ]
}
```

この設定により、インストールしていない拡張機能があると、VS Code 上で右下に通知が出るようになります。

## `_document.tsx` の作成

`src/pages/_document.tsx` を作成します。`_document.tsx` には、サイト全体に反映させたいデフォルトの `<Head>` タグの設定などを記述することができます。
例えば、Google Fonts のインポートはこのファイルで行います。

```shell:Terminal
# src/pages/_document.tsx を作成
touch src/pages/_document.tsx
```

作成した `src/pages/_document.tsx` に以下のコードを書いてください。

```tsx:src/pages/_document.tsx
import Document, { Html, Head, Main, NextScript, DocumentContext } from 'next/document';

class MyDocument extends Document {
  static async getInitialProps(ctx: DocumentContext) {
    const initialProps = await Document.getInitialProps(ctx);

    return initialProps;
  }

  render() {
    return (
      <Html lang='ja' dir='ltr'>
        <Head>
          {/* サイト全体に反映させたいデフォルトの設定を記述する ex) Google Fonts の読み込み */}
        </Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```

## 不要なものを整理

デフォルトで生成されるもので、不要なものを削除します。

### `index.tsx` の整理

`src/pages/index.tsx` の中身を整理します。以下のようにいらない部分を削除します。

```tsx:src/pages/index.tsx
const Home = (): JSX.Element => {
  return <div>Hello Next.js!</div>;
};

export default Home;
```

### `Home.module.css` を削除

`src/styles/Home.module.css` を削除します。

```shell:Terminal
rm src/styles/Home.module.css
```

# Tailwind CSS の導入

ここからは Tailwind CSS を導入していきます。

## Tailwind CSS のインストール

以下コマンドでプロジェクトに Tailwind CSS 関連のファイルをインストールします。

```shell:Terminal
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
```

## Tailwind CSS 設定ファイルの生成

以下のコマンドで、`tailwind.config.js` と `postcss.config.js` を作成します。`-p`オプションが `postcss.config.js` を同時に生成するオプションです。

```shell:Terminal
npx tailwindcss init -p
```

以下のような 2 つのファイルが生成されます。

```js:tailwind.config.js
module.exports = {
  purge: [],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```

```js:postcss.config.js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

## `tailwind.config.js` の編集

生成された `tailwind.config.js` を編集します。
中身を以下のように書き換えてください。

```js:talwind.config.js
const defaultTheme = require('tailwindcss/defaultTheme');

module.exports = {
  mode: 'jit', //JITモードを有効化
  // パージの対象ファイルを設定
  purge: ['./src/pages/**/*.{js,ts,jsx,tsx}', './src/components/**/*.{js,ts,jsx,tsx}'],
  darkMode: false, // or 'media' or 'class'
  theme: {
    fontFamily: {
      // スプレッドで展開している前に、メインとしたいフォント名を追加する
      // フォント名にスペースがある場合は、'Noto\\ Sans\\ JP'のように \ (バックスラッシュ)でエスケープする
      // Serif 体がメインの場合は、 `...defaultTheme.fontFamily.serif` を展開する
      ja: [...defaultTheme.fontFamily.sans],
      en: [...defaultTheme.fontFamily.sans],
    },
    extend: {
      colors: {
        // 開発で使用するカラーを設定する
        // `DEFAULT` で設定しているものは、`text-theme` や `bg-primary` のように使用できる
        // それ以外は、 `text-theme-light` や `bg-primary-dark` のように使用する
        // テキストなどに使うベースカラー
        theme: {
          light: '#ffffff',
          medium: '#cccccc',
          DEFAULT: '#242424',
          dark: '#111111',
        },
        // メインカラー
        primary: {
          // light: '',
          // medium: '',
          DEFAULT: '#242424',
          // dark: '',
        },
        // サブカラー
        // secondary: {
        //   light: '',
        //   medium: '',
        //   DEFAULT: '',
        //   dark: '',
        // },
        // アクセントカラー
        // accent: {
        //   light: '',
        //   medium: '',
        //   DEFAULT: '',
        //   dark: '',
        // },
      },
    },
  },

  variants: {
    extend: {},
  },
  plugins: [],
};
```

追加した部分にはコメントを入れています。さらにカスタムする場合は、適宜足してください。

## `globals.css` で読み込む

`src/styles/globals.css` で Tailwind CSS をインポートします。
`global.css` の冒頭に以下のコードを追加してください。

```css:src/styles/globals.css
@tailwind base;
@tailwind components;
@tailwind utilities;

...
```

## `globals.css` の編集

Tailwind CSS をインポートしたら、デフォルト設定を Tailwind CSS に書き直します。
`globals.css` を以下のように書き直しましょう。（デフォルトの CSS は全て削除して大丈夫です。）

```css:src/styles/globals.css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  body {
    @apply font-ja leading-relaxed tracking-wider;
  }
}
```

`@layer base {}` で Tailwind CSS の `base` を編集することができます。
上記のように `body`　に対してデフォルトのフォントと行間・文字間を設定しています。
デフォルトの、文字間や行間だと日本語では窮屈に見えてしまうので、設定することをおすすめします。

## `eslint-plugin-tailwind` の導入

Tailwind CSS に関する Lint を行ってくれる ESLint プラグイン `eslint-plugin-tailwind` を導入します。
以下のコマンドでプラグインをインストールします。

```shell:Terminal
npm install -D eslint-plugin-tailwindcss
```

インストールが完了したら、`.eslintrc.json` に `eslint-plugin-tailwind` の設定を書き込みます。

```diff json:.eslintrc.json
{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "extends": [
    "next/core-web-vitals",
    "eslint:recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "prettier",
+   "plugin:tailwindcss/recommended"
  ],
  "plugins": [
    "@typescript-eslint",
    "import",
    "unused-imports",
+   "tailwindcss"
  ],
  "rules": {
    "semi": "error",
    "no-console": "warn",
    "no-var": "error",
    "sort-imports": 0,
    "no-unused-vars": "warn",
    "import/order": [
      "error",
      {
        "groups": [
          "builtin",
          "external",
          "internal",
          ["parent", "sibling"],
          "object",
          "type",
          "index"
        ],
        "newlines-between": "always",
        "pathGroupsExcludedImportTypes": ["builtin"],
        "alphabetize": { "order": "asc", "caseInsensitive": true }
      }
    ],
    "unused-imports/no-unused-imports": "error",
    "unused-imports/no-unused-vars": [
      "warn",
      { "vars": "all", "varsIgnorePattern": "^_", "args": "after-used", "argsIgnorePattern": "^_" }
    ],
+   "tailwindcss/classnames-order": "warn",
+   "tailwindcss/no-custom-classname": "warn",
+   "tailwindcss/no-contradicting-classname": "error"
  }
}
```

以上の設定で、`npm run lint` や `npm run lint:fix` をやると、Tailwind CSS の Lint も行ってくれます。

### `tailwind.config.js` を ESLint から除外

`tailwind.config.js`　を ESLint の対象外に設定します。
`.eslintignore` ファイルに以下の設定を追加します。

```diff:.eslintignore
  node_modules/
+ tailwind.config.js
```

以上で Tailwind CSS の導入は完了です。

# テンプレートからプロジェクトを始める

今回作成したテンプレートは、弊社の公開リポジトリとして置いています。

https://github.com/UnReacts/next-template-v12

上記のテンプレートからプロジェクトを始めたい場合は、以下のコマンドを実行してください。
参考になればと思います。

また、記事やテンプレートに関してミスや改善点などございましたら、Issue を立ててもらったり、プルリクエストを送っていただけるとありがたいです。

最後まで読んでいた抱きありがとうございました。
弊社の仲間のアドベントカレンダー投稿も興味があればご覧いただけるとみんな喜びます。よろしくお願いします。

https://qiita.com/advent-calendar/2021/unreact

#### この記事のリポジトリ

https://github.com/ta1fukumoto/zenn-content
