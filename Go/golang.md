## Multiple result
---
関数は複数の戻り値を返すことができる

```golang
package main

import "fmt"

func swap(x, y int) (int, int) {
	return x+y, x*y
}

func main() {
	a, b := swap(4, 9)
	fmt.Println(a, b)
}
```

## Named return values
---
戻り値となる変数に名前をつけること（**named return values**）ができる。
名前をつけた戻り値の変数を使うと、**return**ステートメントに何も書かずに戻ること（**naked return**）ができます。
※但し、このnaked return は短い関数でのみ利用すべきである。そうでないと読みやすさに影響が出る。

```golang
package main

import "fmt"

func split(sum int) (added, multipled int) {
	added = sum + sum
	multipled = sum * sum
	return
}

func main() {
	fmt.Println(split(17))
}

```

## Variables
---
varステートメントは、変数を宣言する。複数の変数の最後に型を書くことで、変数のリストを宣言できる。
varステートメントは、パッケージ、または関数で利用できる。

```golang
package main

import "fmt"

var c, python, java bool

// こんな風(factored)にもかけるよ
var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)

func main() {
	var i int
	python = true
	
	fmt.Println(i, c, python, java)
}
```

## Variables with initializers
---
var宣言では、変数まいに初期化子（initializer）を与えることができます。
初期化子が与えられている場合、型を省略できます。その変数は、その初期化子が持つ型になる。

```golang
package main

import "fmt"

var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}

```

## Short variables declarations
---
関数の中では、var宣言の代わりに、短い``:=``の代入文を使い、暗黙的な型宣言ができる。
※但し、関数の外では、キーワードで始まる宣言（var, func）が必要で、``:=``は使えない。

```golang
package main

import "fmt"

func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

## Basic types
---
型の種類は以下。

```golang
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr
// 基本的に整数は int で良い
// int, uint, uintptr 型は、32-bitのシステムでは32 bitで、64-bitのシステムでは64 bitです。

byte // uint8 の別名

rune // int32 の別名
     // Unicode のコードポイントを表す

float32 float64

complex64 complex128
```

## Type inference
---
明示的な型を指定せずに変数を宣言する場合、変数の型は右側の変数、値の定数の精度から型推論される。

```golang
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

## Constants
---
定数は、``const``キーワードを使って変数と同じように宣言します。
定数は、文字、文字列、boolean、数値でのみ使えます。
※但し、定数は``:=``を使って宣言できない。