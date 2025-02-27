---
layout: ~/layouts/MainLayout.astro
title: importエイリアス
description:  Astroを使ったエイリアス入門
i18nReady: true
---

**エイリアス**は、インポートのためのショートカットを作成する方法です。

エイリアスは、多くのディレクトリや相対的なインポートを持つコードベースにおいて、開発体験を改善するのに役立ちます。

```astro title="src/pages/about/company.astro" del="../../components" del="../../assets"
---
import Button from '../../components/controls/Button.astro';
import logoUrl from '../../assets/logo.png?url';
---
```

この例では、開発者は`src/pages/about/company.astro`、`src/components/controls/Button.astro`、そして`src/assets/logo.png`間のツリーの関係を理解する必要があります。そして、`company.astro`ファイルが移動された場合、これらのインポートも更新しなければなりません。

インポートエイリアスは`tsconfig.json`または`jsconfig.json`のどちらかから追加できます。

```json title="tsconfig.json" ins={5-6}
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@components/*": ["src/components/*"],
      "@assets/*": ["src/assets/*"]
    }
  }
}
```

:::note
エイリアスのパスが解決できるように`compilerOptions.baseUrl`が設定されていることを確認してください。
:::

この変更により、プロジェクト内の任意の場所でエイリアスを使用してインポートできるようになりました。

```astro title="src/pages/about/company.astro" ins="@components" ins="@assets"
---
import Button from '@components/controls/Button.astro';
import logoUrl from '@assets/logo.png?url';
---
```

これらのエイリアスは、[VS Code](https://code.visualstudio.com/docs/languages/jsconfig)や他のエディタにも自動的に統合されます。
