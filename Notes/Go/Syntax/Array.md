 #GoSyntax #Golang 

an array is numbered sequence of elements of a specific length. To create array we need to specify the length of the array.
while [slices](Slices) dont need size similar to vector.

ways to iterate over arrays are mentioned in [[Range]].

#### Syntax
```
var a [size]dataType

b := [5]int{1, 2, 3, 4, 5}

twoD = [2][3]int{
    {1, 2, 3},
    {1, 2, 3},
}
```

You can also have the compiler count the number of elements for you with `...`

```
b = [...]int{1,2,3,4,5,6}
```

If you specify the index with `:`, the elements in between will be zeroed.
```
b = [...]int{100, 3: 400, 500}

//output : [100 0 0 400 500]
```
