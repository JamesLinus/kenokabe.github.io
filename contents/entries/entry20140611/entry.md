## SwiftでiOS開発を関数型（宣言型）プログラミングへパラダイムシフトしよう
### KenOKABE tech blog
###[←コンテンツ](/contents/entries/entry0/entry.html)



<code>
println("hello World")

var a = 5

var plus1 = {
    (n: Int) -> Int in
    let x = n+1
    return x
}

[0,1,2].map(plus1)

var numbers = [0,1,2,3,4,5,6,7,8,9]

var result1 = numbers.map(plus1)
println(result1)


var plus1print = {
    (n: Int) -> Int in
    let x = n+1
    println(x)
    return x
}

var result2 = numbers.map(plus1print)

var result3 = numbers.reduce(0) { $0 + $1 }
println(result3)

</code>



###[←コンテンツ](/contents/entries/entry0/entry.html)
