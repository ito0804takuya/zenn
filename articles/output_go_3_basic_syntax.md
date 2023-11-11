---
title: "Go 基本構文・制御構文"
emoji: "🙆"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Go"]
published: true
---

# 基本構文

## <変数>

```go
// 1: 宣言と代入を分ける方法
var age int
age = 20

var firstName, lastName string
firstName = "ichiro"
lastName = "tanaka"


// 2: 宣言をまとめる方法（代入は別々）
var (
  age int
  firstName, lastName string
)
age = 20
firstName = "ichiro"
lastName = "tanaka"

// 3: 宣言と代入を一度に行う方法
var age = 20
var (
  firstName = "ichiro" // 型が推定される
  lastName = "tanaka"
)

// 4: var でなく、:= を使う方法
age := 20
firstName, lastName := "ichiro", "tanaka"

age = age / 3 // = 6 になる（int = 整数 のため小数が切り捨てられた）
```

:::message
値は `_` で区切れる。（例：`1_000_000`）
:::

:::message alert

### 変数のゼロ値

Go には、変数を初期化したときに勝手に代入されるデフォルト値があり、それをゼロ値という。

- int : `0`
- bool : `false`
- string : `""`
- error : `nil`
  :::

## <定数>

（変数は var だが）定数は const 。

```go
const limit = 100
const (
  StatusOk = 0 // 変数のように := は使わない!!
  StatusError = 1
)
```

## <演算子>

代入演算（`+=`, `++`など）、論理演算（`||`, `&&`など）、比較演算（`!=`, `<=`など）は
（私の知っている）他の言語と変わらない。

### ビット演算

https://www.flyenginer.com/low/go/go%E3%81%AE%E3%83%93%E3%83%83%E3%83%88%E6%BC%94%E7%AE%97%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6.html

Web 開発ではあまり登場しないと思うので説明は上記リンク先にお任せするが、
ビット演算とは何をすることなのか、少しだけ下記コードで説明。

```go:否定(^)だけピックアップ
one := 3 // 2進数表現では「0000 0011」
fmt.Println(one) // 3
fmt.Println(^one) // -4
// -3 を2進数に変換するときは「3の2進数表現をすべて反転して、1を足す」ので、
// 3の否定 である「1111 1100」は、-4 となる
```

### アドレス演算

- `&` : ポインタを取得。
- `*` : ポインタ（`*string`など）に対して`*`でアクセスすると、ポインタが指す値を取得する。

:::message
```go
a := "テスト"
fmt.Println(a) // 文字列

b := &a
fmt.Println(b) // aのポインタ
fmt.Println(*b) // 文字列
```
このとき、`b`（`&a`）の型は`*string`。

つまり、↓の2行は等価。
```go
b := &a
var b *string = &a
```

`*string`に対して、`*`でアクセスするとポインタが指す値（ここではstring）を取得できる。
:::

### チャネル演算

- `<-` : チャネルへの送受信

# 制御構文

## <if 文>

```go
// if文で代入文を書ける（変数を定義して、それをifブロック内で使える）
if num := givemeanumber(); num < 0 {
  fmt.Println(num, "is negative")
} else if num < 10 {
  fmt.Println(num, "has only one digit")
} else {
  fmt.Println(num, "has multiple digits")
}

// if文で定義した変数numは、ifブロック外で使えない
fmt.Println(num) // エラー undifed: num
```

## <switch 文>

switch 句にも case 句にも、（スカラ値はもちろん）式・関数を使える。

```go:switch句に式を使用
switch time.Now().Weekday().String() {
case "Monday", "Tuesday", "Wednesday", "Thursday", "Friday":
  fmt.Println("平日")
default:
  fmt.Println("休日")
}
```

デフォルトでは、1 つの case が実行されると break され他の case は評価しない。
（頻度は低いと思うが、）*他の case をまたぎたい*ときは、`fallthrough`を使う。
ただし、複雑なので、使わないほうがいいと思う。

```go:fallthroughの説明用
switch num := 1; {
case num < 10: // case1
  fmt.Printf("%d は10以下\n", num)
  fallthrough
case num > 20: // case2
  // 1つ上のcase(case1)でfallthroughを使っているため、caseに合致しない（numは20以上ではない）が、実行される
  fmt.Println("20以上")
case num > 30: // case3
  // case2にて既にbreakされているため評価・実行されない
  fmt.Println("30以上")
case num < 40: // case4
  // 条件には合致するが、case2にて既にbreakされているため評価・実行されない
  fmt.Println("40以下")
}
// -> 1 は10以下
//    20以上
```

## <for 文>

:::message
Go には、**繰り返しは for しか無い**。
（while,each などは無く、forで実現する。）
:::

```go:シンプルなfor
for i := 0; i <= 10; i++ {
}
```

```go:while
for i < 10 {
  // ...
  i++
}
```

```go:each
// itemsは、配列やmap
for i, v := range items {
}
```

- break : ループから抜ける。
- continue : continue以下の処理は行わず、次のループに移る。
- **defer** : **スタック的に保管され、遅延実行**される。**引数の評価はdefer宣言時**。ただし、基本的に**forの中でdeferを呼んではいけない**。forを抜けるまでdeferは実行されないので、例えばファイルが長時間開きっぱなしのままになる可能性がある。
