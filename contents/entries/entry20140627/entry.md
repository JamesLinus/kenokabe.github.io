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
#遅延評価(Lazy.js)とFRP(Bacon.js)とJavaScriptで時空なんでもマッピング！【時空プログラミング①】

今回はとりあえず、 *上沼恵美子のおしゃべりクッキング*みたいにやっていきます。

講釈は後でしっかり垂れてみようと思っています。


##材料

JavaScript (node.js ブラウザでもOK)

[Lazy.js](http://danieltao.com/lazy.js/) (遅延評価型関数ライブラリ)

[Bacon.js](http://baconjs.github.io/) (関数リアクティブプログラミング(FRP)ライブラリ)

##レシピ

0. **数学世界のコード** を **物理世界**に **マッピング**できる計算機（ **コンピュータ** ）を準備します。（お手持ちのパソコンのブラウザでOK！）
1. 数学世界のコードで **Lazy.js** の無限フライパンと **Bacon.js** の無限（時間軸方向）フライパンをそれぞれ準備します。
2.  **Lazy.js** のフライパンで無限数列の **自然数** `_natural`を作ります。
3.  **Bacon.js** のフライパンで時間軸上の無限数列の **ペースメーカー** `__timer`を作ります。
4. 出来上がった **自然数** `_natural`を **ペースメーカー** `__timer`にマッピングして **無限カウンター** `__a`を作ります。
5. **無限カウンター** `__a`に **２ｘ関数** `x2`の調味料をマッピングして **２x無限カウンター** `__b`を作ります。`__b = __a × 2` という数学の方程式の関係になっており、両辺は **常にイコール**です。
6. ここまでの作業で、時空をモデリングした数学世界モデルが出来ました。
7. 時間が流れるこの物理世界で **無限カウンター** `__a`と **２x無限カウンター** `__b`の時間軸上のデータを監視する設定をします。
8. 数学世界のコードを時間が流れるこの物理世界にマッピングする準備が整いました。完成です。

##試食(ブラウザ)
###時空をモデルリングした数学世界のコードを物理世界に
###[マッピング（コンピューティング）開始！](http://jsfiddle.net/s2h59/)


##数学世界のコード（数学的実体）

```
var _ = Lazy;
var __ = Bacon;

var natural = function(n)
{
  return n;
};

var _natural = _.generate(natural);
var __timer = __.interval(1000); //every second

var map_to__ = function(_seq, __seq)
{
  var it = _seq.getIterator();

  var sequence =
    __seq
    .map(function()
    {
      it.moveNext();
      return it.current();
    });

  return sequence;
};

var __a = map_to__(_natural, __timer);

var x2 = function(val)
{
  return val * 2;
};

var __b = __a.map(x2);

//Compute

__a.onValue(function(val)
{
  document.body.innerText += " " + val; // print every second
  //console.log(val);
});

__b.onValue(function(val)
{
  document.body.innerText += " " + val; // print every second
  //console.log(val);
});

```






#### KenOKABE tech blog
####[←ブログコンテンツ](http://kenokabe.github.io/contents/entries/entry0/entry.html)
