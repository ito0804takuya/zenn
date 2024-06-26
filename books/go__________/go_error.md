---
title: "Go_エラー"
---

# エラー処理

- Go の**エラーはただの値** である。
  他の言語のような例外機構とは異なり、Go のエラーは return を使って戻り値として返す。

- エラーが予想されていなくても、エラーがないかどうかを常に確認して、不要な情報がエンドユーザーに公開されないようにすること。（本当に例外的な場合のみ panic する。）

## エラーの作成

- エラーを作成する方法は 2 つ。

  1. `errors.New()`
  2. `fmt.Errorf()` : ヴァーブ（Verb, %s や%d のこと）を使って動的な情報を埋め込むときはこっちを使う。
     また、`fmt.Errorf()`はエラーを埋め込むヴァーブ`%w`を使うことで、他のエラーをラップすることもできる。

- エラーメッセージ（文字列）は他のコンテキストで使われるケースがあるため、頭文字を大文字にしたり、句読点で終わってはいけない。（他のメッセージと組み合わせないことが確かな場合は除く。）
  つまり、`log.Print("Reading %s: %v", filename, err)` が途中で大文字を出力することがないように `fmt.Errorf("Something bad")` ではなく、 `fmt.Errorf("something bad")` を使う。

- エラーメッセージにプレフィックスを含めて、エラーの発生元がわかるようにすること。（例: パッケージや関数の名前を含める。）

## エラーの判定

エラーの値を判定するには`errors.Is()`を使う。エラーがラップされていても、引数同士が同一か判定できる。
`errors.As()`を使うと、（ラップされているエラーに対しても）特定の型に変換して取得・判定できる。

## 独自エラーの宣言

### 独自エラー変数

エラー変数を作成して、特定のエラー状態が発生したことを表す。
（例: `var ErrNotFound = errors.New("Employee not found")`）

### 独自のエラー構造体

独自エラー変数で識別するだけでなく、エラーに詳細（独自の code 値など）を格納したり、独自めそっどを用意したい場合、独自のエラー構造体を定義する。

## エラーに文脈をもたせる

`fmt.Errorf`の`%w`を使うことで、既にあるエラーにラップした（1 枚上乗せした）エラーを作ることができる。

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

## panic 関数

プログラムを強制的にパニックにする。スタックトレースを出力してクラッシュする。

`error`は呼び出し元の関数などに順々に伝わるのに対して、**`panic`は大域脱出**する。

:::message
**想定される異常状態は`error`を使って表現**すべきで、
**プログラムが対処できないような問題やロジック上のバグは`panic`を使って対応**する。

（例えば、ユーザ入力で指定されたファイル名のファイルが無い という場合は`error`。呼び出している CSS ファイルが存在しない 場合は`panic`。）

:::

## recover 関数

`panic()`後に、制御できる。
`defer()`の中で実行できる。
`panic()`と違い、それ単体では自動でスタックトレースを出力しない。

```go
func main() {
	// panicが起きたときに、catchすることを予約しておく
	defer func() {
		if r := recover(); r != nil {
			fmt.Println("panicしたら実行", r)
		}
	}()

	g(0)
	fmt.Println("finish!!") // panicするので、この行は絶対に実行されない
}

func g(i int) {
	if i > 3 {
		panic("パニック") // panicを起こす
	}

	defer fmt.Println("defer:", i) // recoverより先に実行される

	fmt.Println("print:", i)
	g(i + 1) // 無限再帰
}
```

:::message alert
**`recover`では、異なるゴルーチンで発生した`panic`を捉えることができない**。
新たなゴルーチンで起動する場合は、その処理の中で panic が発生しないか確認しておく必要がある。
:::

# 参考文献

http://golang.org/doc/effective_go.html#errors

## まだ読んでないやつ

https://zenn.dev/spiegel/books/error-handling-in-golang

https://qiita.com/Maki-Daisuke/items/80cbc26ca43cca3de4e4
