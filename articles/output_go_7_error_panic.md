---
title: "Go エラー処理、パニック"
emoji: "👏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Go"]
published: true
---

# エラー処理

```go
data, err := getData()
if err != nil {
  // エラー時の処理
}
```

上の書き方よりもerr変数のスコープが小さくするには、下のように代入付きif文でerr変数を作成する。
```go:代入付きif文でerr変数を作成
if err := f(); err != nil {
  // エラー時の処理
}
```

## エラー処理で大事なこと
- 必要十分な正しい情報を伝えること。
- 相手によって、伝えるべき情報を考えて返すこと。

## エラー処理に関する推奨事項

Go でエラーを処理する場合は、次のような推奨事項を考慮すること。

1. エラーが予想されていなくても、**エラーがないかどうかを常に確認**します。
   そのうえで、**不要な情報がエンドユーザーに公開されないように**します。
   (エラー処理の最も簡単な方法は、if 条件を使用すること)
2. エラーメッセージに**プレフィックスを含めて、エラーの発生元がわかるように**します。
   たとえば、パッケージや関数の名前を含めることができます。
3. 可能な限り、**再利用可能なエラー変数**を作成します。
4. エラーを返すことと、パニックの違いを理解します。
   **他に対処方法がない場合は、パニック**が発生します。
   たとえば、依存関係の準備ができていない場合、プログラムは動作しません (既定の動作を実行する場合を除きます)。
5. 可能な限り多くの詳細情報でエラーをログに記録し、エンドユーザーが理解可能なエラーを出力します。

```go:
type Employee struct {
  ID        int
  FirstName string
  LastName  string
  Address   string
}

func main() {
  employee, err := getInformation(1001)
  if err != nil {
    // なにかする
  } else {
    fmt.Println(employee)
  }
}

func getInformation(id int) (*Employee, error) {
  employee, err := apiCallEmployee(1000)

  // 修正前
  // return employee, err

  // 修正後
  // 1. エラーが予想されていなくても、エラーがないかどうかを常に確認します。
  //   そのうえで、不要な情報がエンドユーザーに公開されないようにします。
  if err != nil {
    return nil, err // apiCallEmployee()からエラーが返って来ている場合は、errのみ返す
  }
  return employee, nil
}

// 3. 可能な限り、再利用可能なエラー変数を作成します。
var ErrNotFound = errors.New("Employee not found")

func apiCallEmployee(id int) (*Employee, error) {
  if id != 1001 {
    return nil, ErrNotFound
  }

  employee := Employee{LastName: "hoge", FirstName: "john"}
  return &employee, nil
}
```

## エラーに文脈をもたせる
`fmt.Errorf`の`%w`を使うことで、既にあるエラーにラップした（1枚上乗せした）エラーを作ることができる。

また、そうやって上乗せされたエラーの根本（根本原因）を取得するには、`errors.Unwrap()`を使う。

```go
err := fmt.Errorf("bar: %w", errors.New("foo"))
fmt.Println(err) // bar: foo
fmt.Println(errors.Unwrap(err)) // foo
```

## エラーの値を判定する
`errors.Is`で、第一引数のエラーが第二引数の値かどうかを判定できる。
```go
if errors.Is(err, os.ErrExist) {
  // os.ErrExistだった場合のエラー処理
}
```

# 例外処理

panic と recover の組み合わせは、Go での特徴的な例外処理方法。
（他のプログラミング言語では、try/catch。）

:::message alert
## エラーとパニック（例外）の使い分け
**panicは回避不可能な場合のみ**使う。

想定内のエラーはerror型で処理する。
:::

## panic 関数

プログラムを強制的にパニックにする。ログメッセージを出力してクラッシュする。

```go
import "fmt"

func main() {
  g(0) // 関数g()を実行
  fmt.Println("finish") // panicにより、この行は実行されない
}

func g(i int) {
  if i > 3 {
    fmt.Println("パニック前") // 2. panic前の処理は、ふつうに2番目に実行される
    panic("パニック") // 4. deferの後に実行される
  }

  defer fmt.Println("defer:", i) // 3. panic("パニック")より先に実行される

  fmt.Println("print:", i) // 1. まずはこの行が実行される
  g(i + 1) // 再帰(panicが無ければ、無限に繰り返す)
}

/* 出力
  print: 0
  print: 1
  print: 2
  print: 3
  パニック前
  defer: 3
  defer: 2
  defer: 1
  defer: 0
  panic: パニック

  goroutine 1 [running]:
  main.g(0x4)
          /Users/itotakuya/projects/go/src/helloworld/main.go:329 +0x22e
  main.g(0x3)
          /Users/itotakuya/projects/go/src/helloworld/main.go:335 +0x17a
  main.g(0x2)
          /Users/itotakuya/projects/go/src/helloworld/main.go:335 +0x17a
  main.g(0x1)
          /Users/itotakuya/projects/go/src/helloworld/main.go:335 +0x17a
  main.g(0x0)
          /Users/itotakuya/projects/go/src/helloworld/main.go:335 +0x17a
  main.main()
          /Users/itotakuya/projects/go/src/helloworld/main.go:322 +0x2e
  exit status 2
*/
```

## recover 関数

`panic()`後に、制御できる。
`defer()`の中で実行できる。
`panic()`と違い、ログメッセージを出力しない。

```go
import "fmt"

func main() {
  defer func() { // 5. panicが発生したので、deferの後で実行される
    if r := recover(); r != nil { // panicが起きたときに、catchするよう予約
      fmt.Println("リカバーした", r) // r = "パニック"
    }
  }()

  g(0)
  fmt.Println("finish!!") // panicにより、この行は実行されない
}

func g(i int) {
  if i > 3 {
    fmt.Println("パニック前") // 2. panic前の処理は、ふつうに2番目に実行される
    panic("パニック") // 4. panic発生
  }

  defer fmt.Println("defer:", i) // 3. recover() より先に実行される

  fmt.Println("print:", i) // 1. まずはこの行が実行される
  g(i + 1) // 再帰(panicが無ければ、無限に繰り返す)
}

/* 出力
  print: 0
  print: 1
  print: 2
  print: 3
  パニック前
  defer: 3
  defer: 2
  defer: 1
  defer: 0
  リカバーした パニック
/*
```

https://qiita.com/Maki-Daisuke/items/80cbc26ca43cca3de4e4

