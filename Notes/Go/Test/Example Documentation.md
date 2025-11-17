 #GoExtra #GoTest #Golang 


## What are Go Examples?

In Go, an "example" is a special kind of function that serves a dual purpose:

1. **It's Documentation:** It demonstrates how to use a specific function, making your code easier for others (and your future self) to understand.
    
2. **It's a Test:** The Go tooling can execute your example and verify that its output is correct. This keeps your documentation in perfect sync with your code.
    

If the code changes in a way that breaks the example, the test will fail, alerting you to update the documentation.

## How to Write an Example

Writing an example is simple. You follow two main rules:

1. Create your example in a test file (one that ends with `_test.go`).
    
2. Name the function `Example` followed by the name of the function you are documenting.
    
```
// In mymath_test.go
package mymath

import "fmt"

func ExampleAdd() {
	sum := Add(5, 10)
	fmt.Println(sum)
	// Output: 15
}
```

### The Magic: The `// Output:` Comment

The special `// Output:` comment at the end is the key. When you run `go test`, the tool does the following:
1. **Executes** the code inside `ExampleAdd()`.
2. **Captures** any text printed to standard output.
3. **Compares** the captured text with the content of the `// Output:` comment.

If they match exactly, the example passes. If not, it fails.

## Example vs. Regular Test

The table below summarizes the key differences:

|              |                               |                                       |
| ------------ | ----------------------------- | ------------------------------------- |
| **Feature**  | **ExampleAdd() (Example)**    | **TestAdd() (Unit Test)**             |
| **Purpose**  | 📖 To document and teach      | ✅ To verify correctness and find bugs |
| **Audience** | 👩‍💻 Human developers        | 🤖 The `go test` command              |
| **Method**   | Prints output, checks comment | Uses `t.Errorf` or `t.Fatal` to fail  |
| **Focus**    | A clear, simple "happy path"  | Covers edge cases and complex logic   |

In short:

- Use **Examples** to show how your code is _meant_ to be used.
- Use **Tests** to ensure your code works correctly under _all_ conditions.

## How to Run Examples

You can run your examples just like any other test using the Go toolchain:

```
# Run all tests and examples in the current package
go test

# Run tests and examples with verbose output to see them pass
go test -v
```