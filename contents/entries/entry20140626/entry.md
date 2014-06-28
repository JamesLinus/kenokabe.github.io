#### KenOKABE tech blog
####[←ブログコンテンツ](http://kenokabe.github.io/contents/entries/entry0/entry.html)

#記事シリーズ
#### [Swiftで脱アルゴリズム！iOS開発を関数型（宣言型）プログラミングへパラダイムシフトしてみる【脱アルゴリズム宣言①】](http://qiita.com/kenokabe/items/41189c45001321c9e283)
####  [関数リアクティブプログラミング（FRP）で分断された2つの世界を繋ぐ【脱アルゴリズム宣言②】](http://qiita.com/kenokabe/items/a8477694a499ca869cde)
#### [関数型（宣言型）プログラミングで無限をコーディングする「遅延評価」のわかりやすい解説【脱アルゴリズム宣言③】](http://qiita.com/kenokabe/items/821ce4020644372b648c)

#### [LISPデータ構造の問題点と抜本的な解法としての新プログラミング言語の策定　純粋関数型言語「SpaceTime」 ドラフト](http://qiita.com/kenokabe/items/aa5705978d6a13753fe2)

#### [遅延評価(Lazy.js)とFRP(Bacon.js)とJavaScriptで時空なんでもマッピング！【時空プログラミング①】](http://qiita.com/kenokabe/items/b04e3d8d49b0ffc7a78b)

#### [タイマーを無限個作り、データとして取り回す【時空プログラミング②】](http://qiita.com/kenokabe/items/8c970d2b0dfa98187998)

#### [自然数と自然数+1 で偶数と奇数を作る【時空プログラミング③】]( http://qiita.com/kenokabe/items/f6172df8d8416429656a)

#### [数学と別離したプログラミング、時間を抽象化し数学を取り戻すプログラミング【時空プログラミング④】](http://qiita.com/kenokabe/items/b81c7aa8af86314551a0)

# LISPデータ構造の問題点と抜本的な解法としての新プログラミング言語の策定　純粋関数型言語「SpaceTime」 ドラフト

ジョン・マッカーシーが開発したLISPは

(関数 引数 引数 引数)

という

[S式](http://ja.wikipedia.org/wiki/S%E5%BC%8F)
で表記され、プログラミング構文が存在しません。

```
> (+ 1 2)
3
```


データ=コード
コード＝データ
という（一見）美しい構造で、コードを変更するにはデータを変更すればよい、というコード自身を第一級（ファーストクラス）オブジェクトとして扱うことができます。

最近、巷で若干注目を集めている、Javaでも実装されはじめた [リフレクション](http://ja.wikipedia.org/wiki/%E3%83%AA%E3%83%95%E3%83%AC%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3_(%E6%83%85%E5%A0%B1%E5%B7%A5%E5%AD%A6))
の最上級機能を言語構造として先天的に実装しているようなもので、自己定義する人工知能研究の分野（マッカーシーの専門分野）でよく使われてきたようです。
　
しかし、私がLISPを関数型言語の大御所として試すなかで問題だと思った事をざっくばらんに書きます。

すでに【脱アルゴリズム宣言】シリーズで述べたように、関数型パラダイムは、まず最初にデータをまるごと用意し、そこに関数操作を加えていくという宣言をするという思考・指向でした。

こういうデータをまるごと用意する、データのコレクション、リストという発想は、LISPのリストがオリジナルなのですが、遺憾なことに

(関数 引数 引数 引数)

っていうのが命令型パラダイムであるのに気づくでしょうか？

(Do what what what)

という構造になっている。

関数型パラダイムであるならば、こう書くべきでしょう。

(What Do Do Do)

(データ 関数 関数 関数)

そして、実際のところ

(関数 引数 引数 引数)

というデータ構造のために様々な目に見える弊害がLISP/Schemeにはあります。

(関数 引数 引数 引数)とS式の最初が（データでなく）関数と決め打ちしているので、

(データ　データ　データ)　たとえば

```
(1 2 3)
```

というデータをそのまま書くとエラーになるんですね。だってまず `1`を関数として実行しようとするけど、関数ではないので。


すべてがデータでありコードであり、コード自身を第一級（ファーストクラス）オブジェクトとして扱うことができる、という看板の割にあまりにもお粗末な結果です。

これを回避するために、

```
>(list 1 2 3)
(1 2 3)
```

という、見た目、カッコの位置がずれてデータ構造が変わってしまう方法
だったり

カッコの位置が一緒のデータ構造を維持しようと思えば、

```
> (quote (1 2 3))
(1 2 3)
```
あるいは、quoteのショートカット記号を用いて

```
> '(1 2 3)
(1 2 3)
```

と書く必要があります。このクオート記号すぐ打つの忘れるんですよね。非常に面倒くさい。美しくない。

そしてよく考えてみると、S式の要件定義によれば、

(関数 引数 引数 引数)

なので、

この

```
(quote (1 2 3))
```
の中身のかっこ(1 2 3)
はS式じゃないだろ！ってことになります。

そもそも(1 2 3)ってそのまま書いたらエラーになるから、quote付けたけど、定義どうりならばどっちにせよ無理で、

最初の

(list 1 2 3)

って書くしか整合性を保つ方法がない！ってことになります。
どうなってるのか？

実は、

(quote (1 2 3))

の場合、引数である
(1 2 3)
っていうのは、S式として評価されません。

S式でない評価しない特別なデータとして扱います。例外です。

例外、美しくないですね。余剰なルールが追加されるわけで単純さが損なわれるからです。

こんな調子では、

「すべてがデータでありコードであり、コード自身を第一級（ファーストクラス）オブジェクトとして扱うことができる」

と鵜呑みにして統一的に操作できると突き進んでも、いろいろ面倒なパッチを加えながらやるしかない未来が待ち受けているのは明々白々な状況です。

- 命令型オリエンテッドなS式の表記

- 破綻したデータ表現

どうにか回避策は無いものか？と試行錯誤しましたが

「ありませんでした」

「そして根本的な理由がわかりました」

S式のリストは、
[Singly_linked_list](http://en.wikipedia.org/wiki/Singly_linked_list#Singly_linked_list)
というリスト構造になっています。
![](http://upload.wikimedia.org/wikipedia/commons/6/6d/Singly-linked-list.svg)

LISPのすべてを束縛するS式の表記は、この[Singly_linked_list](http://en.wikipedia.org/wiki/Singly_linked_list#Singly_linked_list)
というリスト構造と等価であり、問題を解決するにはここからどうにかしないといけない。


LISPのリストで
((a b c)(d e f)(g h i))
は、
http://www.ki.nu/OHP/dot.emacs/list-drawing.html
（ToDo　図作成）

のようになっており、データの末尾を示すため終端にnilという空データをにつけます。

これは美しくない。

- データの末尾を示すため終端にnilという空データをにつける

というのもルールの一つです。

このルールどおりのものをPureList（純リスト）とし、LISPのS式はそれで構成されているのですが、このルールをぶちやぶって、任意のアトムを終端につけることだって可能です。この場合は
PureListじゃないので各種操作は破綻する。

セルを自由自在に組み合わせられる、柔軟な表現方法だとおもいきや、こういうルールがある。
そしてルールどおりにするならば、新たなリストを既存のリストに結合する際には、終端のnilを一旦外し、そこに接続するという２ステップの置換になる。
自由に接続じゃなくて、置換です。

何かがおかしい、何もかもがしっくりこない、と思ったので、思案の上、自分が理想とする純粋関数型言語をJavaScript上で書きました。

LISPを純粋関数型言語として全面的に書きなおす。

LISPと逆方向のリスト構造を基盤とする。

リストは`EmplyPair`という無限再帰構造をもつ`0`に相当するペアから始まる任意の構造である。

終端に特別な操作がない、特別ルールがないのですべての構造がPureである。

遅延評価であり、必要な構造のみ随時評価されていく。

関数はファーストクラスオブジェクトであり、さらにLISPのように関数とデータの区別はしない

すべての関数はデータである。

すべてのデータは関数である。

`(1 2 3)`というデータは`(1 2 3)`とクオート無しでクリーンにデータどおり表記され遅延評価される。。

もはやLISPとは別物の新言語である。

JSで書かれている、JSで動作する。

#SpaceTime Programming Language
The Spacetime Operating Language

![](http://kenokabe.github.io/stability-badges/dist/experimental.svg)

***SpaceTime***
#### - is a Functional Reactive Programming (FRP) language
#### - employs Lazy evaluation strategy
#### - runs on JavaScript Engines (browsers & node.js)
#### - is written in JavaScript

##Hello world
---

###Qiitaではスクリプトが許可されていないので
###[動作デモはプロジェクトサイト](http://spacetimeprogramminglanguage.github.io/)


##Foundation
---

*Inspired by John McCarthy's LISP*

*SpaceTime* is founded on an inverted data structure of
[Singly linked list](http://en.wikipedia.org/wiki/Singly_linked_list#Singly_linked_list) and [S-expression](http://en.wikipedia.org/wiki/S-expression).

###Pair

![001](http://spacetimeprogramminglanguage.github.io/contents/img/001.svg)

This is the fundamental unit of *SpaceTime*.
`Pair` has a pair of hands to point a pair of any objects.

---
###Pair points a pair of objects

![](http://spacetimeprogramminglanguage.github.io/contents/img/002.svg)

Now, a `Pair` points objects: `a` and `b`.

---
###Pair notation
![](http://spacetimeprogramminglanguage.github.io/contents/img/003.svg)

When a `Pair` points objects: `a` and `b`, it's expressed as **{a b}** in `Pair notation`.

---
###Pair can point itself

![](http://spacetimeprogramminglanguage.github.io/contents/img/004.svg)

---
###Empty Pair

![](http://spacetimeprogramminglanguage.github.io/contents/img/005.svg)



When a Pair points itself, since it's a form of [Self-reference](http://en.wikipedia.org/wiki/Self-reference), an  [Infinite recursion](http://en.wikipedia.org/wiki/Infinite_loop#Infinite_recursion) occurs.

Accordingly, the `Pair notaion` is { { {...} {...} } { {...} {...} } }, so we simply express the entity as **{ }**, and let's call it `Empty Pair`.

---
###Push Pair

![](http://spacetimeprogramminglanguage.github.io/contents/img/006.svg)

A `Pair` can point another `Pair` so that we can joint `Pair`s.

Now let a `pair` point another `Pair` and `5` by each hands.
Let's call this special action `Push`.

In this case, we `push` 5 to an `Empty Pair`.

The `Pair notation` is **{ {} 5 }**.


---
###Push another Pair

![](http://spacetimeprogramminglanguage.github.io/contents/img/007.svg)

In the same manner, we can `push` another `Pair`.

We `push` `2` to the previous sequence.

The `Pair notation` is **{ { {} 5 } 2 }**.

---
###Push to any sequence

![](http://spacetimeprogramminglanguage.github.io/contents/img/008.svg)

---
###Sequence

![](http://spacetimeprogramminglanguage.github.io/contents/img/009.svg)

Here, *SpaceTime* explicitly defines a term `Sequence` for this form.
Please note this is the same term and meaning of [Sequence](http://en.wikipedia.org/wiki/Sequence) in Mathematics.

At the same time, instead of `Pair notation` : { { { {} 5 } 2 } 7 }, it can be simply expressed as **( 5 2 7 )**.

---
###Push function

![](http://spacetimeprogramminglanguage.github.io/contents/img/010.svg)

A `Pair` can point `function`.

Accordingly, we can `push` `function` to any `Sequence`.

In this case, we `push` a `function` to **(5)**.

---
###A case of Push function

![](http://spacetimeprogramminglanguage.github.io/contents/img/011.svg)

When we push a `function` : `plus2` to **(5)**, the result is **(7)**.

---
###Function is Sequence
The `function` : `plus2` is fundamentally some `Sequence`.

`plus2` consists of a `Sequence` : **( plus (2) )**.

**(2)** is an `attribute Sequence` of the `function`.

**( 5 (plus (2)) )** is equivalent to **(7)**.

---
### Everything is a function and Sequence

![](http://spacetimeprogramminglanguage.github.io/contents/img/012.svg)


Everything is `function` in *SpaceTime*.

In this case, `3` is a `function` that maps a source : **( 5 )** to a target : **( 5 3 )**.

Consequently, since `function` is `Sequence` in *SpaceTime*, everything is `Sequence` in *SpaceTime*.

Therefore, `3` is a `function` and at the same time, is a `Sequence`.

However, `3` is `3`. There is no other way to express than just `3` in *SpaceTime*.



---
###Every Sequence is a result of function to `Empty Pair`

![](http://spacetimeprogramminglanguage.github.io/contents/img/013.svg)

---
###Function composition

![](http://spacetimeprogramminglanguage.github.io/contents/img/014.svg)

[Function composition](http://en.wikipedia.org/wiki/Function_composition) is naturally expressed in a form :
**( *source function fucnction )** in *SpaceTime*.

Please note the `source` = **( 1 )** as a `Sequence`, not 1.

---
###1 +2 +3 = 6

![](http://spacetimeprogramminglanguage.github.io/contents/img/015.svg)

*SpaceTime* has a short-cut notation : **+** corresponding to `plus` function.

---
###1 +(2 +3) = 6

![](http://spacetimeprogramminglanguage.github.io/contents/img/016.svg)

( 1 (+ ( 2 (+ ( 3 ) ) ) ) )  =  ( 6 )

---
###Indefinite sequence

![](http://spacetimeprogramminglanguage.github.io/contents/img/017.svg)

*SpaceTime* employes an [evaluation strategy](http://en.wikipedia.org/wiki/Evaluation_strategy) : [lazy evaluation, or call-by-need](http://en.wikipedia.org/wiki/Lazy_evaluation).

Accordingly, *SpaceTime* can deal with Indefinite sequence such as [Natural number](http://en.wikipedia.org/wiki/Natural_number).

---
###I/O (Input and Output)

![](http://spacetimeprogramminglanguage.github.io/contents/img/018.svg)

[I/O](http://en.wikipedia.org/wiki/Input/output) does not affect the evaluation context of *SpaceTime* directly, so that it can preserve [Functional programming](http://en.wikipedia.org/wiki/Functional_programming) paradigm.

*SpaceTime* employes [FRP](http://en.wikipedia.org/wiki/Functional_reactive_programming) or `SpaceTime Programming paradigm`.

---


##Basic
---
###Hello world
`Code`
```
(
  "hello World"
  (map (CONSOLE))
)
```
`Evaluation`
```
("hello World")
```
`Console`
```
hello World
```
---
###Hello world twice

`Code`
```
(
  "hello World"
  (map (CONSOLE))
  (map (CONSOLE))
)
```
`Evaluation`
```
("hello World")
```
`Console`
```
hello World
hello World
```

---
###Boolean

`Code`
```
(3 (== (3)) )
```
`Evaluation`
```
(true)
```


`Code`
```
(3 (== (5)) )
```
`Evaluation`
```
(false)
```


`Code`
```
(2 (> (1)) )
```
`Evaluation`
```
(true)
```


`Code`
```
(2 (< (1)) )
```
`Evaluation`
```
(false)
```


---
###Conditional

`Code`
```
(
　　3 (==(3))　
   (
     if (
           ("match")
           ("mismatch")
        )
   )
)
```
`Evaluation`
```
("match")
```


`Code`
```
(
　　3 (==(5))　
   (
     if (
           ("match")
           ("mismatch")
        )
   )
)
```
`Evaluation`
```
("mismatch")
```


`Code`
```
(
　　3 (==(5))　
   (
     if (
           ("case1")
           (
           　　3 (==(3))　
              (
                if (
                      ("case2")
                      (
                      　　3 (==(3))　
                         (
                           if (
                                 ("case3")
                                 ("mismatch")
                              )
                         )
                      )
                   )
              )
           )
        )
   )
)

```
`Evaluation`
```
("case2")
```


`Code`
```
(
　　3 (==(5))　
   (
     if (
           ("case1")
           (
           　　3 (==(1))　
              (
                if (
                      ("case2")
                      (
                      　　3 (==(3))　
                         (
                           if (
                                 ("case3")
                                 ("mismatch")
                              )
                         )
                      )
                   )
              )
           )
        )
   )
)

```

`Evaluation`

```
("case3")
```






---





#### KenOKABE tech blog
####[←ブログコンテンツ](http://kenokabe.github.io/contents/entries/entry0/entry.html)
