[[Variables]]  [[Go]] #Golang #GoSyntax 

This are same as variables but shorthand can't be used here only difference is that  ***Constant Expressions perform arithmetic with arbitrary precision***.

```
package main

import (
    "fmt"
    "math"
)

const s string = "constant"

func main() {
    fmt.Println(s)

    const n = 500000000

    const d = 3e20 / n
    fmt.Println(d)

    fmt.Println(int64(d))

    fmt.Println(math.Sin(n))
}
```

A numeric constant has no type until its given one such as by an explicit conversion.
A number can be given a type by using it in a context that requires one, such as a variable assignment or function call. For example, here `math.Sin` expects a `float64`