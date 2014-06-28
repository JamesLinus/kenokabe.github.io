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
#自然数と自然数+1 で偶数と奇数を作る【時空プログラミング③】

自然数`natural`を作る

自然数をインクリメントした`natural1`を作る

偶数`even` = `natural1` + `natural1`

奇数`odd` = `natural` + `natural1`

##コード(node.js)
```
var _ = require('lazy.js');

//need this extention of lazy.js
_.mapSequences = function(_seqA, _seqB, f)
{
  var itA = _seqA.getIterator();
  var itB = _seqB.getIterator();

  var ff = function(n)
  {
    itA.moveNext();
    var a = itA.current();
    itB.moveNext();
    var b = itB.current();

    return f(a, b);
  };

  var sequence = _.generate(ff);
  return sequence;
};
//-----------------------

var natural = function(n)
{
  return n;
};
var increment = function(n)
{
  return n + 1;
};

var _natural = _.generate(natural); //0 1 2 3 4 5 .....
var _natural1 = _natural.map(increment); //1 2 3 4 5 6 .....

var add = function(a, b)
{
  return a + b;
};

var _even = _.mapSequences(_natural1, _natural1, add); //2 4 6 8 10 12....
var _odd = _.mapSequences(_natural, _natural1, add); //1 3 5 7 9 11 .....

var even10 =
  _even
  .take(10)
  .toArray();

var odd10 =
  _odd
  .take(10)
  .toArray();

console.log(even10);
console.log(odd10);
```
