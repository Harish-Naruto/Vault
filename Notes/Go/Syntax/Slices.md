[[Go]] #GoSyntax  #Golang 

slices are typed only by the elements they contain (not the number of elements). An uninitialized slice equals to nil and has length 0.
slices are same as [[array]] but they are without size

**Syntax :** `var var_name []datatype`

To create a slice of N-length we use builtin **make**
```
 s := make([]int, N)
```

**len** returns the length of a slice 
```
len(s)
//output : N
```

**append** return a slice containing one or more values notes taht we need to accept a return a return value from append as we may  get new slice value 
```
s := append(s,1)
```

Slices support a “slice” operator with the syntax slice[low:high] 
```
l := s[2:5]
//output : s[2],s[3],s[4]
```

