[[Go]] #Golang #GoSyntax 
Maps are Go’s built-in associative data type (sometimes called hashes or dicts in other languages).
check [[Range]] to know how to iterate over map.


**CREATING** an empty map :
```
m := make(map[key-type]value-type)
```

**SET** value :
```
m[key] = value
```

if key doesnot exit , the **ZERO VALUED** of the value type is returned:
```
fmt.Println(m[unknown-key])
//output : 0
```

The builtin **LEN** returns the number of key/value pairs when called on a map :
```
len(m)
```

The builtin **DELETE** removes key/value pairs from a map :
```
delete(map_name,key)
```

To remove all key/value pairs from a map, use the clear builtin :
```
clear(map_name)
```

Maps can return 2 values. The second value is a boolean which indicates if the key was found successfully.
```
value,bool := m[key]
```

other way to delcare and initialize a new map :
```
n := map[string]int{"foo": 1, "bar": 2}
```

#### Interesting fact about Maps
An interesting property of maps is that you can modify them without passing as an address to it (e.g &myMap).
So when you pass a map to a function/method, you are indeed copying it, but just the pointer part, not the underlying data structure that contains the data.

A gotcha with maps is that they can be a nil value. A nil map behaves like an empty map when reading, but attempts to write to a nil map will cause a runtime panic. You can read more about maps [here](https://blog.golang.org/go-maps-in-action).

Therefore, you should never initialize a nil map variable:
```
var m map[string]string
```

Instead, you can initialize an empty map or use the make keyword to create a map for you:
```
var dictionary = map[string]string{}

// OR

var dictionary = make(map[string]string)
```


Note that maps appear in the form `map[k:v k:v] `when printed with fmt.Println.