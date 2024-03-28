---
title: "Go コマンドラインツール"
emoji: "🌊"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Go"]
published: true
---

# コマンドライン引数

コマンドライン引数は、`os.Args`で受け取る。

`os.Args`は string 型のスライスで、

- 0 番目の要素には、実行したコマンド名が格納されている。
- 1 番目以降の要素に、コマンドに渡された引数が格納されている。

```go:[ go run test.go 123 456 ]と実行した
func main() {
  fmt.Printf("args count: %d\n", len(os.Args))
  // → 3
  fmt.Printf("args : %#v\n", os.Args)
  // → []string{"/var/folders/rj/l6kf96gj15331qbmbr7lggp40000gn/T/go-build4014886209/b001/exe/test", "123", "456"}
  fmt.Printf("実際に欲しい引数 : %#v\n", os.Args[1:])
  // → []string{"123", "456"}
}
```

:::message

### 標準入力からの受け取り

標準入力は`fmt.Scan()`で受け取る。

```go:個数が固定で決まっている場合
var a, b int
var s string
fmt.Scan(&a, &b, &s)
```

```go:n個の入力を受け取る場合
nums := make([]int, n)
for _, num := range nums {
  fmt.Scan(&num)
}
```

https://zenn.dev/pikarich/articles/8f1c7fe4d04e93
:::

# flag パッケージ

コマンドライン引数の**オプション**（フラグ）を便利に扱うパッケージ。

### 定義

オプションを定義するには、下記 2 つの方法がある。

- `var 変数 = flag.型名("フラグ名", デフォルト値, "説明文")` : `型名()`ではポインタが返される。
- `flag.型名Var(&変数, "フラグ名", デフォルト値, "説明文")` : 変数が既に宣言されている場合。`型名Var()`では変数にバインド（=実体を更新）する。

### パース（解析）

こうして**オプションを定義した後**、実際にそれにアクセスする前に`flag.Parse()`で、コマンドをパースしないといけない。

### 例

`go run test.go -msg=こんにちは -n=2`というように実行するコマンドの msg オプション / n オプションを実装するには、下記のように書く。

```go
var msg = flag.String("msg", "デフォルト値です", "メッセージ文")
var n int

func init() {
  flag.IntVar(&n, "n", 1, "メッセージを繰り返す回数")
}
func main() {
  // オプションをパース → 実際にフラグ変数を使えるようになる
  flag.Parse()
  fmt.Println(strings.Repeat(*msg, n)) // こんにちはこんにちは
}
```

:::message

### init()

init()は main()より先に実行される特殊な関数。
:::

# 入出力

| 種類           | 変数      | 説明                                                                                     |
| -------------- | --------- | ---------------------------------------------------------------------------------------- |
| 標準入力       | os.Stdin  | 入力（キーボードとかから待ち受けるやつ）                                                 |
| 標準出力       | os.Stdout | 出力。次のコマンドに（パイプとかで）渡すような値。なのでエラーは標準出力に出さないこと。 |
| 標準エラー出力 | os.Stderr | エラーを出力。                                                                           |

os.Stdin らは、**io.Reader**や**io.Writer**インターフェースを実装している。
なので、様々な関数やメソッドの引数として渡せる。

:::message

### io.Reader、io.Writer

それぞれ 1 つのメソッド（Read、Write）しか持たない、インターフェース。
ファイルへの書き込み、メモリへの書き込み...など、色々なものへの読み書きを抽象化している。
:::

# プログラムの終了

`os.Exit(0)`で、プログラムを終了する。_呼び出し元にも終了状態が伝えられる_。

- 0 : 正常
- 0 以外 : 異常

# ファイル

- ファイルを開く : `f, err := os.Open("ファイル名")`
- ファイルを作成 : `f, err := os.Create("ファイル名")`
- 詳細なオプションを指定して、ファイルを開く :
  `f, err := os.OpenFile(ファイル名, フラグ(ファイルが無ければ作成する等), パーミッション)`

- ファイルを読み込む : `[*os.File型].Read(読み込んだデータを格納するスライス)`
- ファイルを読み込む : `[*os.File型].Write(書き込む文字列を格納したスライス)`

```go:読み込み
func main() {
  // ファイルを開く
  f, err := os.Open("./ingo.log")

  // 関数の実行終了時にファイルを閉じるのを予約しておく
  defer f.Close()

  // ファイルの中身が格納される、空のスライスを作成
  data := make([]byte, 1024) // 最大1024byte

  // （空のスライスに）ファイルfの中身を格納
  count, err := f.Read(data)
  fmt.Println(count) // 何byte読み込んだか

  // 読み込みのエラーハンドリング
  if err != nil {
    fmt.Println("読み込みエラー", err)
  }

  // ファイルの中身を出力 (stringでキャストしないと、文字列にエンコードする前のバイト列が出力されてしまう)
  fmt.Println(string(data))
}
```

```go:作成+書き込み
func main() {
  text := "書き込みたいテキスト"
  data := []byte(text)

  // ファイル作成
  f, err := os.Create("create_sample.txt")
  if err != nil {
    fmt.Println("ファイル作成時エラー")
  }
  defer f.Close()

  // ファイルに書き込み
  count, err := f.Write(data)
  if err != nil {
    fmt.Println("ファイル書き込み時エラー")
  }
  fmt.Println(count) // // 何byte読み込んだか
}
```

```txt:create_sample.txt
書き込みたいテキスト
```

# ファイルパス

`path/filepath`パッケージを使う。

```go
path := filepath.Join("dir", "main.go")
fmt.Println(path) // dir/main.go
// 拡張子 のみ
fmt.Println(filepath.Ext(path)) // .go
// ファイル名 のみ
fmt.Println(filepath.Base(path)) // main.go
// ディレクトリ のみ
fmt.Println(filepath.Dir(path)) // dir
```

# ログ

```go:log.Fatal, log.SetPrefix
func main() {
	log.Print("Printログ1")

	log.SetPrefix("main(): ")
	log.Print("Printログ2")

	log.Fatal("最終ログ")
	log.Print("Printログ3 log.Fatalで終了させるためこれは実行されない")
	/*
		2023/08/24 06:48:09 Printログ1
		main(): 2023/08/24 06:48:09 Printログ2
		main(): 2023/08/24 06:48:09 最終ログ
		exit status 1
	*/
}
```

```go:log.Panic
func main() {
  log.Print("printろぐ")
  // → 2021/08/09 07:23:53 printろぐ

  log.Panic("panicろぐ")
  fmt.Print("これは見えない")
  // → panic: panicろぐ
  //   goroutine 1 [running]:
  //   log.Panic(0xc0000c3f58, 0x1, 0x1)
  //       /usr/local/go/src/log/log.go:354 +0xae
  //   main.main()
  //       /Users/itotakuya/projects/go/src/helloworld/main.go:814 +0xa5
  // Panic()以降は実行されない
}
```

### ファイルにログを記録する

```go:main.go
func main() {
  // ファイル作成
  file, err := os.OpenFile("ingo.log", os.O_CREATE|os.O_APPEND|os.O_WRONLY, 0644)

  if err != nil {
  log.Fatal("エラー発生")
  }

  // 後でファイルにログを記録するが、file.Close()を忘れないようにdefer予約
  defer file.Close()

  // 通常通り、標準出力へ
  log.Print("ログ1")

  // fileにログを記録することを宣言
  // これ以降、log出力すると、ファイルに記録される
  log.SetOutput(file)

  // ファイルに書き込まれる
  log.Print("ログ2")
}
```

# Print 関数

```go
func main() {
  fmt.Print("Hello", "world!")
  fmt.Print("Hello", "world!")
  // -> Helloworld!Helloworld!

  // Printlnの場合は、スペースと改行が挿入される
  fmt.Println("Hello", "world!")
  fmt.Println("Hello", "world!")
  // -> Hello world!
  //    Hello world!

  hello := "Hello world!"
  // Printf : フォーマットを指定して出力する
  fmt.Printf("%s\n", hello) // %s : 文字のみを出力 (""なし)
  fmt.Printf("%#v\n", hello) // %#v : Goの文法での表現を出力
  // -> Hello world!
  //    "Hello world!"
}
```

## フォーマット指定子

`%v`には何でも渡してok。

構造体の出力には、フォーマット指定子は`%+v`か`%#v`がいい。
```go
t := &T{ 7, -2.35, "abc\tdef" }

fmt.Printf("%+v\n", t)
// &{a:7 b:-2.35 c:abc     def}

fmt.Printf("%#v\n", t)
&main.T{a:7, b:-2.35, c:"abc\tdef"}
```

:::message
Printf のフォーマット指定方法については、下の記事が詳細に書いてある。
:::
https://qiita.com/rock619/items/14eb2b32f189514b5c3c
