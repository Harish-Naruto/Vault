 #Golang #GoSyntax 

Go’s _structs_ are typed collections of fields. They’re useful for grouping data together to form records.
Structs are mutable.
#### Syntax :
```
type person struct {
    name string
    age  int
}
```
creating new struct 
```
p := person{"bob",20}
s := person{name: "Alice", age: 30}
```
accessing value of struct
```
fmt.Println(s.name)
```

omitted field will be zero-valued
```
 fmt.Println(person{name: "Fred"})
 
 //output: {Fred 0}
 
 fmt.Println(person{"fred"}) //this wont work it will show error 
```

If a struct type is only used for a single value, we don’t have to give it a name. The value can have an anonymous struct type. This technique is commonly used for [[table-driven tests]].
```
dog := struct {
        name   string
        isGood bool
    }{
        "Rex",
        true,
    }
```

