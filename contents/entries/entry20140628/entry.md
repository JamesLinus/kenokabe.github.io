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

#タイマーを無限個作り、データとして取り回す【時空プログラミング②】


前回とは逆に、

Lazy.jsの無限シークエンスにBacon.jsのタイマー（時間軸上で無限）をマッピングしてみます。

###数学モデル

１秒インターバルのタイマーを用意。

それを無限個つくり、タイマーの無限シークエンスとする。

その無限シークエンスにタイムスタンプ表示機能を持つ関数をマッピングする。

その無限シークエンスから10個だけ切り出す。

###物理世界へマッピング（コンピュート）

10個のタイマーシークエンスを物理世界に展開（toArray()）

##デモ（ブラウザ）
http://jsfiddle.net/VqtR5/


##コード(node.js)

```
var _ = require('lazy.js');
var __ = require('baconjs');

var __timer = __.interval(1000); //every second

var timer = function()
{
  return __timer;
};

var _timers = _.generate(timer); //Infinite sequence of timers

var f = function(timer)
{
  timer
    .onValue(function(x)
    {
      console.log(require('moment')().format('MMMM Do YYYY, h:mm:ss'));
    });
};

_timers
  .map(f)      //every timer shows timestamp per second
  .take(10)    //only 10 timers within infinite sequence
  .toArray();  //compute
```
