---
title: "競技プログラミングでよく使うGoの構文メモ"
emoji: "🙆"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Go"]
published: false
---

# 命名規則

- 変数 : 1 語 かつ 超短い言葉で。`i, s, cnf`
- 関数、type、構造体 : キャメルケース。`MailLog` エクスポートされる場合、頭文字は大文字。

https://zenn.dev/kenghaya/articles/1b88417b1fa44d

# 標準入力からの受け取り

```go:個数が固定で決まっている場合
var a, b int
var s string

fmt.Scan(&a, &b, &s)
```

```go:n個の入力を受け取る場合
nums := make([]int, n)
for _, num := range nums {
  fmt.Scan(&num)

  // numを使った処理
}
```

# 空のスライスを作る

```go
p := []byte{}
```

https://qiita.com/ktnyt/items/535436989220a21b75e6
