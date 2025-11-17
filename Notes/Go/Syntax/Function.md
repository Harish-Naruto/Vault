#Golang  #GoSyntax

```
func functionName(parameter1 type1, parameter2 type2) returnType {

    // Function body - code to be executed

    return value                       // if returnType is specified
}
```

- Rules:
	- Function name start with lowercase
	- Public functions start with a capital letter, and private ones start with a lowercase letter

Function can return two value:
```
func (d Dictionary) Search(word string) (string, error) {
	definition, ok := d[word]
	if !ok {
		return "", errors.New("could not find the word you were looking for")
	}

	return definition, nil
}
```

you can access this value by using  following method 
```
value,err := funcCall()
```
