---
title: "Go å­¦ç¿ã¡ã¢"
emoji: "ð"
type: "tech" # tech: æè¡è¨äº / idea: ã¢ã¤ãã¢
topics: ["Go"]
published: true
---

# å­¦ç¿åãµã¤ã
https://docs.microsoft.com/ja-jp/learn/paths/go-first-steps/

## ãã®ä»ã®ãµã¤ã
#### ç¶²ç¾çã«èª¬æãã¦ããã¦ããã¹ã©ã¤ã
https://docs.google.com/presentation/d/1RVx8oeIMAWxbB7ZP2IcgZXnbZokjCmTUca-AbIpORGk/edit#slide=id.g4f417182ce_0_80

#### grpcãã­ãã³ã«ã®ãã¥ã¼ããªã¢ã«
https://github.com/ymmt2005/grpc-tutorial

#### Goã®ä½ã¬ã¤ã¤ã«ã¤ãã¦
https://ascii.jp/serialarticles/1235262/

# ãã­ã°ã©ã å®è¡
Goã¯ãmainããã±ã¼ã¸åã®mainé¢æ°ãããã­ã°ã©ã ãã¹ã¿ã¼ãããã
ï¼ãã­ã°ã©ã ãã¹ã¿ã¼ãããå°ç¹ã®ãã¨ããã¨ã³ããªã¼ãã¤ã³ãã¨ãããï¼

## å®è¡
Goãã¡ã¤ã«ããããã©ã«ãã¼ã«ç§»åã
```bash
go run main.go

# ç§»åããªãå ´å
go run ./src/helloworld/main.go
```

runã³ãã³ãã¯ããã«ã(ä¸è¨buildã³ãã³ã) + å®è¡ ãè¡ã£ã¦ããã

## ãã«ã
```bash
go build main.go
```
ãã«ãããã¨ãå®è¡ãããã¡ã¤ã«ã®æ¡å¼µå­ããªãver.ã®ãã¡ã¤ã«ãçæãããã
(main.goãâãmain ãçæããã)

# ã¹ãã¼ãã¡ã³ã
```go:main.go
// main: ãã¹ã¦ã® å®è¡å¯è½ãã­ã°ã©ã ã¯ main ããã±ã¼ã¸ã«å«ã¾ããå¿è¦ãããã
package main

// import: å¥ã®ããã±ã¼ã¸åã®ã³ã¼ããããã­ã°ã©ã ã«ã¢ã¯ã»ã¹ã
// fmt: æ¨æºã©ã¤ãã©ãªããã±ã¼ã¸
import "fmt"

// main(): package main å¨ä½ã§ main()é¢æ°ã¯1ã¤ã ãã
func main() {
    fmt.Println("helo")
}
```

# å¤æ°
```go
// 1
var firstName, lastName string
var age int

// 2
var (
  firstName, lastName string
  age int
)

// 3
var (
  firstName = "ichiro" // åãæ¨å®ããã
  lastName = "tanaka"
  age = 20
)

// 4
firstName, lastName := "ichiro", "tanaka" // å®ç¾©+ä»£å¥ãåæã«
age := 20 // int
age = age /3 // = 6 ã«ãªãï¼int = æ´æ° ã®ããå°æ°ãåãæ¨ã¦ãããï¼
```

# å®æ°
```go
const (
  StatusOk = 0 // å¤æ°ã®ããã« := ã¯ä½¿ããªã
  StatusError = 1
)
```

# å
## æµ®åå°æ°ç¹æ°(float32, float64)
```go
var shousuu1 float32 = 2.11
shousuu2 := 2.11
```
## çå½å¤(bool)
:::message alert
ä»ã®è¨èªã®ããã«**æé»çã«0, 1ã«å¤æããªã**ã**æç¤ºçã«è¡ãå¿è¦ãã**ã
:::

## æå­å(string)
ããã«ã¯ã©ã¼ãã¼ã·ã§ã³ (") ã§å²ãã
ã·ã³ã°ã«ã¯ã©ã¼ãã¼ã·ã§ã³ (')ã§å²ãã®ã¯ãruneã

## åå¤æã»ã­ã£ã¹ã
```go:main.go
import "strconv"

func main() {
    i, _ := strconv.Atoi("-42") // Atoi: æå­å â æ°å¤ã¸ãã¼ã¹ãAtoi ã¯è¿ãå¤ã2ã¤ããã2ã¤ç®ã¯ä½¿ããªãããã _ ã§ãããããä½¿ããªããã¨ãæç¤ºã
    s := strconv.Itoa(-42)
    println(i, s)
}
```

åã®ç¢ºèªã¯ãreflect.TypeOf() ã§ã§ããã
```go
fmt.Println(reflect.TypeOf(num1))
// println(reflect.TypeOf(num1)) ï¼ã¤ã¾ããã«ãã¤ã³ã®printlnï¼ã§ã¯å¤ãªåºåã«ãªãã
```

# æ¨æºå¥å
```go:main.go
import (
  "os"
  "strconv"
)

func main() {
  number1, _ := strconv.Atoi(os.Args[1]) // os.Argsã§æ¨æºå¥åãåãåã
  number2, _ := strconv.Atoi(os.Args[2])
  println("åè¨:", number1+number2)
}
```
```bash
# å®è¡
go run src/helloworld/main.go 3 5

# çµæ
åè¨: 8
```

# Printé¢æ°ã«ã¤ãã¦

```go:main.go
import "fmt"

func main() {
  fmt.Print("Hello", "world!")
  fmt.Print("Hello", "world!")
  // -> Helloworld!Helloworld!

  // Printlnã®å ´åã¯ãã¹ãã¼ã¹ã¨æ¹è¡ãæ¿å¥ããã
  fmt.Println("Hello", "world!")
  fmt.Println("Hello", "world!")
  // -> Hello world!
  //    Hello world!

  hello := "Hello world!"
  // Printf : ãã©ã¼ããããæå®ãã¦åºåãã
  fmt.Printf("%s\n", hello) // %s : æå­ã®ã¿ãåºå (""ãªã)
  fmt.Printf("%#v\n", hello) // %#v : Goã®ææ³ã§ã®è¡¨ç¾ãåºå
  // -> Hello world!
  //    "Hello world!"
}
```
:::message
Printfã®ãã©ã¼ãããæå®æ¹æ³ã«ã¤ãã¦ã¯ãä¸ã®è¨äºãè©³ç´°ã«æ¸ãã¦ããã
:::
https://qiita.com/rock619/items/14eb2b32f189514b5c3c


# é¢æ°
```go:main.go
import (
  "os"
  "strconv"
)

func main() {
  sum := sum(os.Args[1], os.Args[2])
  println("åè¨:", sum)
}

// func é¢æ°å(å¼æ°å å) è¿ãå¤ã®å { ... }
func sum(number1 string, number2 string) int {
  int1, _ := strconv.Atoi(number1)
  int2, _ := strconv.Atoi(number2)
  return int1 + int2
}
```
```go
// func é¢æ°å(å¼æ°å å) (è¿ãå¤å è¿ãå¤ã®å) { ... }
func sum(number1 string, number2 string) (result int) {
  int1, _ := strconv.Atoi(number1)
  int2, _ := strconv.Atoi(number2)
  result = int1 + int2
  return result
}
```

## ãã¤ã³ã¿ã¼
  å¤ãé¢æ°ã«æ¸¡ãã¨ãã**ãã®é¢æ°åã§ã®å¤æ´ã¯ãå¼ã³åºãåã«å½±é¿ãä¸ããªã**ã

  :::message
  Go ã¯"å¤æ¸¡ã"ã®è¨èªã
  ã¤ã¾ããé¢æ°ã«å¤ãæ¸¡ããã³ã«ãGoããã®å¤ãåãåã£ã¦**ã­ã¼ã«ã«ã³ãã¼(ã¡ã¢ãªåã®æ°ããå¤æ°)ãä½æ**ããã
  :::

```go:main.go
func main() {
  first := "ã¸ã§ã³"
  updateName(first)
  println(first) // ã¸ã§ã³ ã¨åºåããã
}

func updateName(name string) {
  name = "ç°ä¸­" // update
}
```

  updateNameé¢æ°ã§è¡ãå¤æ´ã mainé¢æ°ã®firstå¤æ°ã«ãåæ ãããã«ã¯ããã¤ã³ã¿ã¼ãä½¿ç¨ããã
  **ãã¤ã³ã¿ã¼ã§ãå¤ã§ã¯ãªãã¢ãã¬ã¹ã¡ã¢ãªãæ¸¡ããã¨ã§ãå¼ã³åºãåã«ãåæ ããã**ã

  #### ãã¤ã³ã¿ã¼
  **å¤æ°ã®ã¡ã¢ãªã¢ãã¬ã¹**ãæ ¼ç´ããå¤æ°ã
  #### &æ¼ç®å­
  ãã®å¾ã«ãããªãã¸ã§ã¯ãã®ã¢ãã¬ã¹ãåå¾ã
  #### *æ¼ç®å­
  ãã¤ã³ã¿ã¼ãéåç§ãããã¤ã¾ãããã¤ã³ã¿ã¼ã«æ ¼ç´ãããã¢ãã¬ã¹ã«ãããªãã¸ã§ã¯ãã¸ã¢ã¯ã»ã¹ããã

```go:main.go
func main() {
  first := "ã¸ã§ã³"
  updateName(&first) // ãã¤ã³ã¿ã¼(ã¡ã¢ãªã¢ãã¬ã¹)ãæ¸¡ã
  println(first) // ç°ä¸­ ã¨åºåããã
}

func updateName(name *string) { // æ³¨: å¤æ°åã§ãªããåã®ã¨ãªãã«*ãæ¸ã
  *name = "ç°ä¸­" // ãã¤ã³ã¿ã¼åã®æå­åãupdate
}
```

# ããã±ã¼ã¸
## ã¹ã³ã¼ã
:::message alert
Goã«ã¯ãpublic ã private ã­ã¼ã¯ã¼ããå­å¨ããªãã
å¤æ°ãé¢æ°ã®**åé ­æå­ã å°æå­ or å¤§æå­** ã§å¤æ­ãããã
:::

| ãã©ã¤ãã¼ã | ãããªãã¯ |
| ---- | ---- |
| å°æå­ | å¤§æå­ |

```go:sum.go
// èªä½ããpackage
package calculator

// ããã±ã¼ã¸ã®ä¸­ããããå¼ã³åºããªã
var logMessage = "[LOG]"

// ã©ãããã§ãã¢ã¯ã»ã¹ã§ãã
var Version = "1.0"

// ããã±ã¼ã¸ã®ä¸­ããããå¼ã³åºããªã
func internalSum(number int) int {
  return number - 1
}

// ã©ãããã§ãã¢ã¯ã»ã¹ã§ãã
func Sum(number1, number2 int) int {
  return number1 + number2
}
```

## ã¢ã¸ã¥ã¼ã«ãä½æ
ä¸ã®ã³ã¼ãã§ã¢ã¸ã¥ã¼ã«ãä½æããã«ã¯ããã®ã³ã¼ã(ãã¡ã¤ã«)ãããã«ã¬ã³ããã£ã¬ã¯ããªã§ `go mod init` ãå®è¡ããã
```
go mod init github.com/myuser/calculator
```
â github.com/myuser/calculator ãã¢ã¸ã¥ã¼ã«åã«ãªãã
â go.mod ãçæãããã
```go:go.mod
module github.com/myuser/calculator

go 1.16
```

## ä½æããã¢ã¸ã¥ã¼ã«ãä½¿ã
```go:main.go
// èªä½ï¼calculatorï¼ããã±ã¼ã¸ãä½¿ã
import "github.com/myuser/calculator"

func main() {
  total := calculator.Sum(3, 5)
  println(total)
  println("version: ", calculator.Version)
}
```

:::message alert
ä¸è¨ã®ããã«ã³ã¼ããæ¸ãã ãã§ã¯ä½¿ããªãã
:::

1. å¼ã³åºãå´ã®ãã¡ã¤ã«ããããã£ã¬ã¯ããªã§ `go mod init` ãå®è¡ããã
```
go mod init github.com/myuser/calculator
```
2. çæããã go.mod ãã¡ã¤ã«ãç·¨éã(è¿½å )
```diff go:go.mod
module helloworld

go 1.14

+ require github.com/myuser/calculator v0.0.0

+ replace github.com/myuser/calculator => ../calculator
```

# ifæ
```go:main.go
import "fmt"

func givemeanumber() int {
  return -1
}

func main() {
  // å¤æ°numãå®ç¾©ãã¦ããããifåã§ä½¿ã
  if num := givemeanumber(); num < 0 {
  fmt.Println(num, "is negative")
  } else if num < 10 {
  fmt.Println(num, "has only one digit")
  } else {
  fmt.Println(num, "has multiple digits")
  }

  // ifæã§å®ç¾©ããå¤æ°numã¯ãifå¤ã§ä½¿ããªã
  fmt.Println(num) // ã¨ã©ã¼ undifed: num
}
```
:::message alert
ifæã§å®ç¾©ããå¤æ°numã¯ãifå¤ã§ä½¿ããªã
:::

# swichæ
```go:main.go
import (
  "fmt"
  "math/rand"
  "time"
)

func main() {
  sec := time.Now().Unix() // UNIXã¿ã¤ã   
  rand.Seed(sec) // seedå¤ã¨ãã¦UNIXã¿ã¤ã ãä½¿ããå®è¡ã®åº¦(1ç§æ¯?)ã«ã©ã³ãã ãªå¤ãçæ
  i := rand.Int31n(10) // 0~10 ã®ä¹±æ°
  fmt.Println(i)
  
  switch i {
  case 0:
  fmt.Println("zero")
  case 1:
  fmt.Println("one")
  case 2:
  fmt.Println("two")
  default: // ããã©ã«ã
  fmt.Println("default output")
  }

  fmt.Println("ok")
}
```

## switchæã«é¢æ°ãä½¿ç¨ã§ãã
  switchå¥ã«ããcaseå¥ã«ãé¢æ°ãä½¿ããã
```go:main.go
import (
  "fmt"
  "time"
)

func main() {
  switch time.Now().Weekday().String() {
  case "Monday", "Tuesday", "Wednesday", "Thursday", "Friday":
  fmt.Println("å¹³æ¥")
  default:
  fmt.Println("ä¼æ¥")
  }

  fmt.Println(time.Now().Weekday().String())
}
```

## fallthrough
caseãæ¤è¨¼ããªã
```go:main.go
import "fmt"

func main() {
  switch num := 15; {
  case num < 50:
  fmt.Printf("%d ã¯50ä»¥ä¸\n", num)
  fallthrough
  case num > 100:
  fmt.Println("100ä»¥ä¸")
  fallthrough // caseãæ¤è¨¼ããªã
  case num < 200:
  fmt.Println("200ä»¥ä¸")
  }
  // -> 15 ã¯50ä»¥ä¸
  //    100ä»¥ä¸  (fallthroughã«ãã£ã¦ãcaseæ(num > 100)ãç¡è¦ããã)
  //    200ä»¥ä¸
}
```

# foræ
- forã®å¾ãã«`()ã«ãã³`ä¸è¦ã
- 3ã¤ã®å¼æ°ã¯ã`; ã»ãã³ã­ã³`ã§åºåãã
```go:main.go
import "fmt"

func main() {
  sum := 0
  
  for i := 1; i < 100; i++ {
  sum += i
  }
  fmt.Println("sumã¯", sum)
}
```
## while
#### Goã« while ã¯ç¡ãã®ã§ãforã§å®ç¾ããã
- foræã«ã¯ãæ¡ä»¶å¼ã®ã¿ ã§OKã
```go:main.go
import (
  "fmt"
  "math/rand"
  "time"
)

func main() {
  var num int64 // ç©ºã®å¤æ°
  rand.Seed(time.Now().Unix()) // ä¹±æ° çæå¨
  
  // numããã¾ãã¾ 5 ã«ãªããªãéãã«ã¼ããã (= while)
  for num != 5 {
  num = rand.Int63n(15) // 0~15ã®ä¹±æ°
  fmt.Println(num)
  }
}
```

## ç¡éã«ã¼ãã¨break
break : ã«ã¼ãããæãã
```go:main.go
import (
  "fmt"
  "math/rand"
  "time"
)

func main() {
  var num int32
  sec := time.Now().Unix()
  rand.Seed(sec)

  for {
  fmt.Print("ã«ã¼ãä¸­")

  if num = rand.Int31n(10); num == 5 { // å¤æ°ãå®ç¾©ãã¦ããããifåã§ä½¿ã
  fmt.Println("çµäº")
  break // ã«ã¼ãããæãã
  }

  // breakå®è¡æã«ã¯ããã®è¡ã¯å®è¡ãããªã
  fmt.Println(num)
  }
}
```

## continue
continueä»¥ä¸ã®å¦çã¯è¡ãããæ¬¡ã®ã«ã¼ãã«ç§»ã
```go:main.go
func main() {
  sum := 0
  for num := 1; num <= 10; num++ {
  // numã3ã®åæ°ãªããsumã«è¶³ããªã
  if num%3 == 0 {
  continue // continueä»¥ä¸ã®å¦çã¯è¡ãããæ¬¡ã®ã«ã¼ãã«ç§»ã
  }
  fmt.Println(num)
  sum += num
  }
  fmt.Println(sum)
}
```

# deferé¢æ°
éå»¶å®è¡ããã
```go:main.go
import "fmt"

func main() {
  for i := 1; i <= 3; i++ {
  defer fmt.Println("defer", -i) // [1, 2, 3]ã¨ã¹ã¿ãã¯çã«ä¿ç®¡ããã3, 2, 1ã®é ã§éå»¶å®è¡ãããï¼åãåºãããï¼
  fmt.Println("regular", i) // ããã§"regular 3"ãçµããã¨ãdeferãéå»¶å®è¡ããã

  /* åºå
  regular 1
  regular 2
  regular 3
  defer -3
  defer -2
  defer -1
  */
  }
}
```

```go:main.go
import (
  "fmt"
  "io"
  "io/ioutil"
  "os"
)

func main() {
  f, err := os.Create("notes.txt") // ãã¡ã¤ã«æ°è¦ä½æ
  if err != nil {
  return
  }
  defer f.Close() // éãããå¿ããªãããã«deferã§éå»¶å®è¡ãäºç´ãã¦ããï¼ä»¥ä¸ã«å¤§éã®ã³ã¼ããããå ´åãªã©ï¼

  // ifæã§å¤æ°ãå®ç¾©ãããã¿ã¼ã³
  if _, err = io.WriteString(f, "ãããæ¸ãè¾¼ã¾ãã"); err != nil {
  return
  }

  out, _ := ioutil.ReadFile("notes.txt") // ãã¡ã¤ã«èª­ã¿è¾¼ã¿
  fmt.Println(string(out)) // åºå

  f.Sync() // ã¡ã¢ãªä¸ã®ãã¡ã¤ã«åå®¹ããã£ã¹ã¯ã«æ¸ãåºãï¼åå®¹ãåæï¼
}
```

# ä¾å¤å¦ç
panic ã¨ recover ã®çµã¿åããã¯ãGo ã§ã®ç¹å¾´çãªä¾å¤å¦çæ¹æ³ã
ï¼ä»ã®ãã­ã°ã©ãã³ã°è¨èªã§ã¯ãtry/catchãï¼

## panicé¢æ°
ãã­ã°ã©ã ãå¼·å¶çã«ãããã¯ã«ãããã­ã°ã¡ãã»ã¼ã¸ãåºåãã¦ã¯ã©ãã·ã¥ããã
```go:main.go
import "fmt"

func main() {
  g(0) // é¢æ°g()ãå®è¡
  fmt.Println("finish") // panicã«ããããã®è¡ã¯å®è¡ãããªã
}

func g(i int) {
  if i > 3 {
  fmt.Println("ãããã¯å") // 2. panicåã®å¦çã¯ããµã¤ãã«2çªç®ã«å®è¡ããã
  panic("ãããã¯") // 4. deferã®å¾ã«å®è¡ããã
  }

  defer fmt.Println("defer:", i) // 3. panic("ãããã¯")ããåã«å®è¡ããã

  fmt.Println("print:", i) // 1. ã¾ãã¯ãã®è¡ãå®è¡ããã
  g(i + 1) // åå¸°(panicãç¡ããã°ãç¡éã«ç¹°ãè¿ã)
}

/* åºå
  print: 0
  print: 1
  print: 2
  print: 3
  ãããã¯å
  defer: 3
  defer: 2
  defer: 1
  defer: 0
  panic: ãããã¯

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

## recoveré¢æ°
`panic()`å¾ã«ãå¶å¾¡ã§ããã
`defer()`ã®ä¸­ã§å®è¡ã§ããã
`panic()`ã¨éããã­ã°ã¡ãã»ã¼ã¸ãåºåããªãã
```go:main.go
import "fmt"

func main() {
  defer func() { // 5. panicãçºçããã®ã§ãdeferã®å¾ã§å®è¡ããã
  if r := recover(); r != nil { // panicãèµ·ããã¨ãã«ãcatchããããäºç´
  fmt.Println("ãªã«ãã¼ãã", r) // r = "ãããã¯"
  }
  }()

  g(0)
  fmt.Println("finish!!") // panicã«ããããã®è¡ã¯å®è¡ãããªã
}

func g(i int) {
  if i > 3 {
  fmt.Println("ãããã¯å") // 2. panicåã®å¦çã¯ããµã¤ãã«2çªç®ã«å®è¡ããã
  panic("ãããã¯") // 4. panicçºç
  }

  defer fmt.Println("defer:", i) // 3. recover() ããåã«å®è¡ããã

  fmt.Println("print:", i) // 1. ã¾ãã¯ãã®è¡ãå®è¡ããã
  g(i + 1) // åå¸°(panicãç¡ããã°ãç¡éã«ç¹°ãè¿ã)
}

/* åºå
  print: 0
  print: 1
  print: 2
  print: 3
  ãããã¯å
  defer: 3
  defer: 2
  defer: 1
  defer: 0
  ãªã«ãã¼ãã ãããã¯
/*
```

# ç·´ç¿åé¡ï¼å¾©ç¿ï¼
## FizzBuzz
```go:main.go
func main() {
  fizzBuzz(15)
  fizzBuzz(10)
  fizzBuzz(9)
  fizzBuzz(2)
}

func fizzBuzz(i int) {
  switch {
  case i%15 == 0:
  fmt.Println("FizzBuzz")
  case i%5 == 0:
  fmt.Println("Buzz")
  case i%3 == 0:
  fmt.Println("Fizz")
  default:
  fmt.Println(i)
  }
}
```

## å¹³æ¹æ ¹ãæ¨æ¸¬ãã
```go:main.go
func main() {
  guessSquare(25)
}

func guessSquare(i float64) {
  // è¨ç®éä¸­å¤ï¼åæå¤ã¯1ï¼
  sqroot := 1.00
  // è¨ç®çµæ
  guess := 0.00

  // è¨ç®ããã®ã¯10åã¾ã§
  for count := 1; count <= 10; count++ {
  // è¨ç®ããï¼ä¸è¨ãã¥ã¼ãã³æ³ï¼
  // sqroot n+1 = sqroot n â (sqroot n * sqroot n â x) / (2 * sqroot n)
  guess = sqroot - (sqroot*sqroot-i)/(2*sqroot)

  if sqroot == guess {
  // è¨ç®åå¾ã§çµæãåãã§ããã°ããããå¹³æ¹æ ¹
  fmt.Println("å¹³æ¹æ ¹ã¯:", guess)
  break // ã«ã¼ã10åä»¥ä¸ã§ãçµäº
  } else {
  // ã«ã¼ã
  fmt.Println("è¨ç®éä¸­:", guess)
  sqroot = guess
  }
  }
}
```

## å¥åããæ°å¤ãåºåãã
```go:main.go
func main() {
  val := 0

  // ç¹°ãè¿ãæ´æ°ã®å¥åãæ±ãã¾ãã ã«ã¼ãã®çµäºæ¡ä»¶ã¯ãã¦ã¼ã¶ã¼ãè² ã®æ°ãå¥åããå ´åã§ãã
  for val >= 0 {
  fmt.Print("æ°å­ãå¥å : ")
  fmt.Scanf("%d", &val)
  if val == 0 {
  // æ°ã 0 ã®å ´åã¯ã0 is neither negative nor positive ã¨åºåãã¾ãã æ°ã®è¦æ±ãç¶ãã¾ãã
  fmt.Println(val, "is neither negative nor positive")
  } else if val < 0 {
  // ã¦ã¼ã¶ã¼ãè² ã®æ°ãå¥åãããããã­ã°ã©ã ãã¯ã©ãã·ã¥ããã¾ãã ãã®å¾ãã¹ã¿ãã¯ ãã¬ã¼ã¹ ã¨ã©ã¼ãåºåãã¾ã
  panic("è² ã®æ°ãªã®ã§çµäº")
  } else {
  // æ°ãæ­£ã®å¤ã®å ´åã¯ãYou entered: X ã¨åºåãã¾ã (X ã¯å¥åãããæ°)ã æ°ã®è¦æ±ãç¶ãã¾ãã
  fmt.Println("å¥åããã®ã¯ :", val)
  }
  }
}
```

# éå
ç¹å®ã®åã®**åºå®é·**ã®ãã¼ã¿æ§é ã
0åä»¥ä¸ã®è¦ç´ ãä¿æãããã¨ãã§ãããããã**å®£è¨ã¾ãã¯åæåããã¨ãã«ãµã¤ãºã®å®ç¾©ãå¿è¦**ã
ã¾ãã**ä½æå¾ã«ãµã¤ãºãå¤æ´ãããã¨ãã§ããªã**ã
ãããã®çç±ã«ãããéåã¯Goãã­ã°ã©ã ã§ã¯ä¸è¬ã«ä½¿ç¨ãããªãããã¹ã©ã¤ã¹ã¨ãããã®åºç¤ã¨ãªã£ã¦ããã
```go:éå
å¤æ° := [è¦ç´ æ°] å {å¤, å¤, ..}
```

```go:main.go
func main() {
  var a [3]int
  a[1] = 10

  fmt.Println(a[0]) // 0 : intã®å ´åãåæå¤ã0ãªã®ã§ 0 ã¨åºåããã
  fmt.Println(a[1]) // 10
  
  fmt.Println(len(a)) // 3 : len()ã¯è¦ç´ æ°ãåå¾ãã

  fmt.Println(a[len(a)-1]) // 0
}
```

### è¦ç´ æ°ãããããªãå ´åã¯çç¥ã§ããã
```go:éå
å¤æ° := [...] å {å¤, å¤, ..}
```

```go:main.go
func main() {
  // 5åã®è¦ç´ ãå¨ã¦åããå¿è¦ã¯ãªãã4, 5åç®ã«ã¯ãåæå¤ã®ç©ºæå­""ãã»ããããã¦ããã
  cities := [5]string{"æ±äº¬", "å¤§éª", "ç¥æ¸"}

  fmt.Println(cities) // [æ±äº¬ å¤§éª ç¥æ¸  ]
}

func main() {
  cities := [...]string{"æ±äº¬", "å¤§éª", "ç¥æ¸"} // è¦ç´ æ°ãçç¥

  fmt.Println(cities) // [æ±äº¬ å¤§éª ç¥æ¸]  ä¸è¨ã¨éã£ã¦ãç©ºæå­ãå«ã¾ãã¦ããªã
  
  fmt.Println(len(cities)) // 3
}
```

### 2æ¬¡åéå
```go:main.go
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
/* åºå
  Row 0 : [1 2 3 4 5]
  Row 1 : [2 4 6 8 10]
  Row 2 : [3 6 9 12 15]
  [[1 2 3 4 5] [2 4 6 8 10] [3 6 9 12 15]]
/*
```

# ã¹ã©ã¤ã¹
ã¹ã©ã¤ã¹ã¯ãåãåã®è¦ç´ ãé£ç¶ãã¦ãããã¨ãè¡¨ããã¼ã¿åã
ãã ããéåã¨ã®å¤§ããªéãã¯ã**ã¹ã©ã¤ã¹ã®ãµã¤ãºã¯åºå®ã§ã¯ãªãåçã§ãã**ã¨ãããã¨ã
```go:ã¹ã©ã¤ã¹
å¤æ° := [] å {å¤, å¤, ..} // éåã¨éãã[]åã«è¦ç´ æ°ã®æå®ä¸è¦
```

ã¹ã©ã¤ã¹ã®ã³ã³ãã¼ãã³ãã¯æ¬¡ã®3ã¤ã®ã¿ã
- **åºã«ãªãéå**ã®**æåã®è¦ç´ ã¸ã®ãã¤ã³ã¿ã¼** : ãã®è¦ç´ ã¯ãå¿ãããéåã®æåã®è¦ç´  array[0] ã§ããã¨ã¯éãã¾ããã
- ã¹ã©ã¤ã¹ã®**é·ã** : ã¹ã©ã¤ã¹åã®è¦ç´ æ°ã
- ã¹ã©ã¤ã¹ã®**å®¹é** : ã¹ã©ã¤ã¹ã®å§ããããåºã«ãªãéåã®çµããã¾ã§ã®è¦ç´ æ°ã

ã¤ã¾ãã**åºã«ãªãéåã«å¯¾ãã¦ãããä½ç½®ããããä½ç½®ã¾ã§åå¾ããéå** ã¨ãããã¨ã

```go:main.go
func main() {
  // ã¹ã©ã¤ã¹ãå®£è¨ï¼ãµã¤ãºãæå®ããªãï¼
  months := []string{"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"}

  fmt.Println(months)
  fmt.Println("length:", len(months)) // è¦ç´ æ°
  fmt.Println("capacity:", cap(months)) // å®¹é
}
```

## ã¹ã©ã¤ã¹ã®æ¡å¼µ
### ã¹ã©ã¤ã¹æ¼ç®å­ s[i:p] 
- s
  éåã
- i 
  **æ°ããã¹ã©ã¤ã¹ã«è¿½å ãããåºã«ãªãéå(ã¾ãã¯å¥ã®ã¹ã©ã¤ã¹)ã®æåã®è¦ç´ ã¸ã®ãã¤ã³ã¿ã¼**ã
  éååã®ã¤ã³ããã¯ã¹ä½ç½®iã«ããè¦ç´ ( **array[i]** )ã«å¯¾å¿ã
- p
  **æ°ããã¹ã©ã¤ã¹ã«è¿½å ãããåºã«ãªãéåã®æå¾ã®è¦ç´ ã¸ã®ãã¤ã³ã¿ã¼**ã
  éååã®ã¤ã³ããã¯ã¹ä½ç½®p-1ã«ããè¦ç´ ( **array[p-1]** )ã«å¯¾å¿ã
```go:main.go
func main() {
  months := []string{"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"}
  fmt.Println("length:", len(months))
  fmt.Println("capacity:", cap(months))

  quarter1 := months[0:3]
  quarter2 := months[3:6]
  quarter3 := months[6:9]
  quarter4 := months[9:12]

  fmt.Println(quarter1, len(quarter1), cap(quarter1)) // [January February March] 3 12
  fmt.Println(quarter2, len(quarter2), cap(quarter2)) // [April May June] 3 9
  fmt.Println(quarter3, len(quarter3), cap(quarter3)) // [July August September] 3 6
  fmt.Println(quarter4, len(quarter4), cap(quarter4)) // [October November December] 3 3
  // lenã¯ããã®ã¹ã©ã¤ã¹ãæã¤è¦ç´ æ°ã
  // capã¯ãåºã«ãªãéåãåºæºã«ãã©ãããã¹ã©ã¤ã¹ãããã«ãã£ã¦å¤ããã

  quarter2Extend := quarter2[:4] // ã¹ã©ã¤ã¹ãæ¡å¼µï¼è¦ç´ æ°ã3â4ã«ï¼
  fmt.Println(quarter2, len(quarter2), cap(quarter2)) // [April May June] 3 9
  fmt.Println(quarter2Extend, len(quarter2Extend), cap(quarter2Extend)) // [April May June July] 4 9
}
```

## è¦ç´ ã®è¿½å 
è¦ç´ ã®è¿½å ã«å¯¾ãã¦ãã¹ã©ã¤ã¹ã®å®¹é(cap)ã¯èªåçã«2ã®nä¹ã§å¢ãããï¼ã¡ã¢ãªãèªåçã«ç¢ºä¿ãããï¼
```go:main.go
func main() {
  var numbers []int
  for i := 0; i < 10; i++ {
  numbers = append(numbers, i) // è¦ç´ ãè¿½å 
  fmt.Printf("%d\tcap=%d\t%v\n", i, cap(numbers), numbers)
  }
}
/* åºå
  0       cap=1   [0]
  1       cap=2   [0 1]
  2       cap=4   [0 1 2]
  3       cap=4   [0 1 2 3]
  4       cap=8   [0 1 2 3 4]
  5       cap=8   [0 1 2 3 4 5]
  6       cap=8   [0 1 2 3 4 5 6]
  7       cap=8   [0 1 2 3 4 5 6 7]
  8       cap=16  [0 1 2 3 4 5 6 7 8]
  9       cap=16  [0 1 2 3 4 5 6 7 8 9]
/*
```

## è¦ç´ ã®åé¤
2éãããã

### 1. append()
```go:main.go
func main() {
  letters := []string{"A", "B", "C", "D", "E"} // ã¹ã©ã¤ã¹
  remove := 2 // åé¤ããè¦ç´ ã®ä½ç½®

  if remove < len(letters) {

  fmt.Println("Before", letters, "Remove", letters[remove]) // Before [A B C D E] Remove C

  fmt.Println(letters[remove]) // C
  fmt.Println(letters[:remove]) // [A B] = Cã®æåã¾ã§
  fmt.Println(letters[remove+1:]) // [D E] = Cä»¥é
  letters = append(letters[:remove], letters[remove+1:]...) // (åãåºãã)[A B]ã«ã(åãåºãã)[D E]ãè¿½å ãã¦ãã

  fmt.Println("After", letters) // After [A B D E]

  }
}
```

### 2. copy()
#### ã³ãã¼ããªãã¨ã©ããªããï¼ï¼ãªãã³ãã¼ããã®ãï¼
ã¹ã©ã¤ã¹ã®è¦ç´ ãå¤æ´ããã¨ãåºã«ãªãéåã®è¦ç´ ãå¤æ´ãããã
ãªã®ã§ãåãéåãåºã«ãã¦ãã ä»ã®ã¹ã©ã¤ã¹ã«ãå½±é¿ãåãã§ãã¾ãã
```go:main.go
func main() {
  letters := []string{"A", "B", "C", "D", "E"} // ã¹ã©ã¤ã¹
  fmt.Println("Before", letters)

  slice1 := letters[0:2] // A, B
  slice2 := letters[1:4] // B, C, D

  slice1[1] = "X"
  
  fmt.Println("slice1", slice1) // slice1 [A X]
  fmt.Println("slice2", slice2) // slice2 [X C D] : åãéåãåºã«ãã¦ãããããè¦ç´ ãå¤æ´ããã¦ãã
  
  fmt.Println("After", letters) // After [A X C D E] : åºã«ãã¦ããéåèªä½ããå¤æ´ããã
}
```

#### éåãã³ãã¼ãã¦ä½¿ã
```go:main.go
func main() {
  letters := []string{"A", "B", "C", "D", "E"}
  fmt.Println("Before", letters)

  slice1 := letters[0:2] // A, B

  slice2 := make([]string, 3) // cap=3ã¨æå®ãã ç©ºã®ã¹ã©ã¤ã¹ãçæããã
  copy(slice2, letters[1:4]) // slice2ã«letters[1:4]ãã³ãã¼ãã -> B, C, D

  slice1[1] = "X"
  
  fmt.Println("slice1", slice1) // slice1 [A X]
  fmt.Println("slice2", slice2) // slice2 [B C D] : è¦ç´ ãå¤æ´ããã¦ããªã!
  
  fmt.Println("After", letters) // After [A X C D E] : slice1ãåºã«ãã¦ããéåã¯å½ç¶å¤æ´ããã
}
```

# ããã
ããã·ã¥ã®ãã¨ã
```go:ããã
å¤æ° := [ã­ã¼ã®å] ããªã¥ã¼ã®å {å¤, å¤, ..}
// []å
// ã»éå : è¦ç´ æ°
// ã»ã¹ã©ã¤ã¹ : ãªã
// ã»ããã : ã­ã¼ã®å
```

```go:main.go
func main() {
  studentAge := map[string]int{
  "john": 32,
  "bob": 31,
  }
  fmt.Println(studentAge) // map[bob:31 john:32]
  fmt.Println(studentAge["john"]) // 32
}
```

ç©ºã®ããããä½ãã«ã¯ãmake()ãä½¿ããï¼ã¹ã©ã¤ã¹ãåæ§ï¼
```go
studentAge := make(map[string]int)
```

## é ç®ã®è¿½å ãåé¤
```go:main.go
func main() {
  // ç©ºã®ããããä½æ
  studentAge := make(map[string]int)
  fmt.Println(studentAge) // map[]
  
  // ãããã«è¿½å 
  studentAge["john"] = 32
  studentAge["bob"] = 31
  fmt.Println(studentAge) // map[bob:31 john:32]

  // ãããåã®é ç®ã«ã¢ã¯ã»ã¹
  fmt.Println("john's age is", studentAge["john"]) // john's age is 32
  
  // ãããåã«ãªãã­ã¼ã§ã¢ã¯ã»ã¹ããã¨ãåæå¤ãè¿ãï¼ä»åã¯intãªã®ã§ 0ï¼
  fmt.Println("hogehoge's age is", studentAge["hogehoge"]) // john's age is 0

  // ãããåã«å­å¨ããããç¢ºèªãããå ´å
  age, exist := studentAge["hogehoge"] // 2ã¤ç®ã®æ»ãå¤ã§boolãè¿ã
  if exist {
    fmt.Println("hogehoge's age is", age)
  } else {
  fmt.Println("not exist")
  }


  // é ç®ã®åé¤
  delete(studentAge, "john") // delete(ããã, åé¤ããé ç®ã®ã­ã¼)
  fmt.Println(studentAge) // map[bob:31]

  // å­å¨ããªãé ç®ãåé¤
  delete(studentAge, "hogehoge") // ã¨ã©ã¼ï¼ãããã¯ï¼ã«ãªããªã
}
```

# ãããåã§ã«ã¼ãããã
range ãä½¿ã£ã¦ããã¹ã¦ã®é ç®ã«ã¢ã¯ã»ã¹ã§ããã
```go
range ããã
```

```go:main.go
func main() {
  studentAge := map[string]int{
  "john": 32,
  "bob": 31,
  }

  // å¦çã¤ã¡ã¼ã¸
  // 1. rangeã«ãã£ã¦ããããã®1ã¤ç®("john": 32)ãname, ageã«ä»£å¥ãããã
  // 2. {}åãå®è¡ããã
  for name, age := range studentAge {
  fmt.Printf("%s\t%d\n", name, age)
  // john    32
    // bob     31
  }

  for name := range studentAge {
  fmt.Printf("%s\t", name) // john    bob
  }

  for _, age := range studentAge {
  fmt.Printf("%d\t", age) // 32      31
  }
}
```

# æ§é ä½
```go:main.go
type Employee struct {
  ID        int
  FirstName string
  LastName  string
}

func main() {
  employee1 := Employee{1001, "Bob", "Hoge"}
  fmt.Println(employee1) // {1001 Bob Hoge}

  employee2 := Employee{FirstName: "John", LastName: "Fuga"}
  fmt.Println(employee2) // {0 John Fuga}
  fmt.Println(employee2.ID) // 0

  employee2.ID = 1002
  fmt.Println(employee2) // {1002 John Fuga}
}
```

#### ãã¤ã³ã¿ãä½¿ã£ã¦ãã³ãã¼ã¨åã®æ§é ä½ãå¤æ´
```go:main.go
func main() {
  employee := Employee{FirstName: "John", LastName: "Fuga"}
  fmt.Println(employee) // {0 John Fuga}

  // ãã¤ã³ã¿ä½æ
  employeeCopy := &employee
  employeeCopy.FirstName = "bob"
  fmt.Println(employeeCopy) // &{0 bob Fuga}
  
  // åã®æ§é ä½ãå¤æ´ããã¦ãã
  fmt.Println(employee) // {0 bob Fuga}
}
```

#### æ§é ä½ã®åãè¾¼ã¿
```go:main.go
type Person struct {
  ID        int
  FirstName string
  LastName  string
}

type Employee struct {
  Person // Personãåãè¾¼ã
  ManagerID int
}

type Contractor struct {
  Person // Personãåãè¾¼ã
  CompanyID int
}

func main() {
  employee := Employee{
    // åæåã®éã¯ãPersonãã£ã¼ã«ãï¼æ§é ä½ï¼ãæç¤ºããªãã¨ãããªã
    Person: Person{
      FirstName: "john",
    },
  }
  fmt.Println(employee) // {{0 john } 0}

  // åæåã§ãªãã®ã§ãPersonçµç±ã§ãªãã¦OK
  employee.LastName = "doe"
  fmt.Println(employee) // {{0 john doe} 0}
}
```

#### æ§é ä½âJSON
```go:main.go
type Person struct {
  ID        int
  FirstName string `json:"name"`
  LastName  string `json:"lastname,omitempty"`
}

type Employee struct {
  Person
  ManagerID int
}

type Contractor struct {
  Person
  CompanyID int
}

func main() {
  employees := []Employee{
    Employee{
      Person: Person{
        LastName: "hoge", FirstName: "john",
      },
    },
    Employee{
      Person: Person{
        LastName: "fuga", FirstName: "bob",
      },
    },
  }

  data, _ := json.Marshal(employees) // æ§é ä½ãjsonã«å¤æ
  fmt.Printf("%s\n", data)
  // [
  //   {"ID":0,"name":"john","lastname":"hoge","ManagerID":0},
  //   {"ID":0,"name":"bob","lastname":"fuga","ManagerID":0}
  // ]
  
  
  var decorded []Employee
  json.Unmarshal(data, &decorded) // jsonãæ§é ä½ã«å¤æ(æ»ã)
  fmt.Printf("%v", decorded)
  // [
  //   {{0 john hoge} 0}
  //   {{0 bob fuga} 0}
  // ]
}
```

# ãã£ããããæ°å
```go:main.go
package main

import (
  "fmt"
  "os"
  "strconv"
)

func main() {
  // å¥åãåãåã
  input_int, _ := strconv.Atoi(os.Args[1])

  // ãã£ããããæ°åãä½æ
  slice := generateFibonacciSequence(input_int)
  fmt.Println(slice)
}

func generateFibonacciSequence(input int) []int {
  if input < 2 {
    // å¥åã2æªæºã®å ´åè¨ç®ã§ããªããããpanicãèµ·ããnilã¹ã©ã¤ã¹ãè¿ã
    panic([]int{})
  } else {
    // å¥åã2ä»¥ä¸ã®å ´åããã£ããããæ°åãçæãã
    
    // ã¹ã©ã¤ã¹ã®åæå¤ã®æå³
    // 0 : ã­ã¸ãã¯ä¸ãã£ãã»ããä¾¿å©ãªã®ã§è¨­ããããã¼ã®æ°å¤ã(ä¸è¦ãªã®ã§å¾ã§åé¤ãã)
    // 1 : å­å¨ãããã¨ãç¢ºå®ãã¦ãããæåã®å¤
    slice := []int{0, 1}

    // [å¥åãããæ°å¤]åã®ãã£ããããæ°åãçæ
    for i := 0; i < input; i++ {
      // è¿½å ããæ°å¤
      add_num := slice[len(slice)-2] + slice[len(slice)-1]

      // ã¹ã©ã¤ã¹ã«è¦ç´ è¿½å 
      slice = append(slice, add_num)
    }

    // åæå¤ã¨ãã¦ç¨æãã¦ãã 0 ãåé¤
    slice = append(slice[:0], slice[1:]...)

    return slice
  }
}
```

# ã¨ã©ã¼å¦ç
## ã¨ã©ã¼å¦çã«é¢ããæ¨å¥¨äºé 
Goã§ã¨ã©ã¼ãå¦çããå ´åã¯ãæ¬¡ã®ãããªæ¨å¥¨äºé ãèæ®ãããã¨ã

1. ã¨ã©ã¼ãäºæ³ããã¦ããªãã¦ããã¨ã©ã¼ããªããã©ãããå¸¸ã«ç¢ºèªãã¾ãã
  ãã®ããã§ãä¸è¦ãªæå ±ãã¨ã³ãã¦ã¼ã¶ã¼ã«å¬éãããªãããã«ãã¾ãã
  (ã¨ã©ã¼å¦çã®æãç°¡åãªæ¹æ³ã¯ãif æ¡ä»¶ãä½¿ç¨ãããã¨)
2. ã¨ã©ã¼ã¡ãã»ã¼ã¸ã«ãã¬ãã£ãã¯ã¹ãå«ãã¦ãã¨ã©ã¼ã®çºçåããããããã«ãã¾ãã
  ãã¨ãã°ãããã±ã¼ã¸ãé¢æ°ã®ååãå«ãããã¨ãã§ãã¾ãã
3. å¯è½ãªéããåå©ç¨å¯è½ãªã¨ã©ã¼å¤æ°ãä½æãã¾ãã
4. ã¨ã©ã¼ãè¿ããã¨ã¨ããããã¯ã®éããçè§£ãã¾ãã 
  ä»ã«å¯¾å¦æ¹æ³ããªãå ´åã¯ããããã¯ãçºçãã¾ãã
  ãã¨ãã°ãä¾å­é¢ä¿ã®æºåãã§ãã¦ããªãå ´åããã­ã°ã©ã ã¯åä½ãã¾ãã (æ¢å®ã®åä½ãå®è¡ããå ´åãé¤ãã¾ã)ã
5. å¯è½ãªéãå¤ãã®è©³ç´°æå ±ã§ã¨ã©ã¼ãã­ã°ã«è¨é²ã(æ¬¡ã®ã»ã¯ã·ã§ã³ã§èª¬æ)ãã¨ã³ãã¦ã¼ã¶ã¼ãçè§£å¯è½ãªã¨ã©ã¼ãåºåãã¾ãã

```go:main.go
type Employee struct {
  ID        int
  FirstName string
  LastName  string
  Address   string
}

func main() {
  employee, err := getInformation(1001)
  if err != nil {
  // ãªã«ããã
  } else {
  fmt.Println(employee)
  }
}

func getInformation(id int) (*Employee, error) {
  employee, err := apiCallEmployee(1000)

  // ä¿®æ­£å
  // return employee, err

  // ä¿®æ­£å¾
  // 1. ã¨ã©ã¼ãäºæ³ããã¦ããªãã¦ããã¨ã©ã¼ããªããã©ãããå¸¸ã«ç¢ºèªãã¾ãã
  //   ãã®ããã§ãä¸è¦ãªæå ±ãã¨ã³ãã¦ã¼ã¶ã¼ã«å¬éãããªãããã«ãã¾ãã
  if err != nil {
  return nil, err // apiCallEmployee()ããã¨ã©ã¼ãè¿ã£ã¦æ¥ã¦ããå ´åã¯ãerrã®ã¿è¿ã
  }
  return employee, nil
}

// 3. å¯è½ãªéããåå©ç¨å¯è½ãªã¨ã©ã¼å¤æ°ãä½æãã¾ãã
var ErrNotFound = errors.New("Employee not found")

func apiCallEmployee(id int) (*Employee, error) {
  if id != 1001 {
  return nil, ErrNotFound
  }

  employee := Employee{LastName: "hoge", FirstName: "john"}
  return &employee, nil
}
```

# ã­ã°
```go:main.go
import (
  "fmt"
  "log"
)

func main() {
  log.Print("printãã")
  // â 2021/08/09 07:23:53 printãã

  log.Fatal("fatalãã")
  fmt.Print("ããã¯è¦ããªã")
  // â 2021/08/09 07:23:53 fatalãã
  // Fatal()ã«ããããã­ã°ã©ã ãåæ­¢ãããããFatal()ä»¥éã¯å®è¡ãããªã
}
```

```go:main.go
import (
  "fmt"
  "log"
)

func main() {
  log.Print("printãã")
  // â 2021/08/09 07:23:53 printãã

  log.Panic("panicãã")
  fmt.Print("ããã¯è¦ããªã")
  // â panic: panicãã
  //   goroutine 1 [running]:
  //   log.Panic(0xc0000c3f58, 0x1, 0x1)
  //       /usr/local/go/src/log/log.go:354 +0xae
  //   main.main()
  //       /Users/itotakuya/projects/go/src/helloworld/main.go:814 +0xa5
  // Panic()ä»¥éã¯å®è¡ãããªã
}
```

```go:main.go
import (
  "log"
)

func main() {
  log.Print("printãã")
  // â 2021/08/09 07:23:53 printãã

  log.SetPrefix("main():") // ã­ã°ã«prefixãè¨­å®ã§ãã
  log.Print("printã­ã°")
  log.Fatal("Fatalã­ã°")
  // â main():2021/08/09 07:29:49 printã­ã°
  //   main():2021/08/09 07:29:49 Fatalã­ã°
  //   exit status 1
}
```

### ãã¡ã¤ã«ã«ã­ã°ãè¨é²ãã
```go:main.go
import (
  "log"
  "os"
)

func main() {
  // ãã¡ã¤ã«ä½æ
  // os.OpenFile(ãã¡ã¤ã«å, ãã©ã°(ãã¡ã¤ã«ãç¡ããã°ä½æããç­), ãã¼ããã·ã§ã³)
  file, err := os.OpenFile("ingo.log", os.O_CREATE|os.O_APPEND|os.O_WRONLY, 0644)

  if err != nil {
  log.Fatal("ã¨ã©ã¼çºç")
  }

  // å¾ã§ãã¡ã¤ã«ã«ã­ã°ãè¨é²ããããfile.Close()ãå¿ããªãããã«deferäºç´
  defer file.Close()

  // fileã«ã­ã°ãè¨é²ãããã¨ãå®£è¨
  log.SetOutput(file)
  // éå¸¸ã©ãããlog.Print()ãå®è¡ãããã¨ã§ããã¡ã¤ã«ã«è¨é²ããã
  log.Print("ã­ã°")
}
```

# ã¡ã½ãã
ã¡ã½ãããè¿½å ãããã¨ã§ãä½æããæ§é ä½ã«åä½ãè¿½å ã§ããã

```go:main.go
type triangle struct {
  size int
}

// func (å¤æ° æ§é ä½) ã¡ã½ããå() è¿å´å {
func (t triangle) perimeter() int {
  return t.size * 3
}

func main() {
  t := triangle{3}
  fmt.Println("ãã®ä¸è§å½¢ã®å¤å¨:", t.perimeter())
}
```

### ãã¤ã³ã¿ã¼ãåç§ãã
ã¡ã½ããã«ãå¤æ°èªä½ã§ãªããã¤ã³ã¿ã¼ãæ¸¡ãã»ããè¯ãå ´åããããï¼å¤æ°ã®ã¢ãã¬ã¹ãåç§ãããï¼
- ã¡ã½ããã§å¤æ°ãæ´æ°ããå ´å
- å¼æ°ãå¤§ããããå ´åï¼ãã®ã³ãã¼ãåé¿ãããï¼

```go:main.go
type triangle struct {
  size int
}

// func (å¤æ° æ§é ä½) ã¡ã½ããå() è¿å´å {
func (t triangle) perimeter() int {
  return t.size * 3
}

// *triangle ã§ãæ§é ä½èªä½ã§ãªã æ§é ä½ã®ãã¤ã³ã¿ã¼ãæ¸¡ãããã«å®ç¾©
func (t *triangle) doubleSize() {
  t.size *= 2
}

func main() {
  t := triangle{3}
  t.doubleSize() // ãã¤ã³ã¿ãåç§ãã¦ tãæ´æ°ããã

  fmt.Println("size:", t.size) // size: 6
  fmt.Println("perimeter:", t.perimeter()) // perimeter: 18
}
```

#### åãè¾¼ã¾ããæ§é ä½ã®ã¡ã½ãããå¼ã³åºããã¨ãã§ãã
```go:main.go
type triangle struct {
  size int
}

type coloredTriangle struct {
  triangle // æ§é ä½ãåãè¾¼ã
  color string
}

// func (å¤æ° æ§é ä½) ã¡ã½ããå() è¿å´å {
func (t triangle) perimeter() int {
  return t.size * 3
}

func main() {
  t := coloredTriangle{triangle{3}, "blue"}
  fmt.Println("size:", t.size)
  // coloredTriangleããtriangleæ§é ä½åãã®ã¡ã½ãããä½¿ãã
  fmt.Println("perimeter:", t.perimeter())
}
```

#### ã¡ã½ããã®ãªã¼ãã¼ã­ã¼ã
â» ãªã¼ãã¼ã­ã¼ã(å¤éå®ç¾©) : åãååã®é¢æ°ç­ãå®ç¾©ãããã¨
```go:main.go
type triangle struct {
  size int
}

// func (å¤æ° æ§é ä½) ã¡ã½ããå() è¿å´å {
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

  // coloredTriangleã§ãcoloredTriangleæ§é ä½åãã®ã¡ã½ãããä½¿ã
  fmt.Println("perimeter:", t.perimeter()) // perimeter: 15
  // coloredTriangleã§ãtriangleæ§é ä½åãã®ã¡ã½ãããä½¿ãã
  fmt.Println("perimeter:", t.triangle.perimeter()) // perimeter: 9
}
```

### ã«ãã»ã«å
```go:size.go
package geometry

type Triangle struct {
  size int
}

func (t *Triangle) doubleSize() {
  t.size *= 2
}

func (t *Triangle) SetSize(size int) {
  t.size = size
}

func (t *Triangle) Perimeter() int {
  t.doubleSize()
  return t.size * 3
}
```
```go:main.go
func main() {
  t := geometry.Triangle{}
  t.SetSize(3)
  
  // publicãªã®ã§å®è¡ã§ãã
  fmt.Println(t.Perimeter()) // 18

  // privateãªã¡ã½ãããªã®ã§ã¢ã¯ã»ã¹ã§ããªã
  fmt.Println(t.doubleSize()) // t.doubleSize undefined
}

```

# ã¤ã³ã¿ã¼ãã§ã¤ã¹
```go:main.go
// ã¤ã³ã¿ã¼ãã§ã¤ã¹ ------------------
type Shape interface {
  Perimeter() float64
  Area() float64
}

// 2ã¤ã®æ§é ä½ ------------------
// æ§é ä½ã«ã¯ã¤ã³ã¿ã¼ãã§ã¤ã¹ã¨ã®é¢é£ã¯å®ç¾©ããã¦ããªã

// Squareæ§é ä½ 
type Square struct {
  size float64
}

func (s Square) Area() float64 {
  return s.size * s.size
}

func (s Square) Perimeter() float64 {
  return s.size * 4
}

// Circleæ§é ä½
type Circle struct {
  radius float64
}

func (c Circle) Area() float64 {
  return math.Pi * c.radius * c.radius
}

func (c Circle) Perimeter() float64 {
  return 2 * math.Pi * c.radius
}

// é¢æ° ------------------
// ãã©ã¡ã¼ã¿ã¨ãã¦Shapeã¤ã³ã¿ã¼ãã§ã¤ã¹ãæ±ãã
func printInformation(s Shape) {
  fmt.Printf("%T\n", s)
  fmt.Println("Area:", s.Area())
  fmt.Println("Perimeter:", s.Perimeter())
  fmt.Println()
}

// main ------------------
func main() {
  var s Shape = Square{3}
  printInformation(s)

  c := Circle{6}
  printInformation(c)
}
```

### stringerã¤ã³ã¿ã¼ãã§ã¤ã¹ãå®è£ãã
```go:main.go
type Person struct {
  Name, Country string
}

// Stringerã¤ã³ã¿ã¼ãã§ã¤ã¹ = fmt.Printfã§ä½¿ç¨ãããã¤ã³ã¿ã¼ãã§ã¤ã¹
// åã®String()ã¡ã½ããããªã¼ãã¼ã©ã¤ããã(ã«ã¹ã¿ã )
func (p Person) String() string {
  return fmt.Sprintf("%v is from %v", p.Name, p.Country)
}

func main() {
  rs := Person{"john", "USA"}
  ab := Person{"hoge", "UK"}
  
  // æå­åãPrintfããã¨ããªã¼ãã¼ã©ã¤ãããéãã«åºåããã
  fmt.Printf("%s\n%s\n", rs, ab)
  // john is from USA
  // hoge is from UK
  fmt.Printf("%q\n%q\n", rs, ab)
  // "john is from USA"
  // "hoge is from UK"

  // æå­åä»¥å¤ãPrintfããã¨ãã¯ãé¢ä¿ãªã
  fmt.Printf("%d\n", 4) // 4
}
```

### æ¢å­ã®å®è£ãæ¡å¼µãã
```go:main.go
type GithubResponse []struct {
  Fullname string `json:"full_name"`
}

// ç¬èªã®Writeã¤ã³ã¿ã¼ãã§ã¤ã¹ãå®ç¾©
type custumWriter struct{}

// Writeã¡ã½ããããªã¼ãã¼ã©ã¤ã
func (w custumWriter) Write(p []byte) (n int, err error) {
  var resp GithubResponse
  json.Unmarshal(p, &resp)
  for _, r := range resp {
  fmt.Println(r.Fullname)
  }
  return len(p), nil
}

func main() {
  resp, err := http.Get("https://api.github.com/users/microsoft/repos?page=15&per_page=5")
  if err != nil {
  fmt.Println("Error:", err)
  os.Exit(1)
  }

  // io.Copy(os.Stdout, resp.Body)
  // ä¸è¨ã§ã¯ãå¤§éã®åºåã«ãªã

  // 1è¡ã®è¦ãããåºåã«å¤æ´ãã
  // Cory()é¢æ°ã®ã½ã¼ã¹ãè¦ãã¨ãä¸è¨ã«ãªã£ã¦ãããããWriteã¤ã³ã¿ã¼ãã§ã¤ã¹ãä½¿ç¨ãã
  //   func Copy(dst Writer, src Reader) (written int64, err error)
  // ã¾ããCory()é¢æ°ã®ã½ã¼ã¹ãè¦ãã¨ãWriteã¡ã½ãããå¼ã³åºããã¨ããããããã
  // Writeã¡ã½ããããªã¼ãã¼ã©ã¤ãããã°åºåãå¤æ´ã§ãã
  writer := custumWriter{}
  io.Copy(writer, resp.Body)
}
```

### ã«ã¹ã¿ã  ãµã¼ãã¼ API ã®ä½æ
```go:main.go
type dollars float32

// dollarsåãæå­åã§åºåããå ´åã¯ $25.00 ã¿ããã«$ãã¤ãã¦å°æ°ç¹2æ¡ã¾ã§è¡¨ç¤ºããããã«å å·¥ãã
func (d dollars) String() string {
  return fmt.Sprintf("$%.2f", d)
}

type database map[string]dollars

// ListenAndServe()ã§å¼ã°ããServeHTTPã¡ã½ãããã«ã¹ã¿ã 
func (db database) ServeHTTP(w http.ResponseWriter, req *http.Request) {
  for item, price := range db {
  fmt.Fprintf(w, "%s: %s\n", item, price)
  }
}

func main() {
  // databaseåãã¤ã³ã¹ã¿ã³ã¹å
  db := database{"Go T-shirt": 25, "Go Jacket": 55}
  fmt.Println(db)

  log.Fatal(http.ListenAndServe("localhost:8000", db))
  // localhost:8000ã«ã¦ã
  // Go Jacket:$55.00
  // Go T-shirt:$25.0 ã¨è¡¨ç¤ºããã

  // ListenAndServeã®å®ç¾© : 
  //   package http
  //   type Handler interface {
  //       ServeHTTP(w ResponseWriter, r *Request)
  //   }
  //   func ListenAndServe(address string, h Handler) error
}
```

# ã³ã³ã«ã¬ã³ã·ã¼
## è¿½å ã§åèã«ããæç®
  https://zenn.dev/hsaki/books/golang-concurrency/viewer

## goroutineã¨ã¯
ãªãã¬ã¼ãã£ã³ã° ã·ã¹ãã ã§ã®å¾æ¥ã®ã¢ã¯ãã£ããã£ã§ã¯ãªããè»½éã¹ã¬ããã§ã®åæã¢ã¯ãã£ããã£ã§ãã 
ï¼ä¸¦åã§ãªãï¼ä¸¦è¡å¦çãã§ããã

```go:main.go
func main() {
  start := time.Now()

  apis := []string{
  "https://management.azure.com",
  "https://dev.azure.com",
  "https://api.github.com",
  "https://outlook.office.com/",
  "https://api.somewhereintheinternet.com/",
  "https://graph.microsoft.com",
  }
  
  // 1ã¤1ã¤ã®ãµã¤ãã«ãé çªã«GETãã
  for _, api := range apis {
    // GETãªã¯ã¨ã¹ããéã
  _, err := http.Get(api)

  // ã¨ã©ã¼æ
  if err != nil {
  fmt.Printf("ERROR: %s is down!\n", api)
  continue
  }
  // æåæ
  fmt.Printf("SUCCESS: %s is up and runnging!\n", api)
  }

  elapsed := time.Since(start)
  fmt.Printf("Done! It took %v seconds\n", elapsed.Seconds())

  // 2ç§ãããããã£ã¦ãã¾ã
}
```

ä¸è¨ã®ã³ã¼ããæ¹è¯ã
```go:main.go
func main() {
  start := time.Now()

  apis := []string{
  "https://management.azure.com",
  "https://dev.azure.com",
  "https://api.github.com",
  "https://outlook.office.com/",
  "https://api.somewhereintheinternet.com/",
  "https://graph.microsoft.com",
  }

  ch := make(chan string)

  for _, api := range apis {
  // APIãã¨ã«goroutineãä½æ
  go checkAPI(api, ch)
  }

  // ãµã¤ãæ°ã¨åãã ããchannelããåä¿¡ããåºå
  for i := 0; i < len(apis); i++ {
  fmt.Print(<-ch)
  }

  elapsed := time.Since(start)
  fmt.Printf("Done! It took %v seconds\n", elapsed.Seconds())

  // 0.6ç§ã»ã©ã«ç­ç¸®
}

func checkAPI(api string, ch chan string) {
  _, err := http.Get(api)
  if err != nil {
  ch <- fmt.Sprintf("ERROR: %s is down!\n", api)
  return
  }
  // Sprintfãä½¿ãã®ã¯ãã¾ã åºåã¯ããããªããã
  ch <- fmt.Sprintf("SUCCESS: %s is up and running!\n", api)
}
```

## ãã£ãã«ã®æ¹å
```go:main.go
// ãã£ãã«ã«éä¿¡
func send(ch chan<- string, message string) {
  fmt.Printf("Senging: %#v\n", message)
  ch <- message
}
// ãã£ãã«ã«åä¿¡
func read(ch <-chan string) {
  fmt.Printf("Receving: %#v\n", <-ch)
}

func main() {
  ch := make(chan string, 1)
  send(ch, "hello") // Senging: "hello"
  read(ch) // Receving: "hello"
}
```

## å¤éå
selectã¹ãã¼ãã¡ã³ãã¯switchã¹ãã¼ãã¡ã³ãã¨åãããã«æ©è½ãã¾ããããã£ãã«ç¨ã§ãã

```go:main.go
func process(ch chan string) {
  time.Sleep(3 * time.Second)
  ch <- "Done processing!"
}

func replicate(ch chan string) {
  time.Sleep(1 * time.Second)
  ch <- "Done replicating!"
}

func main() {
  // ãã£ãã«ãä½æ
  ch1 := make(chan string)
  ch2 := make(chan string)

  // goroutineãä½æ
  go process(ch1)
  go replicate(ch2)

  for i := 0; i < 2; i++ {
  // selectå¥ã§ã¤ãã³ã(ãã£ãã«ããã®éä¿¡)ãå¾ã¡ãæ¥ããcaseåãå®è¡ãã
  // ããã«(2ã¤ä»¥ä¸ã®)ã¤ãã³ããçºçããã¾ã§å¾æ©ãããå ´åãå¿è¦ã«å¿ãã¦ã«ã¼ããä½¿ãå¿è¦ããã(for)
  select {
  case process := <-ch1:
  fmt.Println(process)
  case replicate := <-ch2:
  fmt.Println(replicate)
  }
  fmt.Println("ããã¯ã¤ãã³ãçµäºæ¯ã«å®è¡ããã")
  }
}
  /*
  Done replicating!
  ããã¯ã¤ãã³ãçµäºæ¯ã«å®è¡ããã
  Done processing!
  ããã¯ã¤ãã³ãçµäºæ¯ã«å®è¡ããã
  /*
```

## èª²é¡
### æ¹è¯å
```go:main.go
// ãã£ããããæ°åãçæ
func fib(number float64) float64 {
  x, y := 1.0, 1.0
  for i := 0; i < int(number); i++ {
  x, y = y, x+y
  }

  // ã©ã³ãã ã§ã0~3ç§å¾ã¤
  r := rand.Intn(3)
  time.Sleep(time.Duration(r) * time.Second)

  return x
}

func main() {
  start := time.Now()

  // 1~15ã¾ã§ã®ãã£ããããæ°åãåºå
  for i := 1; i < 15; i++ {
  n := fib(float64(i))
  fmt.Printf("Fib(%v): %v\n", i, n)
  }

  // å®è¡æéãåºå
  elapsed := time.Since(start)
  fmt.Printf("Done! %v seconds\n", elapsed.Seconds())
}
```

### æ¹è¯å¾
#### 1. éåº¦æ¹åver.
ã³ã³ã«ã¬ã³ã·ã¼ãå®è£ããæ¹è¯ãã¼ã¸ã§ã³ã
ç¾æç¹ã§ã¯ããããæéã¯æ°ç§ (15 ç§ä»¥å)ã®ã¯ãã§ãã
ãããã¡ã¼ããã®ãã£ãã«ãä½¿ç¨ããå¿è¦ãããã¾ãã

#### TODO: ãã¨ã§å¾©ç¿ãã¦ãããèª²é¡è§£ããã¨

#### 2. ã¦ã¼ã¶å¥åã«ããå¶å¾¡ver.
ã¦ã¼ã¶ã¼ã fmt.Scanf() é¢æ°ãä½¿ç¨ãã¦ã¿ã¼ããã«ã« quit ã¨å¥åããã¾ã§ãã£ããããæ°ãè¨ç®ããæ°ãããã¼ã¸ã§ã³ãä½æãã¾ãã 
ã¦ã¼ã¶ã¼ã Enter ã­ã¼ãæ¼ããå ´åã¯ãæ°ãããã£ããããæ°ãè¨ç®ããå¿è¦ãããã¾ãã
ã¤ã¾ãã1 ãã 10 ã¾ã§ã®ã«ã¼ãã¯ãªããªãã¾ãã

#### TODO: ãã¨ã§å¾©ç¿ãã¦ãããèª²é¡è§£ããã¨

# Go ã§ãã­ã°ã©ã ãä½æãã¦ãã¹ããã (ãªã³ã©ã¤ã³éè¡ãã­ã¸ã§ã¯ã)
2ã¤ã®ãã­ã¸ã§ã¯ããä½æãã¾ãã
1ã¤ã¯ãã­ã°ã©ã ã®ã³ã¢ã­ã¸ãã¯ç¨ã§ããã1ã¤ã¯WebAPIãéãã¦ã­ã¸ãã¯ãå¬éããããã®ãã®ã§ãã
## æ©è½ã¨è¦ä»¶
- é¡§å®¢ãå£åº§ãä½æã§ããããã«ãã¾ãã
- é¡§å®¢ããéãå¼ãåºããã¨ãã§ããããã«ãã¾ãã
- é¡§å®¢ãå¥ã®å£åº§ã«ééã§ããããã«ãã¾ãã
- é¡§å®¢ãã¼ã¿ã¨æçµæ®é«ãå«ãåå¼æç´°ãæä¾ãã¾ãã
- åå¼æç´°ãå°å·ããããã® Web API ããã¨ã³ããã¤ã³ããéãã¦å¬éãã¾ãã

## ãã¹ãå®è¡ã³ãã³ã
```
go test -v
```
ãã¹ã¦ã® `*_test.go` ãã¡ã¤ã«ãæ¤ç´¢ããã

## ã³ã¼ã
```go:src/bankcore/bank_test.go
package bank

import "testing"

// ãã¹ã
func TestAccount(t *testing.T) {
	account := Account{
		Customer: Customer{
			Name:    "John",
			Address: "Los Angeles",
			Phone:   "123 555 0147",
		},
		Number:  1001,
		Balance: 0,
	}

	// ååã®å¥åãç¡ããã°ã¢ã«ã¦ã³ããä½ããªã
	if account.Name == "" {
		t.Error("ã¢ã«ã¦ã³ããä½ãã¾ãã")
	}
}

func TestDeposit(t *testing.T) {
	account := Account{
		Customer: Customer{
			Name:    "John",
			Address: "Los Angeles",
			Phone:   "123 555 0147",
		},
		Number:  1001,
		Balance: 0,
	}

	// å¥é
	account.Deposit(10)

	// æ®é«ãå¥éé¡ã¨ç­ãããªãå ´åãã¨ã©ã¼ãèµ·ãã
	if account.Balance != 10 {
		t.Error("å¥éããã®ã«æ®é«ãæ´æ°ããã¦ãã¾ãã")
	}
}

func TestDepositInvalid(t *testing.T) {
	account := Account{
		Customer: Customer{
			Name:    "John",
			Address: "Los Angeles",
			Phone:   "123 555 0147",
		},
		Number:  1001,
		Balance: 0,
	}

	// ãã¤ãã¹ã®éé¡ãå¥éããã®ã«ãã¨ã©ã¼ãnilã§ããå ´åãã¨ã©ã¼ãèµ·ãã
	if err := account.Deposit(-10); err == nil {
		t.Error("æ­£ã®æ°å¤ã ããå¥éé¡ã¨ãã¦è¨±å¯ãããã¹ã")
	}
}

func TestWithdraw(t *testing.T) {
	account := Account{
		Customer: Customer{
			Name:    "John",
			Address: "Los Angeles",
			Phone:   "123 555 0147",
		},
		Number:  1001,
		Balance: 0,
	}

	// å¥éãã¦ãåãé¡ãå¼ãåºã
	account.Deposit(10)
	account.Withdraw(10)

	// æ®é«ã¯0ã®ã¯ã
	if account.Balance != 0 {
		t.Error("å¼ãåºããã®ã«æ®é«ãæ´æ°ããã¦ãã¾ãã")
	}
}

func TestStatement(t *testing.T) {
	account := Account{
		Customer: Customer{
			Name:    "John",
			Address: "Los Angeles",
			Phone:   "123 555 0147",
		},
		Number:  1001,
		Balance: 0,
	}

	account.Deposit(100)
	statement := account.Statement()

	// æç´°è¡¨ç¤ºãæ­£ããã
	if statement != "1001 - John - 100" {
		t.Error("æç´°è¡¨ç¤ºãæ­£ããããã¾ãã")
	}
}

func TestSend(t *testing.T) {
	send_account := Account{
		Customer: Customer{
			Name:    "John",
			Address: "Los Angeles",
			Phone:   "123 555 0147",
		},
		Number:  1001,
		Balance: 0,
	}
	receive_account := Account{
		Customer: Customer{
			Name:    "Alice",
			Address: "Los Angeles",
			Phone:   "987 555 0147",
		},
		Number:  1002,
		Balance: 0,
	}

	send_account.Deposit(100)

	// 100ãå¥ã¢ã«ã¦ã³ãã¸éé
	send_account.Send(&receive_account, 100) // éè¦: & ã§ã¢ãã¬ã¹ãæ¸¡ã(åç§æ¸¡ã)
	
  // æç´°è¡¨ç¤ºãæ­£ããã
	send_statement := send_account.Statement()
	if send_statement != "1001 - John - 0" {
		t.Error("æç´°è¡¨ç¤ºãæ­£ããããã¾ãã")
	}

	receive_statement := receive_account.Statement()
	if receive_statement != "1002 - Alice - 100" {
		t.Error("æç´°è¡¨ç¤ºãæ­£ããããã¾ãã")
	}
}
```
```go:src/bankcore/bank.go
package bank

import (
	"errors"
	"fmt"
)

// é¡§å®¢æå ±
type Customer struct {
	Name    string
	Address string
	Phone   string
}

// ã¢ã«ã¦ã³ã
type Account struct {
	Customer
	Number  int32   // å£åº§çªå·
	Balance float64 // æ®é«
}

// å¥é
func (a *Account) Deposit(amount float64) error {
	if amount <= 0 {
		return errors.New("å¥éé¡ã¯0ããå¤§ããå¿è¦ãããã¾ã")
	}

	// æ®é«ã«å¥éé¡ãè¶³ã
	a.Balance += amount
	return nil
}

// å¼ãåºã
func (a *Account) Withdraw(amount float64) error {
	if amount <= 0 {
		return errors.New("æ®é«ã¯0ããå¤§ããå¿è¦ãããã¾ã")
	}

	if a.Balance < amount {
		return errors.New("æ®é«ããå¤§ããé¡ãå¼ãåºãã¾ãã")
	}

	// æ®é«ããå¼ãåºãé¡ãå¼ã
	a.Balance -= amount
	return nil
}

// æç´°è¡¨ç¤º
func (a *Account) Statement() string {
	return fmt.Sprintf("%v - %v - %v", a.Number, a.Name, a.Balance)
}

// éé
// ç¬¬ä¸å¼æ°ã¯ãã¤ã³ã¿(ã¢ãã¬ã¹)
func (send_account *Account) Send(receive_account *Account, amount float64) error {
	if amount <= 0 {
		return errors.New("å¥éé¡ã¯0ããå¤§ããå¿è¦ãããã¾ã")
	}
	if send_account.Balance < amount {
		return errors.New("æ®é«ããå¤§ããé¡ãééã§ãã¾ãã")
	}
	send_account.Withdraw(amount)
	receive_account.Deposit(amount)
	return nil
}
```
```go:src/bankapi/main.go
// æ³¨: ãªããããã®ãã£ã¬ã¯ããªã«ç§»åãã¦go run main.goããªãã¨åããªã
package main

import (
	"fmt"
	"log"
	"net/http"
	"strconv"

	"github.com/msft/bank"
)

var accounts = map[float64]*bank.Account{}

func main() {
	accounts[1001] = &bank.Account{
		Customer: bank.Customer{
			Name:    "John",
			Address: "Los Angeles",
			Phone:   "123 555 0147",
		},
		Number: 1001,
	}

	// ã«ã¼ãã£ã³ã°è¨­å®
	http.HandleFunc("/statement", statement)
	http.HandleFunc("/deposit", deposit)
	http.HandleFunc("/withdraw", withdraw)
	// webãµã¼ãèµ·å
	log.Fatal(http.ListenAndServe("localhost:9000", nil))
}

// æç´°è¡¨ç¤º
// w http.ResponseWriter : ã¬ã¹ãã³ã¹ãæ¸ãæ»ãããã®ãªãã¸ã§ã¯ã
// req *http.Request : httpãªã¯ã¨ã¹ããªãã¸ã§ã¯ã
func statement(w http.ResponseWriter, req *http.Request) {
	// ã¯ã¨ãªæå­åããå£åº§çªå·ãåå¾
	numberqs := req.URL.Query().Get("number")

	if numberqs == "" {
		fmt.Fprintf(w, "å£åº§çªå·ãè¦ã¤ããã¾ãã")
		return
	}

	if number, err := strconv.ParseFloat(numberqs, 64); err != nil {
		fmt.Fprintf(w, "ä¸æ­£ãªå£åº§çªå·ã§ã")
	} else {
		// å£åº§çªå·ãä½¿ã£ã¦ã¢ã«ã¦ã³ããåå¾
		account, ok := accounts[number]
		if !ok {
			fmt.Fprintf(w, "å£åº§çªå· %v ã¯è¦ã¤ããã¾ãã", number)
		} else {
			fmt.Fprintf(w, account.Statement())
		}
	}
}

// å¥é
func deposit(w http.ResponseWriter, req *http.Request) {
	// ã¯ã¨ãªæå­åããå£åº§çªå·ãå¥éé¡ãåå¾
	numberqs := req.URL.Query().Get("number")
	amountqs := req.URL.Query().Get("amount")

	if numberqs == "" {
		fmt.Fprintf(w, "å£åº§çªå·ãè¦ã¤ããã¾ãã")
		return
	}

	if number, err := strconv.ParseFloat(numberqs, 64); err != nil {
		fmt.Fprintf(w, "ä¸æ­£ãªå£åº§çªå·ã§ã")
	} else if amount, err := strconv.ParseFloat(amountqs, 64); err != nil {
		fmt.Fprintf(w, "ä¸æ­£ãªå¥éé¡ã§ã")
	} else {
		// å£åº§çªå·ãä½¿ã£ã¦ã¢ã«ã¦ã³ããåå¾
		account, ok := accounts[number]
		if !ok {
			fmt.Fprintf(w, "å£åº§çªå· %v ã¯è¦ã¤ããã¾ãã", number)
		} else {
			err := account.Deposit(amount)
			if err != nil {
				fmt.Fprintf(w, "%v", err)
			} else {
				fmt.Fprintf(w, account.Statement())
			}
		}
	}
}

// å¼ãåºã
func withdraw(w http.ResponseWriter, req *http.Request) {
	numberqs := req.URL.Query().Get("number")
	amountqs := req.URL.Query().Get("amount")

	if numberqs == "" {
		fmt.Fprintf(w, "å£åº§çªå·ãè¦ã¤ããã¾ãã")
		return
	}

	if number, err := strconv.ParseFloat(numberqs, 64); err != nil {
		fmt.Fprintf(w, "ä¸æ­£ãªå£åº§çªå·ã§ã")
	} else if amount, err := strconv.ParseFloat(amountqs, 64); err != nil {
		fmt.Fprintf(w, "ä¸æ­£ãªå¥éé¡ã§ã")
	} else {
		// å£åº§çªå·ãä½¿ã£ã¦ã¢ã«ã¦ã³ããåå¾
		account, ok := accounts[number]
		if !ok {
			fmt.Fprintf(w, "å£åº§çªå· %v ã¯è¦ã¤ããã¾ãã", number)
		} else {
			err := account.Withdraw(amount)
			if err != nil {
				fmt.Fprintf(w, "%v", err)
			} else {
				fmt.Fprintf(w, account.Statement())
			}
		}
	}
}
```

# æ¬¡ã¯
## goroutineã®çè§£ãæ·±ãã

## å¥ã®ææã§ãå­¦ç¿ãã
ç¶²ç¾çã«èª¬æãã¦ããã¦ããã¹ã©ã¤ã
https://docs.google.com/presentation/d/1RVx8oeIMAWxbB7ZP2IcgZXnbZokjCmTUca-AbIpORGk/edit#slide=id.g4f417182ce_0_80

grpcãã­ãã³ã«ã®ãã¥ã¼ããªã¢ã«
https://github.com/ymmt2005/grpc-tutorial