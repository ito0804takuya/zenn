---
title: "「Goならわかるシステムプログラミング」要点"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Go"]
published: false
---

# 書籍
https://www.lambdanote.com/collections/go-2/products/go-2

# 参考
以前Goを学習した際の記事も参照。
https://zenn.dev/itoo/articles/learn_golang

# 第1章 Go言語で覗くシステムプログラミングの世界
## 1.1 システムプログラミングとは
システムプログラミングとは、OSの提供する機能を使ったプログラミング。
（Web関係のプログラミングとは対照的な場面で使われる言葉。）

### OSの機能
ボトムアップではレベルアップにつながる実感が湧きづらいOSの機能（下記）を、
プログラマー目線で、普段の開発にもフィードバックできるように見ていく。
- メモリの管理
- プロセスの管理
- プロセス間通信
- ファイルシステム
- ネットワーク
- ユーザー管理（権限など）
- タイマー

## 1.2 Go言語
多くのOSの機能を直接扱える。
少ない行数で動くアプリケーションが作れる。

現在のシステムプログラミングでは主にC, C++が使われている。
- C, C++と比較したメリット
  - ライブラリの収集が簡単。
  - コンパイルで多くのエラーが見つかる。
  - 実行時のエラーがわかりやすい。
  - メモリ管理を注意深く設計しなくて済む。（ガベージコレクション）
  - コンパイルが速い。
- デメリット
  - （スクリプト言語よりは速いが）、C, C++と比較すると遅い。
  - バイナルサイズがかなり大きくなる。

### goコマンド
#### プロジェクト作成後の初期化
`mod init`を実行すると、go.modファイルが生成され、Goの処理系から、このファイルがある場所がこのプロジェクトのルートと認識される。
```bash
go mod init <プロジェクト名>
```

---

## 1.4 デバッガーを使って "Hello World!"の裏側を覗く
```go:main.go
package main 

import "fmt"

func main() {
	fmt.Println("Hello", "World!")
}
```
:::message
#### パッケージ package
Go言語は各ディレクトリごとに`パッケージ`という単位で分割される。
このパッケージ内部ではローカル変数を除けばどの変数へもアクセスできる。
（= 同一ディレクトリ上で定義されたオブジェクトにアクセスすることができる。）

※ サブディレクトリはまったく関係のない別モジュールとして扱われる。

- 関連記事 : https://zenn.dev/itoo/articles/learn_golang#%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8
:::

このHello Worldの裏側でどういう処理が行われているか、関数の定義をたどっていくことで確認していく。
↓

```go:print.go
func Println(a ...interface{}) (n int, err error) {
	return Fprintln(os.Stdout, a...)
}
```
↓
```go:print.go
// func 関数名(引数-1 型-1, 引数-2 型-2) (返り値-1 型-1, 返り値-2 型-2) { .... }
func Fprintln(w io.Writer, a ...interface{}) (n int, err error) {
	p := newPrinter()
	p.doPrintln(a)
	n, err = w.Write(p.buf)
	p.free()
	return
}
```
:::message
#### 可変長引数関数(Variadic function) `...(ドット3つ)`
メソッドの引数が、同じ型の複数個の場合、`...`で示す。
```go
func method(args ...Type)
```
- 参考記事 : https://zenn.dev/mikankitten/articles/cfa2ef834e338e
:::
:::message
#### interface{}型
interface{}型はint, string, boolなどと同じ、golangの型名。{}の部分まで含めて型名。
どんな型も格納できる特殊な型。

- 参考記事 : https://qiita.com/sh-tatsuno/items/0c32c01eaeaf2d726fdf
:::
:::message
そのため、上の`Fprintln`関数の第2引数`a ...interface{}`は、「何が渡されるかわからない複数の引数を受け取る」という意味。（今回は、"Hello"と"World!"）
```go
fmt.Println("Hello", "World!")
```
:::
↓
```go:file.go
// 構造体Fileの公開メソッド（メソッド名の最初が大文字）
// https://zenn.dev/itoo/articles/learn_golang#スコープ

// func (変数 構造体) メソッド名(引数 型) (返り値1 型1, 返り値2 型2) { .... }
func (f *File) Write(b []byte) (n int, err error) {
	if err := f.checkValid("write"); err != nil {
		return 0, err
	}
	n, e := f.write(b)
  // 以降は省略
}
```
↓
```go:file_posix.go
func (f *File) write(b []byte) (n int, err error) {
	n, err = f.pfd.Write(b) // 構造体Fileのプライベートフィールドpfd の公開メソッド
	runtime.KeepAlive(f)
	return n, err
}
```
↓
```go:fd_unix.go
func (fd *FD) Write(p []byte) (int, error) {
	if err := fd.writeLock(); err != nil {
		return 0, err
	}
	defer fd.writeUnlock()
	if err := fd.pd.prepareWrite(fd.isFile); err != nil {
		return 0, err
	}
	var nn int
	for {
		max := len(p)
		if fd.IsStream && max-nn > maxRW {
			max = nn + maxRW
		}
		n, err := ignoringEINTRIO(syscall.Write, fd.Sysfd, p[nn:max]) // syscall.Write()はシステムコール
		if n > 0 {
			nn += n
		}
		if nn == len(p) {
			return nn, err
		}
		if err == syscall.EAGAIN && fd.pd.pollable() {
			if err = fd.pd.waitWrite(fd.isFile); err == nil {
				continue
			}
		}
  // 以降は省略
```
:::message
#### システムコールとは（ざっくり）
- アプリケーションのプログラム単体では達成できない仕事を、OSのカーネルに依頼するために使う。
  （上の`syscall.Write()`では、（プログラムの外の世界である）ターミナルに対して文字列を出力するという仕事 を依頼している。）
- いくつも種類がある。
:::

---

# 第2章 低レベルアクセスへの入口1：io.Writer
## 2.1 io.Writerは OSが持つファイルのシステムコールの相似形
OSは、システムコール（例 : 1章の`syscall.Write()`）を呼び出すとき、ファイルディスクリプタ（= 識別子（なので数値））を指定する。
そうすることで、ファイルディスクリプタで指定したモノにアクセスできる。

ファイルディスクリプタに対応するたモノには、ファイルのみならず、標準入出力・ソケットなどのファイルでないものも含まれ、ファイルと同じようにアクセスできる。

*ここにあとで追記する*

## 2.2 Go言語のインタフェース

　2.3 io.Writerは「インタフェース」
　2.4 io.Writerを使う構造体の例
　2.5 インタフェースの実装状況・利用状況を調べる
　2.6 低レベルの機能を組み合わせて入出力APIを作る
　2.7 柔軟性が高く、パフォーマンスのよい設計のための Tips.
　2.8 本章のまとめと次章予告
　2.9 問題



---

第3章 低レベルアクセスへの入口2：io.Reader
　3.1 io.Reader
　3.2 io.Readerの補助関数
　3.3 入出力に関するio.Writerとio.Reader以外のインタフェース
　3.4 io.Readerを満たす構造体で、よく使うもの
　3.5 バイナリ解析用のio.Reader関連機能
　3.6 テキスト解析用のio.Reader関連機能
　3.7 io.Reader／io.Writerでストリームを自由に操る
　3.8 本章のまとめと次章予告
　3.9 問題

第4章 低レベルアクセスへの入口3：チャネル
　4.1 goroutine.
　4.2 チャネル
　4.3 システムからの通知
　4.4 本章のまとめと次章予告
　4.5 問題

第5章 システムコール
　5.1 システムコールとは何か？
　5.2 Go言語におけるシステムコールの実装
　5.3 POSIXとC言語の標準規格
　5.4 システムコールより内側の世界
　5.5 Go言語のシステムコールとPOSIX
　5.6 システムコールのモニタリング
　5.7 エラー処理
　5.8 通常のシステムコール以外の特殊なシステム呼び出し
　5.9 本章のまとめと次章予告
　5.10 問題

第6章 TCPソケットとHTTPの実装
　6.1 プロトコルとレイヤー
　6.2 HTTPとその上のプロトコルたち
　6.3 ソケットとは
　6.4 ソケット通信の基本構造
　6.5 Go言語でHTTPサーバーを実装する
　6.6 速度改善（1）: HTTP/1.1のKeep-Aliveに対応させる
　6.7 速度改善（2）:圧縮
　6.8 速度改善（3）:チャンク形式のボディー送信
　6.9 速度改善（4）:パイプライニング
　6.10 本章のまとめと次章予告

第7章 UDPソケットを使ったマルチキャスト通信
　7.1 UDPとTCPの用途の違い
　7.2 UDPと TCPの処理の流れの違い
　7.3 UDPのマルチキャストの実装例
　7.4 UDPを使った実世界のサンプル
　7.5 UDPと TCPの機能面の違い
　7.6 本章のまとめと次章予告

第8章 高速なUnixドメインソケット
　8.1 Unixドメインソケットの基本
　8.2 Unixドメインソケットの使い方
　8.3 Windowsの名前付きパイプ
　8.4 UnixドメインソケットとTCPのベンチマーク
　8.5 ソケットのシステムコール小話
　8.6 本章のまとめと次章予告

第9章 ファイルシステムの基礎とGo言語の標準パッケージ
　9.1 ファイルシステムの基礎
　9.2 ファイル／ディレクトリを扱うGo言語の関数たち
　9.3 OS内部におけるファイル操作の高速化
　9.4 ファイルパスとマルチプラットフォーム
　9.5 path/filepathパッケージの関数たち
　9.6 本章のまとめと次章予告

第10章 ファイルシステムの最深部を扱うGo言語の関数
　10.1 ファイルの変更監視（syscall.Inotify*）
　10.2 ファイルのロック（syscall.Flock()）
　10.3 ファイルのメモリへのマッピング（syscall.Mmap()）
　10.4 同期・非同期／ブロッキング・ノンブロッキング
　10.5 select属のシステムコールによるI/O多重化
　10.6 FUSEを使った自作のファイルシステムの作成
　10.7 本章のまとめと次章予告

第11章 コマンドシェル
　11.1 シェルとは何か
　11.2 シェルの利用形態
　11.3 POSIX、SUS、LSB、BusyBox
　11.4 環境変数
　11.5 シェルがコマンドを起動するまで
　11.6 Unix哲学とシェル
　11.7 まとめ

第12章 プロセスの役割とGo言語による操作
　12.1 プロセスに含まれるもの（Go言語視点）
　12.2 プロセスの入出力
　12.3 自分以外のプロセスの名前や資源情報の取得
　12.4 OSから見たプロセス
　12.5 Goプログラムからのプロセスの起動
　12.6 プロセスに関する便利なGo言語のライブラリ
　12.7 Go言語では触れることのない世界
　12.8 子プロセスの内部実装
　12.9 本章のまとめと次章予告

第13章 シグナルによるプロセス間の通信
　13.1 シグナルのライフサイクル
　13.2 シグナルの種類
　13.3 Go言語におけるシグナルの種類
　13.4 シグナルのハンドラを書く
　13.5 シグナルの応用例（Server::Starter）
　13.6 Go言語ランタイムにおけるシグナルの内部実装
　13.7 Windowsとシグナル
　13.8 本章のまとめと次章予告

第14章 Go言語と並列処理
　14.1 複数の仕事を同時に行うとは？
　14.2 Go言語の並列処理のための道具
　14.3 スレッドとgoroutineの違い
　14.4 GoのランタイムはミニOS
　14.5 runtimeパッケージのgoroutine関連の機能
　14.6 RaceDetector
　14.7 syncパッケージ
　14.8 sync/atomicパッケージ
　14.9 本章のまとめと次章予告

第15章 並行・並列処理の手法と設計のパターン
　15.1 並行・並列処理の手法のパターン
　15.2 Goにおける並行・並列処理のパターン集
　15.3 本章のまとめと次章予告

第16章 Go言語のメモリ管理
　16.1 メモリ確保の旅
　16.2 Go言語の配列
　16.3 スライスなど
　16.4 ガベージコレクタ
　16.5 本章のまとめと次章予告

第17章 実行ファイルが起動するまで
　17.1 実行ファイルが起動するまで
　17.2 実行ファイルを支える仕組み
　17.3 実行ファイルのメモリ配置
　17.4 Goのプログラムの起動
　17.5 インタプリタでのコードの起動
　17.6 まとめ

第18章 時間と時刻
　18.1 OSのタイマー／カウンターの仕組み
　18.2 さまざまな時間
　18.3 時間に関するシステムコール
　18.4 Go言語で時間を扱う
　18.5 時刻のフォーマット
　18.6 本章のまとめと次章予告

第19章 Go言語とコンテナ
　19.1 仮想化
　19.2 コンテナ
　19.3 Windows Subsystem for Linux 2（WSL2）
　19.4 libcontainerでコンテナを自作する
　19.5 本章のまとめ