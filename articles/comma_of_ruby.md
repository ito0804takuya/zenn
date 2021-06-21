---
title: "Ruby, Railsの「:」について"
emoji: "😎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Ruby", "Rails"]
published: true
---

# 結論
1. 前コロン は「シンボル」
2. hash{} の内側の後ろコロン は「シンボルをキーにしたhash」
3. メソッドの引数の後ろコロン は「メソッドのキーワード引数」

# 説明

## 1. 前コロン は「シンボル」
- シンボルとは、ざっくりいうと、文字列の上位互換。
  - 別の変数に、同じシンボルを定義すると、それらはPC上は同じ領域に保存される。→ 重複しない。

```ruby
mojirestu = "abc" # 文字列
shinboru = :abc   # シンボル
```

## 2. hash{} の内側の後ろコロン は「シンボルをキーにしたhash」
```ruby
hash1 = { "id" => 1, "key" => "value" } # 文字列をキーにしたhash
hash2 = { :id => 1, :key => "value" }   # シンボルをキーにしたhash
hash3 = { id: 1, key: "value" }         # シンボルをキーにしたhash（簡略ver.）
```

## 3. メソッドの引数の後ろコロン は「メソッドのキーワード引数」
- メソッドのキーワード引数 = 見出しをつけた引数
- キーワード引数は、順番は関係ない。
- キーワード引数は、通常の引数と併用できる。
- メソッドの呼び出しは、カッコを省略できる。

```ruby
# 通常の引数
def add(num1, num2)
  result = num1 + num2
end
p add(2, 3)
```

```ruby
# キーワード引数
# 「メソッドのキーワード引数 = 見出しをつけた引数」
def add(num1: , num2:)
  result = num1 + num2
end
p add(num1: 2, num2: 3)

# 「キーワード引数は、順番は関係ない。」
p add(num2: 3, num1: 2)  # OK

# 「メソッドの呼び出しは、カッコを省略できる。」
p add num1: 2, num2: 3   # OK

# 「キーワード引数は、通常の引数と併用できる。」
render "users/signin", user: @user
# ↓
render("users/signin", user: @user) # 第1引数は通常の引数、第2引数はキーワード引数
```

## 具体例
```ruby
validates :content, presence: true, length: {maximum: 50}
# validates(:content, presence: true, length: {maximum: 50})
```
- validates
メソッド
- :content
validatesメソッドの第1引数。シンボル。
- presence: true
validatesメソッドの第2引数。presenceというキーワード引数に、trueを指定している。
- length: {maximum: 50}
validatesメソッドの第3引数。lengthというキーワード引数に、hashを渡している。


# 参考文献
https://www.udemy.com/course/ruby-on-rails-c/
