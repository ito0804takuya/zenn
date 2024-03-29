---
title: "Go 型・関数"
emoji: "😸"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Go"]
published: true
---

# 型

## <組み込み型>

- 整数 :
  int
  byte（int8 のエイリアス）
  rune（int32 のエイリアス）
  int64
  uint（負数以外（0 以上）を扱う） など
- 浮動小数点数 : float32, float64
- 文字列 : string
  :::message
  ダブルクォーテーション (") で囲む。
  シングルクォーテーション (')で囲むのは、rune。
  :::
  :::message
  エスケープはバックスラッシュ (`\`)。
  リテラルはバッククォート (`)。
  :::
- 真偽値 : bool
  :::message alert
  他の言語のように**暗黙的に 0, 1 に変換しない**。**明示的に行う必要あり**。
  :::

## <型変換(キャスト)>

基本は、**strconv**パッケージを使う。

ただし、数値同士のキャストは`T(v)`でできる。

```go:T(v)
var f float64 = 10
var n int = int(f)
```

### よく使うもの

- `Atoi` : 文字列 → int（32 か 64 なのかは環境依存）
- `Itoa` : int → 文字列
  :::message
  A は ASCII コード を表す。
  :::

```go:Atoi, Itoa
i, _ := strconv.Atoi("-42") // Atoi は返り値が2つあり、2つ目は使わないため、 _ で、これから使わないことを明示。
s := strconv.Itoa(-42)
```

### あまり使わないもの

- ParseX : 文字列 → X
  - ParseInt : 文字列 → int（指定の bit 値への変換）
  - ParseFloat : 文字列 → float
  - ParseUint : 文字列 → uint
  - ParseBool : 文字列 → bool
- FormatX : X → 文字列
  - FormatInt : int8 など（int が指す bit 値ではない bit 値） → 文字列
  - FormatUint : uint → 文字列
  - FormatFloat : float → 文字列
  - FormatBool : bool → 文字列

## <型の確認>

型の確認は、`reflect.TypeOf()` でできる。

```go
fmt.Println( reflect.TypeOf(v) )
```

## <コンポジット型>

複合データ型。構造体、配列、スライス、マップ がある。

### 構造体

**型の異なる**データを集めたデータ型。

- 初期化方法 : `struct { name string, age int }`
- 操作 : 参照も代入も、`.`でアクセスできる。
  ```go
  var person struct {
    name string
    age  int
  }
  // .でアクセス
  person.name = "太郎"
  fmt.Println(person.name) // 太郎
  ```

### 配列

**同じ型**のデータを集めたデータ型。
**要素数は変更できない**。

- 初期化方法
  - `[2]string{"hoge", "fuga"}`
  - `[...]string{"hoge", "fuga"}`
  - `[...]int{2: 10, 5: 20}`
    `arr[i]`にあたる位置に値を指定する方法。この例だと`[0 0 10 0 0 20]`となる。
- 操作
  - アクセス : `arr[3]`
  - 長さ : `len(arr)`
  - スライス演算 : `arr[2:5]`
    配列からスライスを作る。この例だと`arr[2] から arr[5-1] まで`を抽出したものになる。
    ```go:スライス演算
    arr := [...]string{"a", "b", "c", "d", "e", "f"}
    fmt.Println(arr[2:5]) // [c d e]
    ```

:::details 2 次元配列

```go:2次元配列
func main() {
  var twoD [3][5]int

  for i := 0; i < 3; i++ {
    for j := 0; j < 5; j++ {
      twoD[i][j] = (i + 1) * (j + 1)
    }
    fmt.Println("Row", i, ":", twoD[i])
  }
  fmt.Println(twoD)
}
/* 出力
  Row 0 : [1 2 3 4 5]
  Row 1 : [2 4 6 8 10]
  Row 2 : [3 6 9 12 15]
  [[1 2 3 4 5] [2 4 6 8 10] [3 6 9 12 15]]
/*
```

:::

### スライス

**配列の一部を切り出した**データ型。
（スライスは配列を切り出したものなので、**背後には必ず配列が存在する**。）

:::message
スライスを使うときは、参照している（後ろの）配列を意識すること。
:::

配列との大きな違いは、スライスの**長さ**（サイズ）が固定ではなく**動的**であること。

スライスは下記 3 つの要素を持っている。（この 3 要素の構造体である。）

1. **ポインタ** : **基となる配列の最初の要素の場所**（**切り出した場所**）を表す。
2. **len** : 切り出した長さ。
3. **cap** : 基になる配列に対して、ポインタの位置（切り出した位置）から配列の終端までの要素数。つまり、このスライスはどこまで拡張できるのかを表す量。

- 初期化方法
  - `arr[2:5]`
    **スライス演算子**`s[i:p]`によって、（既にある）配列から切り出す方法。
    この例だと`arr[2] から arr[5-1] まで`を抽出したものになる。
    i と p は省略することもでき、配列の全要素をベースにする場合は`arr[:]`となる。
  - `[]int{1, 2, 3}`
    **スライスリテラル**で、基になる配列は指定せずにスライスだけを作る方法。
    （正しくは、内部的に`{1, 2, 3}`の配列が自動で作られ、それをスライスが参照している。）
  - `[]int{2: 10, 5: 20}`
    `slice[i]`にあたる位置に値を指定する方法。この例だと`[0 0 10 0 0 20]`となる。
  - `make([]int, 3, 10)`
    **make メソッド**によって**指定した要素数、容量のスライスを作る**方法。`make([]型名, length, capacity)`
- 操作
  - アクセス : `sl[3]`
  - 長さ : `len(sl)`
  - 容量 : `cap(sl)`
  - 要素の**追加** : `sl = append(sl, 50, 60)`
  - 要素の**削除**
    下記のどちらも、（削除したい要素を除外した）削除しない側の 2 つの要素郡をくっつけることで、要素を削除したスライスを作成している。
    - i 番目の要素のみ削除 : `a = append(a[:i], a[i+1]...)`
    - i~j 番目の要素を削除 : `a = append(a[:i], a[j]...)`

:::message

### append メソッドについて

```go:構文
<スライス> = append(<スライス>, <以降、追加したい要素>)
```

- 第一引数は追加するスライスで、第二引数以降は追加したい要素。
- 左辺はもちろん決まってはいないが、**そのスライス（変更を加えたスライス変数）で受ける**のがバグが起こらなく**安全**なので、（別のスライスで受けたりせずに）こういう構文だと覚えるのがいい。

#### [append の挙動]

- cap が足りる場合は、新しい要素を基となる配列の後ろにコピーして、len を更新すれば、要素を追加できる。
- cap が足りない場合は、後ろに追加したくてもできないので、**元の約 2 倍の容量の配列**（を置けるメモリ）**を確保し直す**。その新しい配列を参照するようにポインタを貼り直して、元の配列から要素をコピーする。そして、新しい要素をその配列にコピーして、len と cap を更新する。
  → なので、append メソッドの左辺を別のスライスで受けると、別の配列を参照してしまっているということになりかねない。

:::details コードでの解説

```go
// --------------------- スライスa ---------------------
a := []int{10, 20}
fmt.Println("A-1:", a, cap(a))

// --------------------- スライスb ---------------------
// aのスライスに30を追加したものを、別のbというスライスで受けると、どうなるのか？
b := append(a, 30)
fmt.Println("B-1:", b, cap(b)) // B-1: [10 20 30] 4
fmt.Println("A-2:", a, cap(a)) // A-2: [10 20] 2
// → aには追加されておらず、bにしか追加されていない。
// つまり、スライスaとbは、別の配列をベースとして参照している。
// これは、新しい要素30を追加するには、aのcapのままでは足りなかったため、配列を配置する容量を再確保したため。
// bはaとは別の空間（配列）を参照するようになってしまった。

// aに変更を加えた。bにもこの変更は伝わるのか？
a[0] = 100
fmt.Println("A-3:", a, cap(a)) // A-3: [100 20] 2
fmt.Println("B-2:", b, cap(b)) // B-2: [10 20 30] 4
// → bにaの変更が伝搬しなかった。スライスaとbは、別の配列を参照しているため、当然。

// --------------------- スライスc ---------------------
// aのスライスに30を追加したものを、別のbというスライスで受けた。
// が、bには1つ容量が空いている（cap(b)が4で、実際には10, 20, 30の3つしか使っていない）ので、
// 配列は再確保されず、bとcは同じ配列を参照する。
c := append(b, 40)
fmt.Println("C-1:", c, cap(c)) // C-1: [10 20 30 40] 4

// bに変更を加えたが、bとcは同じ配列を参照しているので、当然cにもこの変更は伝わる。
b[1] = 200
fmt.Println("B-3:", b, cap(b)) // B-3: [10 200 30] 4
fmt.Println("C-2:", c, cap(c)) // C-2: [10 200 30 40] 4
```

:::

:::message alert
append の挙動によって、**配列が再確保される場合がある**ので、**append は必ず元のスライスで受けること**。
:::

#### 空のスライスを作る方法

```go
p := []byte{}
```

#### 配列をコピーして使う方法
`copy(移す先のスライス, 元の配列から欲しい要素のスライス)`を使う。
```go
func main() {
  // 配列
  letters := [...]string{"A", "B", "C", "D", "E"}
  fmt.Println("Before", letters) // Before [A B C D E]

  slice1 := letters[0:2] // A, B

  slice2 := make([]string, 3) // cap=3と指定した 空のスライスが生成される
  // copy!!
  copy(slice2, letters[1:4]) // slice2にletters[1:4]をコピーする -> B, C, D

  slice1[1] = "X"

  fmt.Println("slice1", slice1) // slice1 [A X]
  fmt.Println("slice2", slice2) // slice2 [B C D] : 要素が変更されていない!

  fmt.Println("After", letters) // After [A X C D E] : slice1が基にしている配列は当然変更される
}
```


### マップ

**キーと値**をマッピングしたデータ型。

- 初期化方法
  - `map[string]int{ "hoge": 1, "fuga": 2 }`
  - `make(map[string]int)`
- 操作
  - アクセス : `m["x"]`
  - 存在確認 : `n, ok := m["z"]` で、n には値、ok には bool が入る。
  - 追加 : `m["z"] = 30`
  - 削除 : `delete(m, "z")`

:::message

## <ゼロ値>

| 型       | ゼロ値                 |
| -------- | ---------------------- |
| int      | 0                      |
| string   | ""                     |
| bool     | false                  |
| 構造体   | フィールドの型のゼロ値 |
| 配列     | 要素の型のゼロ値       |
| スライス | _nil_                  |
| マップ   | _nil_                  |

:::

## <ユーザ定義型>

`type 型名 基底型` で定義する。（基底型 : 基にする型。）

なので、ユーザ定義型というのは struct だけでなく、`type MyInt int`とかも定義できる。

# 関数

- 引数の型は、変数名の**後ろ**に書く。
- 2 つ以上の引数の型が同じ場合は、1 つに省略できる。
- 複数の戻り値を返せる。
  使わない戻り値は、ブランク変数`_`によって明示すること。Go の変数は必ず使わないといけないため。

```go
func sample(x, y int) (string, string) {
  // 処理
  // return ...
}
```

## Named return value

返り値の定義で変数を定義できる。
それで、それをそのまま関数内で使えるのはもちろん、return とするだけでその変数が返される。
```go
func greetingPrefix(language string) (prefix string) {
	switch language {
	case japanese:
		prefix = japaneseHelloPrefix
	case french:
		prefix = frenchHelloPrefix
	default:
		prefix = englishHelloPrefix
	}
	return
}
```

https://zenn.dev/yuyu_hf/articles/c7ab8e435509d2


## <ポインター>

### ポインターじゃないとどうなるか

多くの言語と同じように、Go は「値渡し」の言語。
つまり、`b := a`では（変数 b に変数 a のメモリ位置を共有している訳でなく、）変数 a の**値のコピー**を変数 b に格納している。
（_ローカルコピー（メモリ内の新しい変数）を作成している。_）

よって、**関数の引数で変数（値）を渡しても、その関数内での変更は呼び出し元に反映されない**。
その引数は、元の変数の値のコピーなので。

```go:関数に変数を渡しても呼び出し元に反映されない
func main() {
  first := "ジョン"
  updateName(first)
  println(first) // ジョン と出力される
}

func updateName(name string) {
  name = "田中" // "田中"に書き換えたつもりだが...
}
```

### ポインターの用途

ただ、そうじゃなくて「**関数内で値を変更したい**」というときに**ポインタ**を使う。
**ポインターでは、値ではなくアドレスメモリを渡す**。そうすることで、**呼び出し元にも反映される**。
（ポインターを使うことで、上記の updateName 関数で行う変更を main 関数の first 変数にも反映させることができる。）

**ポインター**は、**変数の格納先（メモリアドレス）を表す値**。

- **&演算子** : ポインタを**渡す**。
- **\*演算子** : ポインターを**参照**して、（ポインターが示す）格納先のオブジェクトへアクセスする。

```go:main.go
func main() {
  first := "ジョン"
  updateName(&first) // ポインター(メモリアドレス)を渡す
  println(first) // 田中 と出力される
}

func updateName(name *string) { // 注: 変数名でなく、型のとなりに*を書く
  *name = "田中" // ポインター先の文字列をupdate
}
```

:::message alert

#### スライス、マップ、チャネル はポインタを用いる必要がない（場合が多い）。

スライスの場合、`b := a`としても、スライス a が持つ**配列へのポインタ**を変数 b にコピーしているため、a と b は（背後にある）同じ配列を参照する。

ただし、（スライスでなく）**配列は、ポインタを使わないとアドレスを参照できない**。
配列の場合、`b := a`は、配列 a の持つ**値**を変数 b にコピーしている。
:::

:::message alert
ポインタが指すものが**配列の場合**、書き方に注意。

`*arr[i]` : NG（エラーになる）
`(*arr)[i]` : OK
:::

## <メソッド>

**メソッド**は、**レシーバ**（= 構造体 や その他何かのデータ）**に紐付けられた関数**。
メソッドによって、作成した構造体やデータに**動作**を追加できる。

**レシーバは、第 0 引数みたいなイメージ**。（なので変数の値のコピーが発生する。）

文法 : `func (変数 レシーバ) メソッド名() 返却型 { ... }`

```go:例
type triangle struct {
  size int
}

func (t triangle) perimeter() int {
  return t.size * 3
}

func main() {
  t := triangle{3}
  fmt.Println("この三角形の外周:", t.perimeter())
}
```

### レシーバにポインターを使う

メソッドに、変数でなくポインターを渡すほうが良い場合がある。（変数のアドレスを参照する。）

- メソッドで変数を更新する場合。
- 引数が（データ容量として）大きすぎる場合。→ そのコピーを回避したい。

```go:ポインタを使う
type triangle struct {
  size int
}

// ポインタを使わないメソッド
func (t triangle) perimeter() int {
  return t.size * 3
}

// ポインタを使うメソッド
func (t *triangle) doubleSize() {
  t.size *= 2 // return しない
}

func main() {
  t := triangle{3}
  t.doubleSize() // ポインタを参照して tが更新される

  fmt.Println("size:", t.size) // size: 6
  fmt.Println("perimeter:", t.perimeter()) // perimeter: 18
}
```

:::message
メソッドにポインターを使うこの用法はよく使われるので、シンタックスシュガーがある。

`(&t).doubleSize()` 本来はこう書かないといけないが、
`t.doubleSize()` こう書いても同じ意味になる。
:::

:::message alert
下記については詳細の記載を省略している。

- メソッド値 : メソッドを値として扱える。なので、変数にメソッドを格納して、それを別の場所で実行できたりする。
- メソッド式 : おおよそメソッド値と同じようなこと。違いは、レシーバでなく型を渡しているだけなので、メソッド式にはレシーバを第一引数に渡す必要がある。
  :::

### 埋め込まれた構造体のメソッドを呼び出すことができる

```go:埋め込まれた構造体のメソッドを呼び出す
type triangle struct {
  size int
}

type coloredTriangle struct {
  triangle // 構造体を埋め込む
  color string
}

func (t triangle) perimeter() int {
  return t.size * 3
}

func main() {
  t := coloredTriangle{triangle{3}, "blue"}
  fmt.Println("size:", t.size)
  // coloredTriangleが、triangleのメソッドも使える
  fmt.Println("perimeter:", t.triangle.perimeter())
  // fmt.Println("perimeter:", t.perimeter()) // ↑の書き方のシンタックスシュガー
}
```

また、**埋め込んだ構造体のメソッドをオーバーロード**することもできる。
※ オーバーロード(多重定義) : 同じ名前の関数等を定義すること。

その場合、下記のように、埋め込んだ側・埋め込まれた側どちらのメソッドも使用することができる。

```go:埋め込んだ構造体のメソッドをオーバーロード
type triangle struct {
  size int
}

func (t triangle) perimeter() int {
  return t.size * 3
}

type coloredTriangle struct {
  triangle
  color string
}

func (t coloredTriangle) perimeter() int {
  return t.size * 5
}

func main() {
  t := coloredTriangle{triangle{3}, "blue"}
  fmt.Println("size:", t.size)

  // coloredTriangleで、coloredTriangleのメソッドを使う
  fmt.Println("perimeter:", t.perimeter()) // perimeter: 15

  // coloredTriangleで、triangleのメソッドも使える
  fmt.Println("perimeter:", t.triangle.perimeter()) // perimeter: 9
}
```

:::message
Goには**継承は無い**。
↑の**構造体の埋め込み**で行っているのは**継承ではなく、委譲**。
:::