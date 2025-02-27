---
title: カスタムフォントの使用
description: Astro Webサイトにカスタム書体を使用したいですか？ FontsourceでGoogle Fontsを使用するか、お好みのフォントを追加してください。
layout: ~/layouts/MainLayout.astro
setup: |
    import PackageManagerTabs from '~/components/tabs/PackageManagerTabs.astro';
---

Astroは、サイトデザインにカスタム書体を追加するための一般的な戦略をすべてサポートしています。このガイドでは、プロジェクトにWebフォントを含めるための2つの異なるオプションを紹介します。

## ローカルのフォントファイルを使う

フォントファイルをプロジェクトに追加したい場合は、[`public/` directory](/ja/core-concepts/project-structure/#public)に追加することをお勧めします。CSSでは[`@font-face`ステートメント](https://developer.mozilla.org/ja/docs/Web/CSS/@font-face)でフォントを登録し、`font-family`プロパティを使ってサイトのスタイルを設定できます。

### 例

`DistantGalaxy.woff`というフォントファイルがあると想像してみましょう。

1. `public/fonts/`にフォントファイルを追加します。

2. CSSに`@font-face`ステートメントを追加します。これは、インポートするグローバルな`.css`ファイルでも、このフォントを使用したいレイアウトやコンポーネントの`<style>`ブロックでもかまいません。

    ```css
    /* カスタムフォントファミリーを登録し、ブラウザにその場所を知らせます。 */
    @font-face {
      font-family: 'DistantGalaxy';
      src: url('/fonts/DistantGalaxy.woff') format('woff');
      font-weight: normal;
      font-style: normal;
      font-display: swap;
    }
    ```

    :::note
    `public`にあるすべてのファイルはサイトのルートディレクトリに追加されるため、フォントのソースURLに`public`は含めません。
    :::

3. コンポーネントやレイアウト要素にスタイルを設定するには、`@font-face`ステートメントで指定した`font-family`を使用します。この例では、見出しの`<h1>`にはカスタムフォントが適用され、段落の`<p>`には適用されません。

    ```astro {10-12}
    ---
    // src/pages/example.astro
    ---

    <h1>はるかかなたの銀河系で…</h1>

    <p>カスタムフォントを使うと、見出しがよりカッコよくなりますね！</p>

    <style>
    h1 {
      font-family: 'DistantGalaxy', sans-serif;
    }
    </style>
    ```

## Fontsourceを使う

[Fontsource](https://fontsource.org/)プロジェクトを使うと、簡単にGoogle Fontsやその他のオープンソースのフォントを使用できます。使用したいフォントをnpmモジュールとしてインストールできます。

1. [Fontsource’s catalog](https://fontsource.org/fonts)で使用したいフォントを探します。例として、ここでは[Twinkle Star](https://fontsource.org/fonts/twinkle-star)を使用します。

2. 選択したフォントのパッケージをインストールしてください。

    <PackageManagerTabs>
      <Fragment slot="npm">
      ```shell
      npm i @fontsource/twinkle-star
      ```
      </Fragment>
      <Fragment slot="pnpm">
      ```shell
      pnpm i @fontsource/twinkle-star
      ```
      </Fragment>
      <Fragment slot="yarn">
      ```shell
      yarn add @fontsource/twinkle-star
      ```
      </Fragment>
    </PackageManagerTabs>

    :::tip
    正しいパッケージ名は、Fontsourceのウェブサイトの各フォントページの「Quick Installation」セクションに記載されています。 @fontsource/で始まり、その後にフォントの名前が続きます。
    :::

3. フォントを使用したいレイアウトまたはコンポーネントで、フォントパッケージをインポートします。通常は、サイト全体でフォントを利用するために、共通のレイアウトコンポーネントでこれを行います。

    インポートすると、フォントを設定するのに必要な`@font-face`ルールが自動的に追加されます。

    ```astro
    ---
    // src/layouts/BaseLayout.astro
    import '@fontsource/twinkle-star';
    ---
    ```

4. そのフォントのFontsourceページの内容にしたがって、`font-family`を使用します。これは、AstroプロジェクトでCSSを書けるところならどこでも使えます。

    ```css
    h1 {
      font-family: "Twinkle Star", cursive;
    }
    ```

## その他のリソース

### Tailwindでフォントを追加する

[Tailwindインテグレーション](/ja/guides/integrations-guide/tailwind/)を使用している場合は、フォントを登録する際に、ローカルのフォントに対して`@font-face`ステートメントを追加するか、Fontsourceの`import`戦略を使えます。その後、[カスタムフォントファミリーの追加についてのTailwindのドキュメント](https://tailwindcss.com/docs/font-family#using-custom-values)に従ってください。

### Webフォントの仕組みを学ぶ

[MDNのWebフォントガイド](https://developer.mozilla.org/ja/docs/Learn/CSS/Styling_text/Web_fonts)で紹介されています。

### フォント用のCSSを生成する

[Font SquirrelのWebフォントジェネレータ―](https://www.fontsquirrel.com/tools/webfont-generator)は、フォントファイルを準備するのに便利です。
