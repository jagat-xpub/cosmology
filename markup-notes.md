# 「宇宙論入門」のScholarly HTMLマークアップのメモ

## 変更点 2016-11-07

- インライン数式は `\( ... \)` で囲む（`$ ... $` から変更）。ディスプレイは `$$ ... $$` で囲む（これまで通り）。
さらにそれらを `<span data-type="tex" data-math-typeset="true"> ... </span>` で囲む。

例：
```
<p>宇宙論に表れる距離はpcでもまだ小さすぎるため， <span data-type="tex" data-math-typeset="true">\(10^{6}\mathrm{pc}\)</span> を表すMpc (メガパーセク)がよく用いられる：
<span data-type="tex" data-math-typeset="true">$$ \mathrm{Mpc} = 10^6 \mathrm{pc} = 3.086 \times 10^{22} \mathrm{m} \tag{A.2.1}$$</span></p>
```

- HTMLヘッダー部分から次を削除：
```
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {inlineMath: [["$","$"],["\\(","\\)"]]}
  });
</script>
```


## Scholarly HTML形式について

ドラフト仕様:
https://w3c.github.io/scholarly-html/

そのGitHubリポジトリ:
https://github.com/w3c/scholarly-html/

そこにある index.html、css、js をベースにしてます。


## HTMLファイルのテキストの形式

Dropboxなどでファイル共有する場合に作業環境に依存しないよう次の形式に統一：

* 改行はLF
* 文字エンコーディングはUTF-8 (BOM無し)

インデントは次のようにする：

* HTML要素の階層に応じてインデントする
* インデントはスペース(U+0020)２つずつ
* タブ文字は使わない

## HTMLヘッダー部分とタイトルと著者名

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>宇宙論入門</title>
    <link rel="stylesheet" href="css/scholarly.css">
    <script src="js/scholarly.min.js"></script>
    <script type="text/javascript" async
      src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
    </script>
  </head>
  <body prefix="schema: http://schema.org">
    <header>
      <h1>宇宙論入門</h1>
    </header>
    <div role="contentinfo">
      <dl>
        <dt>著者</dt>
        <dd>
          松原 隆彦
        </dd>
      </dl>
    </div>
    ...
  </body>
</html>
```

## セクション構造と見出しと段落のマークアップ

* セクションの階層構造は section 要素のネストで表す。
* 見出しは、最上位の section に h2、その下のsectionに h3、その下の section に h4 を使う。
* section 要素には id 属性をつける。とりあえず元テキストの見出しの番号（例"A-2-1"）をidにする。
* 見出し要素の内容に元テキストの見出しの番号は入れない。

元テキストとマークアップの例：

```
A 序章

A-1天文学と宇宙論

誰しもこの宇宙がなぜ存在するのか，（……）科学技術の進歩に支えられて大きく発展している．

一方で，天文学(Astronomy) はもともと（……）基礎物理学全般に大きな影響を及ぼしている．

A-2 天文学的単位について

宇宙論で用いられる物理量のスケールは（……）である．

A-2-1 長さ

宇宙論で用いられる長さの単位はpc（……）である．

B　ロバートソン・ウォーカー計量

宇宙をありのままの姿で記述しようとしても，（……）になる．

B-1　ワイルの要請

宇宙の時空のふるまいは一般相対論によって調べられるが，（……）ではない．
```
↓
```html
    <section id="A">
      <h2>序章</h2>
      <section id="A-1">
        <h3>天文学と宇宙論</h3>
        <p>誰しもこの宇宙がなぜ存在するのか，（……）科学技術の進歩に支えられて大きく発展している．</p>
        <p>一方で，天文学(Astronomy) はもともと（……）基礎物理学全般に大きな影響を及ぼしている．</p>
      </section>
      <section id="A-2">
        <h3>天文学的単位について</h3>
        <p>宇宙論で用いられる物理量のスケールは（……）である．</p>
        <section id="A-2-1">
          <h4>長さ</h4>
          <p>宇宙論で用いられる長さの単位はpc（……）である．</p>
        </section>
      </section>
    </section>
    <section id="B">
      <h2>ロバートソン・ウォーカー計量</h2>
      <p>宇宙をありのままの姿で記述しようとしても，（……）になる．</p>
      <section id="B-1">
        <h3>ワイルの要請</h3>
        <p>宇宙の時空のふるまいは一般相対論によって調べられるが，（……）ではない．</p>
      </section>
    </section>
```

## 数式

MathJax によりTeXの数式が有効です。

* インライン数式は元テキストのTeXコード `$ ... $` を `\( ... \)` に直して `<span data-type="tex" data-math-typeset="true"> ... </span>` で囲む。
* ディスプレイ数式は 元テキストの `$\displaystyle ... $` を `$$ ... $$` に直して `<span data-type="tex" data-math-typeset="true"> ... </span>` で囲む。
* 数式についている番号（例 `(A.2.1)`）は、`\tag{A.2.1}` の形式にする。

数式のマークアップ例

```
宇宙論に表れる距離はpcでもまだ小さすぎるため， $ 10^{6}\mathrm{pc}$  を表 すMpc (メガパーセク)がよく用いられる：
$\displaystyle \mathrm{Mpc} = 10^6 \mathrm{pc} = 3.086 \times 10^{22} \mathrm{m}$ (A.2.1)
```
↓
```html
<p>宇宙論に表れる距離はpcでもまだ小さすぎるため， <span data-type="tex" data-math-typeset="true">\(10^{6}\mathrm{pc}\)</span> を表すMpc (メガパーセク)がよく用いられる：
<span data-type="tex" data-math-typeset="true">$$ \mathrm{Mpc} = 10^6 \mathrm{pc} = 3.086 \times 10^{22} \mathrm{m} \tag{A.2.1}$$</span></p>
```
