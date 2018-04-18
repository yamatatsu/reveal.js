## 目次

- reduxは要らないかもしれない
- contextAPIはあんまり必要ないかもしれない

===

## reduxは要らないかもしれない

==

って

==

作者が言ってました。

==

<a href="https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367">
<img src="https://i.gyazo.com/78b5a24e461e9f85538315090a558c2c.png" alt="https://gyazo.com/78b5a24e461e9f85538315090a558c2c">
</a>

==

曰く

==

トレードオフがあるので、そこを理解して使おうね

===

## reduxの恩恵

==

**single state &amp; serializable の恩恵**
- 状態をローカルストレージに永続化して、それをもとに状態の復元を行う
- 初期状態をHTMLに埋め込んでおいて、それをもとに状態の初期化を行う
- エラー時に状態をサーバーに投げつけておくことで再現確認が容易になる

==

**event store風 &amp; serializable の恩恵**
- actionを投げつけるだけでコラボレーション環境を簡単に作れる
- undo実装や楽観的更新を簡単に作れる
- 過去の状態の復元や、コード変更時の状態の再評価
- dev toolsの作りやすさ

==

**ステートマシンを用意することの恩恵**
- ビジネスロジックの再利用性によるUI交換のしやすさ

===

#### 個人の感想

==

redux大好き!!

===

## context APIも必要ないかもしれない

==

って

==

作者が言ってました。

==

<a href="https://gyazo.com/a7c6eb83d9299b65f2ef09fefac314a8">
<img src="https://i.gyazo.com/a7c6eb83d9299b65f2ef09fefac314a8.png" alt="https://gyazo.com/a7c6eb83d9299b65f2ef09fefac314a8" width="800">
</a>

==

<a href="https://gyazo.com/78fd5ebaeb3b2f7949bfa4879a396719">
<img src="https://i.gyazo.com/78fd5ebaeb3b2f7949bfa4879a396719.png" alt="https://gyazo.com/78fd5ebaeb3b2f7949bfa4879a396719" width="900">
</a>

==

# 😇

==

意訳

> バケツリレーの代わりとして、そこかしこで使うものではないよ。
> 未解決の問題を解消するためのものだよ。
> これを使うべきだ、ってものではないんだよ。

===

#### 個人の感想

==

「reduxの代わりになる！」って記事が多いけど、そう思っちゃうってことはreduxの魅力をreact-reduxに感じてたってことであって、それってdan氏が思ってるreduxの魅力ではないわけで、つまり最初からredux要らなかったんじゃないのか。。。

==

<a href="https://facebook.github.io/relay/">
<img src="https://i.gyazo.com/f083cd53210f257a4095ba8b32f52f97.png" alt="https://gyazo.com/f083cd53210f257a4095ba8b32f52f97" width="100">
</a>

===

## じゃあいつcontect API使うんだろ 🤔

==

#### 個人の考え

==

- 多言語対応
- 🤔。。。 <!-- .element: class="fragment" data-fragment-index="2" -->
- 🤔。。。。。。 <!-- .element: class="fragment" data-fragment-index="3" -->
- 広範囲に及ぶ状態。。。 <!-- .element: class="fragment" data-fragment-index="4" -->

==

## きっとあります

===

## おまけ

==

<a href="https://twitter.com/KubaWojtach/status/978232928982503426">
<img src="https://i.gyazo.com/9dc28cbc8efb0973f1833b7baef16507.png" alt="https://gyazo.com/9dc28cbc8efb0973f1833b7baef16507" width="800">
</a>

==

<a href="https://twitter.com/dan_abramov/status/978236117970571264">
<img src="https://i.gyazo.com/407fab95a6dcc867f8bd55f0ecf9645e.png" alt="https://gyazo.com/407fab95a6dcc867f8bd55f0ecf9645e" width="800">
</a>

==

<a href="https://twitter.com/dan_abramov/status/978269123028373504">
<img src="https://i.gyazo.com/36a3d041e274d313492def803d47ae21.png" alt="https://gyazo.com/36a3d041e274d313492def803d47ae21" width="800">
</a>

===

おまけ2

==

<a href="https://github.com/jaredpalmer/formik">
<img src="https://i.gyazo.com/55da59a4294c9658972a076a7e056c15.png" alt="https://gyazo.com/55da59a4294c9658972a076a7e056c15" width="321">
</a>

==

- 「form置くたびにredux書くのだるい」
- 「redux-formだるい」

って方を幸せにするかもしれません。

==

<a href="https://github.com/renatorib/react-powerplug">
<img src="https://i.gyazo.com/55cd0ccf4f147afa331a3ebf63db785d.png" alt="https://gyazo.com/55cd0ccf4f147afa331a3ebf63db785d" width="456">
</a>

==

アコーディオンとか、モーダルとか、ちょっとtoggleだけできれば良いようなものを簡単に書けるかもしれません。

===

ご清聴ありがとうございました 🙇
