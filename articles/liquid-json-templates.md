---
title: "Shopify Uniteで刷新されたLiquidのJSON templatesを理解する"
emoji: "💧"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Shopify, Liquid, 公式ドキュメント, 和訳]
published: true
---

# Shopify の Tempates ファイルが刷新された

先日の Shopify Unite 2021 で大幅なアップデートがありました。

その目玉アップデートとして、トップページ以外でも動的セクションを使用することができる（通称：Section Everywhere）ようになりました！

この改変に伴い、Shopfiy の Tempates ファイルも刷新されたというわけです。

# どう変わったのか

では、具体的にどのように変更になったのでしょうか？

これまでの`template`ファイルは、`.liquid`のように Liquid ファイルで記述されていました。
これが、Section Everywhere の実装に伴い、JSON ファイルに変わりました。

当然、Liquid と JSON ではファイル構造が全く違いますので、書き方も大きく違います。
刷新された JSON ファイルの記述方法についてまとめていきたいと思います。

基本的に[Shopify.dev の Themes のリファレンス](https://shopify.dev/themes)を和訳していきます。

# Templates の概要｜ Overview

まず、Shopify の[Templates の概要](https://shopify.dev/themes/architecture/templates)をみていきます。

Tempates ファイルでは、テーマ内の各タイプのページでレンダリングされる内容を制御します。Templates ファイルは`templates`ディレクトリに置かれます。

## Template ファイルの形式

テンプレートファイルには、2 種類の型式があります。

- JSON テンプレート
- Liquid テンプレート

JSON テンプレートが、今回新しく追加されたファイル形式ですね。

### JSON と Liquid の比較｜ JSON vs. Liquid

以下はドキュメントの直訳です。

> テンプレートでセクションを使用したい場合には、JSON テンプレートを使用します。
> JSON テンプレートを使用すると、アプリのセクションを含め、マーチャントがセクションを追加、削除、並び替えを柔軟に行うことができます。
> また、 settings_data.json のデータ量を最小限に抑えることができます。その代わり、データはテンプレートに直接保存されるため、テーマエディタのパフォーマンスが向上します。

JSON テンプレートを使用すると、どのテンプレートでも動的セクションを使用することができます。

後半が大きな変更点です。
Liquid テンプレートを使用した場合、ストアの状態を記録した`settings_data.json`は、`config`ディレクトリのなかに記述されていました。
このストアの状態が各 JSON テンプレートに記録されるようになりました。これにより、テーマエディタのパフォーマンスが向上したようです。

### JSON templates

> JSON テンプレートは、拡張子が`.json`のデータファイルです。これらのテンプレートを使うと、セクションのコンテンツを簡単にテンプレートに入れることができます。セクションは、テーマエディタを使ってマーチャントが追加、削除、順序変更を行うことができます。

詳しい JSON テンプレートの使い方については、記事の後半で解説していきます。

### Liquid templates

> Liquid テンプレートは、Liquid のマークアップファイルで、`.liquid`のファイル拡張子を持っています。

こちらに関しては、これまで通りになりますので、こちらの記事では説明を割愛いたします。

# JSON テンプレート｜ JSON templates

それでは、JSON テンプレートの詳細についてみていきましょう。

> JSON テンプレートを使うと、セクションを使ってオンラインストアのさまざまなページの見た目やフィールをコントロールすることができます。
>
> JSON テンプレートは、レンダリングされるセクションのリストと、それに関連する設定を格納したデータファイルです。マーチャントは、テーマエディターを使って、これらのセクションを追加、削除、および並べ替えることができます。JSON テンプレートを使ってページがレンダリングされると、セクションは `order` 属性で指定された順序でレンダリングされます。セクションの間にはマークアップはありません。

何度もできてきますが、JSON ファイルを使うことで Section Everywhere を実装できます。
従来の静的セクションのような機能は、`order`属性によって実装します。

> JSON テンプレートは、Liquid テンプレートとは内容が異なりますが、以下の Shopify テーマの機能をサポートするテンプレートファイルです。
>
> - `gift_card`と`robots.txt`を除く、すべてのテンプレートタイプ
> - 代替テンプレート
>
>   ファイル名は有効なテーマテンプレートタイプでなければならず、オプションで代替テンプレート用のサフィックスが必要です。例えば、商品テンプレートの名前は、`product.json` または `product.alternate.json` とすることができます。
>
> テンプレートは、JSON または Liquid のテンプレートとしてのみ存在し、両方は存在しません。例えば、`product.liquid` が既に存在している場合、`product.json` を作成することはできません。

要約すると、Shopify のテンプレートファイルの命名規則に則ったテンプレートであれば、JSON テンプレートが使えます。
そして同じ名前の Liquid テンプレートと JSON テンプレートは同時に使用することはできません。

## テンプレートの構造｜ Template structure

JSON テンプレートの構造についてみていきましょう。

```json
{
  "name": "template-name", //テンプレートの名前
  "layout": "layout", //theme.lisquid以外のlayoutファイルを適用したい場合
  "wrapper": "section", //テンプレート内の全てセクションのwrapper HTMLタグを指定します。
  "sections": {...}, //セクションIDをキーとし、セクションデータを値とするオブジェクト
  "order": [
    <SectionID>, //実際のセクションの並び順
    ...
  ]
}
```

JSON テンプレートは、上記のようなコードで実装されます。
各属性についてみていきましょう。

### `name` 属性

`name`属性には、文字列型でテンプレートな名前を記述します。必須属性です。

> `name`は公式ドキュメントでは確かに必須属性と記載があるのですが、Unite で新たに登場したテーマ「Dawn」のテンプレートファイルには`name`属性がないんですよね。
> これについて何か情報知っている方がいればぜひコメント等で教えていただきたいです。

### `layout` 属性

`layout` 属性には、テンプレートのレンダリングに使用される`layout`ファイル名を記述します。
例えば、任意のテンプレートファイルで`layout/full-width.liquid`ファイルを使ってレンダリングを行いたい場合は、`"layout": "full-width"` と書きます。
Liquid テンプレートファイルで使われる`{% layout 'file-name' %}`と同じ挙動をします。

`layout`属性は任意の属性で、指定を行わないとデフォルトの`layout/theme.liquid`がレンダリングに使用されます。

また、layout ファイルを使用したくない場合は、`false`を指定します。

### `wrapper` 属性

`wrapper`属性には、テンプレート内の全てのセクションをラップしたい HTML タグを記述します。タグは`div`・`main`・`section`のいずれかから選択します。

`wrapper`オブジェクトでは、`id`や`class`も合わせて指定することが可能です。そのため、そのテンプレート全てのセクションに同じクラス名を付与するといったことも可能です。`#`で`id`、`.`で`class`、`[]`でその他の属性を指定することができるようです。

```json:product.json
{
  "name": "Default product template",
  "wrapper": "div#div_id.div_class[attribute-one=value]",
  "sections": {
    "main": {
      "type": "product"
    }
  },
  "order": [
    "main"
  ]
}
```

```html:Output
<div id="div_id" class="div_class" attribute-one="value">
    <!-- product.json sections -->
</div>
```

### `sections` 属性

`sections`属性には、セクション ID をキーとし、セクションデータを値とするオブジェクトを記述します。これは、従来の`config/settings_data.json`と同じ役割を果たしており、その時のテーマの状態が記録されます。
`sections`属性は必須属性で、少なくとも 1 つのセクションが含まれている必要があります。また、テンプレート内での ID の重複は許されません。
セクションデータには、テーマセクションまたはアプリセクションのいずれかが含まれます。

JSON テンプレートは**最大 20**のセクションをレンダリングでき、各セクションは**最大 12**のブロックを持つことができます。

セクションデータのフォーマットについて説明していきます。
セクションデータは以下の例のようになっています。

```json
sections: {
  <SectionID>: {
    "type": <SectionType>,
    "disabled": <SectionDisabled>,
    "settings": {
      <SettingID>: <SettingValue>
    },
    "blocks": {
      <BlockID>: {
        "type": <BlockType>,
        "settings": {
          <SettingID>: <SettingValue>
        }
      }
    },
    "block_order": <BlockOrder>
  }
}
```

#### `<SectionID>`

`<SectionID>`は、セクションのユニークな ID です。半角英数字で設定されます。

これは考察も入っていますが、これまでのように静的セクションのような使い方をしたいセクションは`<SectinID>`を開発者が任意で指定し、`presets`が記述された動的セクションは自動的にユニークな ID が割り振られます。

#### `<SectionType>`

`<SectionType>`は、レンダリングするセクションファイルのファイル名です。拡張子はいりません。こちらは必須です。

#### `<SectionDisabled>`

`<SectionDisabled>`は`boolean`で指定し、セクションの表示状態を表します。任意の値でデフォルトでは`true`です。`false`にするとセクションはレンダリングされませんが、テーマエディタではカスタマイズできます。

テーマエディタのセクションにある"目のマーク"の設定ですね。

#### `<BlockID>`

`<BlockID>`は、ブロックのユニークな ID です。半角英数字のみで設定されます。

#### `<BlockType>`

`<BlockType>`は、セクションファイルの`schema`内で定義されている、ブロックの`type`です。

#### `<BlockOrder>`

`<BlockOrder>`は、ブロック ID の配列で、レンダリングされる順番に並べられています。
ID は blocks オブジェクト内に存在する必要があり、重複は許されません。

ブロックの ID は自動で一位の値が割り振られます。

#### `<SettingID>`

`<SettingID>`は、セクションファイルの`schema`で設定されているセクションまたはブロックの`settings`の`id`の値です。

#### `<SettingValue>`

`<SettingValue>`は、セクションまたはブロックの`settings`の有効値です。

以下は JSON テンプレートの具体例で、`product.liquid`と`product-recommendations.liquid`セクションをレンダリングします。

```json:templates/product.json
{
  "name": "Default product template",
  "sections": {
    "main": {
      "type": "product",
      "settings": {
        "show_vendor": true
      }
    },
    "recommendations": {
      "type": "product-recommendations"
    }
  },
  "order": [
    "main",
    "recommendations"
  ]
}
```

:::message
テンプレートに含まれるセクションのうち、アプリのセクションではないものは、テーマに存在してはいけません。存在しない場合はエラーになります。
:::

# まとめ

テンプレートファイルが`.liquid`から`.json`に変わり、どのテンプレートでも動的セクションの実装が可能になりました。
これは Shopify に関わる全ての人にとって嬉しいことだと思います。

新しいテンプレートファイルの形式に慣れつつ、開発を頑張っていきたいです。

最後まで読んでいただきありがとうございました。ではまた。
