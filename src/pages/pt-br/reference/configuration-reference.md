---
layout: ~/layouts/MainLayout.astro
title: Referência da Configuração
setup: |
  import Since from '../../../components/Since.astro';
---

A referência a seguir cobre todas as opções de configuração suportadas no Astro. Para aprender mais sobre como configurar o Astro, leia o nosso guia, [Configurando Astro](/pt-br/guides/configuring-astro/).

```js
// astro.config.mjs
import { defineConfig } from 'astro/config'

export default defineConfig({
  // suas opções de configuração aqui...
})
```
## Opções Top-Level

### root

<p>

**Tipo:** `string`<br>
**Interface de Linha de Comando:** `--root`<br>
**Padrão:** `"."` (diretório de trabalho atual)
</p>

Você deve apenas providenciar esta opção se você executar os comandos da interface de linha de comando `astro` em um diretório diferente do que o diretório raiz do projeto. Geralmente, esta opção é providenciada pela interface de linha de comando ao invés do [arquivo de configuração Astro](/pt-br/guides/configuring-astro/#tipos-de-arquivo-de-configuração-suportados), já que Astro precisa saber a raiz do seu projeto antes de poder localizar seu arquivo de configuração.

Se você providenciar um caminho relativo (ex: `--root: './meu-projeto'`) Astro irá resolvê-lo com base no seu diretório de trabalho atual.

#### Exemplos

```js
{
  root: './diretorio-do-meu-projeto'
}
```
```bash
$ astro build --root ./diretorio-do-meu-projeto
```


### srcDir

<p>

**Tipo:** `string`<br>
**Padrão:** `"./src"`
</p>

Define o diretório em que Astro irá ler o seu site. 

O valor pode ser tanto um caminho absoluto do sistema ou um caminho relativo a raiz do projeto.

```js
{
  srcDir: './www'
}
```


### publicDir

<p>

**Tipo:** `string`<br>
**Padrão:** `"./public"`
</p>

Define o diretório de seus assets estáticos. Arquivos nesse diretório são servidos em `/` durante o desenvolvimento e são copiados para o diretório de saída durante o processo de build. Esses arquivos serão sempre servidos ou copiados da forma que são, sem transformações ou etapa de bundle.

O valor pode ser tanto um caminho absoluto do sistema ou um caminho relativo a raiz do projeto.

```js
{
  publicDir: './meu-diretorio-public-customizado'
}
```


### outDir

<p>

**Tipo:** `string`<br>
**Padrão:** `"./dist"`
</p>

Define o diretório em que `astro build` escreve a sua build final.

O valor pode ser tanto um caminho absoluto do sistema ou um caminho relativo a raiz do projeto.

```js
{
  outDir: './meu-diretorio-build-customizado'
}
```


### site

<p>

**Tipo:** `string`
</p>

Sua URL final no deploy. Astro utiliza esta URL completa para gerar seu sitemap e URLs canônicas na sua build final. É fortemente recomendado que você defina esta configuração para usufruir ao máximo o que o Astro pode oferecer.

```js
{
  site: 'https://www.meu-site.dev'
}
```


### base

<p>

**Tipo:** `string`
</p>

O caminho base no qual você está fazendo deploy. Astro irá corresponder este nome de caminho durante o desenvolvimento para que a experiência corresponda ao máximo possível o seu ambiente de build. No exemplo abaixo, `astro dev` irá iniciar seu servidor em `/docs`.

```js
{
  base: '/docs'
}
```


### trailingSlash

<p>

**Tipo:** `'always' | 'never' | 'ignore'`<br>
**Padrão:** `'ignore'`
</p>

Define o comportamento de correspondência de rotas do servidor de desenvolvimento. Escolha entre as seguintes opções:
- `'always'` - Apenas corresponde URLs que incluem uma barra final (ex: "/foo/")
- `'never'` - Nunca corresponde URLs que incluem uma barra final (ex: "/foo")
- `'ignore'` - Corresponde URLs independente da presença de uma "/" final

Use esta opção de configuração se o seu host de produção lida de forma estrita em como barras finais funcionam ou não.

Você também pode definir isto se você preferir ser mais estrito consigo mesmo, para que então URLs com ou sem barras finais não funcionem durante o desenvolvimento.

```js
{
  // Exemplo: Requer uma barra final durante o desenvolvimento
  trailingSlash: 'always'
}
```
**Veja Também:**
- build.format

### adapter

<p>

**Tipo:** `AstroIntegration`
</p>

Faça deploy para seu servidor de hospedagem, serverless ou edge favorito com adaptadores de build. Importe um dos nossos adaptadores oficiais para [Netlify](/pt-br/guides/deploy/netlify/#adaptador-para-ssredge), [Vercel](/pt-br/guides/deploy/vercel/#adaptador-para-ssr) e mais para começar a usar SSR no Astro.

[Veja nosso guia sobre Renderização no lado do Servidor](/pt-br/guides/server-side-rendering/) para mais sobre SSR e [nossos guias de deploy](/pt-br/guides/deploy/) para uma lista completa de hospedagem.

```js
import netlify from '@astrojs/netlify/functions';
{
  // Exemplo: Faça build para o serverless da Netlify
	 adapter: netlify(),
}
```
**Veja Também:**
- output


### output

<p>

**Tipo:** `'static' | 'server'`<br>
**Padrão:** `'static'`
</p>

Especifica o alvo de saída para builds.

- 'static' - Fazendo build de um site estático para fazer deploy em qualquer hospedagem estática.
- 'server' - Fazendo build de um app para fazer deploy em uma hospedagem que suporta SSR (renderização no lado do servidor).

```js
import { defineConfig } from 'astro/config';
export default defineConfig({
  output: 'static'
})
```
**Veja Também:**
- adapter


## Opções da Build

### build.format

<p>

**Tipo:** `('file' | 'directory')`<br>
**Padrão:** `'directory'`
</p>

Controla o formato final do arquivo de cada página.
  - Se for 'file', Astro irá gerar um arquivo HTML (ex: "/foo.html") para cada página.
  - Se for 'directory', Astro irá gerar um diretório com um arquivo `index.html` aninhado (ex: "/foo/index.html") para cada página.

```js
{
  build: {
    // Exemplo: Gera `pagina.html` ao invés de `pagina/index.html` durante a build.
    format: 'file'
  }
}
```

#### Efeito no Astro.url
Definir `build.format` controla o que `Astro.url` é durante a build. Quandl ele for:
- `directory` - O `Astro.url.pathname` irá incluir uma barra final para imitar o comportamento da pasta; ou seja, `/foo/`.
- `file` - O `Astro.url.pathname` irá incluir `.html`; ou seja, `/foo.html`.

Isso significa que quando você criar URLs relativas usando `new URL('./relativo', Astro.url)`, você terá um comportamento consistente entre dev e build.

## Opções do Servidor

Customize o servidor de desenvolvimento do Astro, usado por `astro dev` e `astro preview`.

```js
{
  server: { port: 1234, host: true }
}
```

Para definir uma configuração diferente baseada no comando run ("dev", "preview") uma função também pode ser passada para esta opção de configuração.

```js
{
  // Exemplo: Use a sintaxe de função para customizar com base no comando
  server: (command) => ({ port: command === 'dev' ? 3000 : 4000 })
}
```

### server.host

<p>

**Tipo:** `string | boolean`<br>
**Padrão:** `false`<br>
<Since v="0.24.0" />
</p>

Define em quais endereços de IP da rede o servidor deve ser escutado em (ou seja, IPs que não sejam localhost).
- `false` - não o expõe em um endereço de IP da rede
- `true` - escutado em todos os endereços, incluindo endereços LAN e públicos
- `[endereço-customizado]` - o expõe ao endereço de IP da rede em `[endereço-customizado]` (ex: `192.168.0.1`)

### server.port

<p>

**Tipo:** `number`<br>
**Padrão:** `3000`
</p>

Define em qual porta o servidor deve ser escutado.

Se a porta indicada já estiver em uso, Astro irá automaticamente tentar a próxima porta disponível.

```js
{
  server: { port: 8080 }
}
```


## Opções de Markdown

### markdown.drafts

<p>

**Tipo:** `boolean`<br>
**Padrão:** `false`
</p>

Controla se páginas de rascunho Markdown devem ser inclusas na build.

Uma página Markdown é considerada um rascunho se ela inclui `draft: true` em seu frontmatter. Páginas de rascunho estarão sempre inclusas e visíveis durante o desenvolvimento (`astro dev`) mas por padrão elas não serão inclusas em sua build final.

```js
{
  markdown: {
    // Exemplo: Inclui todos os rascunhos em sua build final
    drafts: true,
  }
}
```

### markdown.shikiConfig

<p>

**Tipo:** `Partial<ShikiConfig>`
</p>

Opções da configuração do Shiki. Veja [a documentação da configuração de Markdown](/pt-br/guides/markdown-content/#configuração-do-shiki) para entender como configurá-lo.


### markdown.syntaxHighlight

<p>

**Tipo:** `'shiki' | 'prism' | false`<br>
**Padrão:** `shiki`
</p>

Qual syntax highlighter deve ser utilizado, caso utilize. 
- `shiki` - utiliza o highlighter do [Shiki](https://github.com/shikijs/shiki)
- `prism` - utiliza o highlighter do [Prism](https://prismjs.com/)
- `false` - não aplica syntax highlighting.

```js
{
  markdown: {
    // Exemplo: Mudando para utilizar prism como syntax highlighter em Markdown
    syntaxHighlight: 'prism',
  }
}
```


### markdown.remarkPlugins

<p>

**Tipo:** `RemarkPlugins`
</p>

Passe [plugins remark](https://github.com/remarkjs/remark) para customizar como seu Markdown é construído. Você pode importar e aplicar a função do plugin (recomendado), ou passar o nome do plugin como uma string.

:::caution
Providenciar uma lista de plugins irá **remover** nossos plugins padrão. Para preservar esses padrões, veja a flag `extendDefaultPlugins`.
:::

```js
import remarkToc from 'remark-toc';
{
  markdown: {
    remarkPlugins: [remarkToc]
  }
}
```


### markdown.rehypePlugins

<p>

**Tipo:** `RehypePlugins`
</p>

Passe [plugins rehype](https://github.com/remarkjs/remark-rehype) para customizar como o HTML resultante do seu Markdown é processado. Você pode importar e aplicar a função do plugin (recomendado), ou passar o nome do plugin como uma string.

:::caution
Providenciar uma lista de plugins irá **remover** nossos plugins padrão. Para preservar esses padrões, veja a flag `extendDefaultPlugins`.
:::

```js
import rehypeMinifyHtml from 'rehype-minify';
{
  markdown: {
    rehypePlugins: [rehypeMinifyHtml]
  }
}
```


### markdown.extendDefaultPlugins

<p>

**Tipo:** `boolean`<br>
**Padrão:** `false`
</p>

Astro aplica os plugins [GitHub-flavored Markdown](https://github.com/remarkjs/remark-gfm) e [Smartypants](https://github.com/silvenon/remark-smartypants) por padrão. Ao adicionar seus próprios plugins remark ou rehype, você pode preservar esses padrões definindo a flag `extendDefaultPlugins` como `true`:

```js
{
  markdown: {
    extendDefaultPlugins: true,
		 remarkPlugins: [pluginRemarkExemplo],
    rehypePlugins: [pluginRehypeExemplo],
  }
}
```


### markdown.remarkRehype

<p>

**Tipo:** `RemarkRehype`
</p>

Passe opções para o [remark-rehype](https://github.com/remarkjs/remark-rehype#api).

```js
{
  markdown: {
    // Exemplo: Traduza o texto das notas de rodapé para outra língua, aqui estão os valores padrões em Inglês
    remarkRehype: { footnoteLabel: "Footnotes", footnoteBackLabel: "Back to content"},
  },
};
```

## Adaptador

Faça deploy em seu servidor de hospedagem, serverless ou edge favorita com adaptadores de build. Importe um de nossos adaptadores oficiais para [Netlify](/pt-br/guides/deploy/netlify/#adaptador-para-ssredge), [Vercel](/pt-br/guides/deploy/vercel/#adaptador-para-ssr) e mais para possibilitar o SSR do Astro.

[Veja nosso guia de Renderização no lado do Servidor](/pt-br/guides/server-side-rendering/) para saber mais sobre SSR, e [nossos guias de deploy](/pt-br/guides/deploy/) para uma lista completa de hospedagens.

```js
import netlify from '@astrojs/netlify/functions';
{
  // Exemplo: Faça build para fazer deploy no serverless da Netlify
	 adapter: netlify(),
}
```

## Integrações

Estenda Astro com integrações customizadas. Integrações são sua via de mão única para adicionar suporte a frameworks (como Solid.js), novas funcionalidades (como sitemaps) e novas bibliotecas (como Partytown e Turbolinks).

Leia nosso [Guia de Integrações](/pt-br/guides/integrations-guide/) para mais ajuda em como começar a utilizar Integrações Astro.

```js
import react from '@astrojs/react';
import tailwind from '@astrojs/tailwind';
{
  // Exemplo: Adicione suporte a React + Tailwind no Astro
  integrations: [react(), tailwind()]
}
```

## Vite

Passe opções de configuração adicionais ao Vite. Útil quando o Astro não suporta algumas configurações avançadas que você pode precisar.

Veja a documentação completa do objeto de configuração `vite` em [vitejs.dev](https://vitejs.dev/config/).

#### Exemplos

```js
{
  vite: {
    ssr: {
      // Exemplo: Force um pacote quebrado a pular o processamento SSR, se necessário
      external: ['pacote-npm-quebrado'],
    }
  }
}
```

```js
{
  vite: {
    // Exemplo: Adicione plugins vite customizados diretamente em seu projeto Astro
    plugins: [meuPlugin()],
  }
}
```

## Flags Legado

Para ajudar usuários a migrarem entre versões do Astro, nós ocasionalmente introduzimos flags `legacy`. Estas flags te permitem optar por um descontinuado ou antigo comportamento do Astro na versão mais recente, para que então você possa continuar atualizando e se aproveitando de novas versões do Astro.

### legacy.astroFlavoredMarkdown

<p>

**Tipo:** `boolean`<br>
**Padrão:** `false`<br>
<Since v="1.0.0-rc.1" />
</p>

Habilite o suporte pré-v1.0 do Astro para componentes e expressões JSX em arquivos Markdown `.md`. No Astro `1.0.0-rc`, esse comportamento original foi removido como o padrão, em favor da nova [integração MDX](/pt-br/guides/integrations-guide/mdx/).

Para habilitar este comportamento, defina `legacy.astroFlavoredMarkdown` para `true` no seu [arquivo de configuração `astro.config.mjs`](/pt-br/guides/configuring-astro/#o-arquivo-de-configuração-astro).

```js
{
  legacy: {
    // Exemplo: Adicione suporte para funcionalidades Markdown legado
    astroFlavoredMarkdown: true,
  },
}
```
