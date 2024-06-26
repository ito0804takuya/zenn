---
title: "Go_構造体, 関数, インターフェース"
---

# 構造体

**型の異なる**データを集めたデータ型。

## 宣言・初期化・アクセス

```go
// 宣言
type User struct {
  Name string
  Age  int
}

// 初期化
u := User{Name: "太郎", Age: 20}

// 代入・参照
u.Age = 30
fmt.Println(u.Name, u.Age) // 太郎 30
```

## 構造体の埋め込み

```go
type Person struct {
  ID        int
  FirstName string
  LastName  string
}

type Employee struct {
  Person // Personを埋め込む
  ManagerID int
}

employee := Employee{
  // 初期化の際は、Personフィールド（構造体）を明示しないといけない
  Person: Person{
    FirstName: "john",
  },
}
fmt.Println(employee) // {{0 john } 0}

// 初期化でないので、Person経由でなくてOK
employee.LastName = "doe"
fmt.Println(employee) // {{0 john doe} 0}
```

## 構造体 ↔JSON

- json.Marshal() : 構造体を json に変換
- json.Unmarshal() : json を構造体に変換

### json タグ

```go
type Person struct {
  ID        int
  FirstName string `json:"name"`
  LastName  string `json:"lastname,omitempty"`
}

type Employee struct {
  Person
  ManagerID int
}

employees := []Employee{
  {
    Person: Person{
      LastName: "hoge", FirstName: "john",
    },
  },
  {
    Person: Person{
      LastName: "fuga", FirstName: "bob",
    },
  },
}

data, _ := json.Marshal(employees) // 構造体をjsonに変換
fmt.Printf("%s\n", data)
// [
//   {"ID":0,"name":"john","lastname":"hoge","ManagerID":0},
//   {"ID":0,"name":"bob","lastname":"fuga","ManagerID":0}
// ]


var decorded []Employee
json.Unmarshal(data, &decorded) // jsonを構造体に変換(戻す)
fmt.Printf("%v", decorded)
// [
//   {{0 john hoge} 0}
//   {{0 bob fuga} 0}
// ]
```

# 関数

## Named return value

返り値の定義で変数を指定すると、それをそのまま関数内で使え、return 時にはそれが返される。

```go
func greetingPrefix(language string) (prefix string) {
	switch language {
	case japanese:
		prefix = japaneseHelloPrefix
	default:
		prefix = englishHelloPrefix
	}
	return
}
```

https://zenn.dev/yuyu_hf/articles/c7ab8e435509d2

# メソッド

`func (変数 レシーバ) メソッド名() 返却型 { ... }`

メソッドは、**レシーバ**（= 構造体 や その他何かのデータ）**に紐付けられた関数**。
メソッドによって、作成した構造体やデータに動作を追加できる。

**レシーバは、第 0 引数みたいなイメージ**。（なので変数の値のコピーが発生する。）

## レシーバにポインターを使う

メソッドに、変数でなくポインターを渡すほうが良い場合がある。（変数のアドレスを参照する。）

- メソッドで変数を更新する場合。
- 引数が（データ容量として）大きすぎる場合。→ そのコピーを回避したい。

```go
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

t := triangle{3}
t.doubleSize() // ポインタを参照して tが更新される

fmt.Println("size:", t.size) // size: 6
fmt.Println("perimeter:", t.perimeter()) // perimeter: 18
```

:::message
メソッドにポインターを使うこの用法はよく使われるので、シンタックスシュガーがある。

`(&t).doubleSize()` 本来はこう書かないといけないが、
`t.doubleSize()` こう書いても同じ意味になる。
:::

ポインタを使わない場合のレシーバ のことを**値レシーバ**、使う場合は**ポインタレシーバ**という。

```go
// 値レシーバ
func (t triangle) perimeter() int { }

// ポインタレシーバ
func (t *triangle) doubleSize() { }
```

:::message
**基本的にはポインタレシーバを使うこと**。

- 大きなデータのコピーが発生しないため。
- メソッドがインスタンスの状態を変更する場合。値の変更が簡単。

逆に言うと、値レシーバを使うのは下記の場面。

- メソッド呼び出しが**オリジナルのインスタンスに影響を与えてはいけない場合**。
- レシーバがプリミティブ型など単純な型の場合。

また、**どっちを使うのか、1 つの型の中で混在させない（統一する）こと**。
:::

:::message alert
下記については詳細を省略。

- メソッド値 : メソッドを値として扱える。なので、変数にメソッドを格納して、それを別の場所で実行できたりする。
- メソッド式 : おおよそメソッド値と同じようなこと。違いは、レシーバでなく型を渡しているだけなので、メソッド式にはレシーバを第一引数に渡す必要がある。

:::

## 構造体の埋め込み

```go
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

t := coloredTriangle{triangle{3}, "blue"}

fmt.Println("size:", t.size) // triangleのメソッドも使える

fmt.Println("perimeter:", t.triangle.perimeter())
fmt.Println("perimeter:", t.perimeter()) // ↑の書き方のシンタックスシュガー
```

また、**埋め込んだ構造体のメソッドをオーバーロード**することもできる。
※ オーバーロード(多重定義) : 同じ名前の関数等を定義すること。

その場合、下記のように、**埋め込んだ側・埋め込まれた側どちらのメソッドも使用することができる**。

```go
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

t := coloredTriangle{triangle{3}, "blue"}

// coloredTriangleのメソッドを使う
fmt.Println("perimeter:", t.perimeter()) // perimeter: 15

// triangleのメソッドも使える
fmt.Println("perimeter:", t.triangle.perimeter()) // perimeter: 9
```

:::message
Go には**継承は無い**。
↑ の**構造体の埋め込み**で行っているのは**継承ではなく、委譲**。
:::

### カプセル化

パッケージ外からは public（頭文字が大文字）なメソッド・フィールドしか参照できないため、それによってカプセル化を行う。

# インターフェース

`type インターフェース名 interface { ... }`

- Go では**インターフェースでしか抽象化できない**。
- インターフェースを実装した具象を定義する際、Java とかみたいに`implements`などのキーワードはいらない。
  逆に言うと、**この型がそのインターフェースを実装しているのかどうか**が分からなくなるが、下記のようにしてチェックできる。

  ```go:インターフェースを満たすかチェック
  // 独自型を定義
  type Func func() string

  func (f Func) String() string { return f()}

  // これがfmt.Stringerインターフェースを満たすか確認する
  // （fmt.Stringer はStringメソッドを持つインターフェース）

  // もし等しくなければコンパイルエラーが起きるので、インターフェースを満たしていないということが分かる。
  // チェックのためだけに使うのでFunc()にはnilを渡し、返り値も使わないので _ で受け取っている。
  var _ fmt.Stringer = Func(nil)
  ```

### empty interface

empty interface（`interface{}`）を使うと、どの型の値も実装していることになる。
つまり、**何でも入れれる箱**が作れる。

```go
var v interface{}
v = 100
v = "hoge" // 普通なら代入できないが、できる
```

（空のインターフェースというのはつまり、インターフェースを満たす条件となるメソッドを定義していないということ。int や string など色々な型は、**空のインターフェースを既に実装している**ということになるので、何でもボックスになる。）

ただし、それをすると型の恩恵が受けれなくなるため、**基本使うべきではない**。どうしてもそうしたい場合のみ使う。

## インターフェースの設計

:::message

- **メソッドリストは小さく**するのが良い。
  - **共通点を抜き出して抽象化しない**。
  - **1 つの振る舞い（塊）を 1 つのインターフェース**にする。
  - **型を使うクライアント（ユーザ）が触れる部分がインターフェースでなくてもよい**。

:::

## インターフェースの合成

構造体だけでなく、インターフェースも埋め込み（合成）できる。

```go
type Reader interface { Read(p []byte) (n int, err error) }
type Writer interface { Write(p []byte) (n int, err error) }
type ReadWriter interface {
	Reader
	Writer
}
```

## 利用者側で最小のインターフェースを定義する

（他の多くのオブジェクト指向言語と異なり）Go のインターフェースは、**実装側（構造体）だけを見ても、それがどのインターフェースを満たしているのかが分からない**。

だが、**利用するメソッドだけを持つインターフェース**を**利用者側で定義できる**ため、
SOLID の**インターフェース分離の原則**に則った、実装側と利用側の結合度が低い（**疎結合**な）関係性にできる。

:::message

### インターフェースの命名 -er

`Xxx`メソッドを 1 つだけ持つインターフェースの場合、そのインターフェースは（`er`をつけて）`Xxxer`という命名にする。
例えば`Do`メソッドだけを持つインターフェースは、（そんな単語は無いけども）`Doer`とする。
:::

## ライブラリとしてインターフェースを返す

パッケージをライブラリとして他のパッケージに提供する場合、実装の詳細を隠蔽するために、インターフェースを返す関数を作成する場合がある。

その場合、契約による設計を意識して、コードコメントで事前条件・事後条件・不変条件などを記載すること。

## インターフェースを作りすぎない

構造体から、どれがどのインターフェースを満たしているのかを確認できないため、**不要な抽象化を行うインターフェースの定義は可読性を低下させる**。

インターフェースは以下の場合に定義するようにする。

1. 統一的に扱わなければいけない、2 つ以上の具象型が存在する場合。
2. インターフェースは単一の具象型で満足しているが、依存性のためにその具象型が別々のパッケージにそれぞれ存在しないといけない場合。
