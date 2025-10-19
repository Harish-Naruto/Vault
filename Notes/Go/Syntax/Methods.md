[Go] #GoSyntax #Golang 

Methods are only used for struct types and types
#### Syntax
```
func (receiver type) methodName() return_type {
	//func logic
}

//method call

receiver_type.methodeName()

```
#### Example
```
type rect struct {
	width,height int 
}

func (r *rect) area() int {
	return r.width * r.height
}

r := rect{10,20}

fmt.Println("area: ", r.area())

```


==You can call a method using either pointer value== it can automatically handles type conversion between values and pointer for method call 
```
 rp := &r
fmt.Println("area: ", rp.area())
```