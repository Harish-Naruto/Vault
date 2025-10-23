#Golang #GoSyntax [[Go]]

The `var` keyword allows us to define values global to the package.

Syntax for declaration :
```
var variable_name dataType = value
```

some ways to declear variable are :
```
package main
import "fmt"

func main(){
	
	var a = "initial"
	
	var b,c int = 1,2
	
	var d int
}
```

**NOTE :**  when variable is only declared then the default value is ***zero-valued***  for int  is 0

Shorthand for declaring and initializing a variable is :
```
f:="apple"  //types are auto assigin here
```
