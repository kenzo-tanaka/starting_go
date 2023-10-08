- 実行速度がRubyとかの10-100倍くらいになる
- go run コマンド
- OSが提供するライブラリにも依存しないため、実行ファイルは大きくなる
- packageを記述する、1つだけに限定。
- 相対パスでimportしようとすると以下エラーになった。
```
but relative import paths are not supported in module mode
```
- Goのバージョンによるものっぽい。
- https://qiita.com/momotaro98/items/23fa4356389a7e610acc
- main.goを書き換えてみる
```go
package main

import (
	"fmt"
	"github.com/kenzo-tanaka/starting_go/animails"
)

func main() {
	fmt.Println(animals.ElephantFeed())
}
```

```shell
main.go:5:2: no required module provides package github.com/kenzo-tanaka/starting_go/animails: go.mod file not found in current directory or any parent directory; see 'go help modules'
```
- go.modファイルが必要そう
- `go mod init github.com/kenzo-tanaka/starting_go` (https://は不要)
- ここまでやるとgo run main.goが成功する
- パッケージ内の関数を呼ぶときには `animals.ElephantFeed()` みたいにするんだな。
- mainを分割する。同一ディレクトリにmainパッケージのファイルを作成して、mainで関数を呼び出し。このときimportは特に不要。
  - そのままgo runするとundefinedエラー。
  - go run main.go app.go こうか、`go run *.go` でいける。
  - go build はデフォルトで `go build *.go` として動作するので main, app みたいな指定せんでいい
- Goは1つのディレクトリに1つのパッケージ定義のみ、という原則
