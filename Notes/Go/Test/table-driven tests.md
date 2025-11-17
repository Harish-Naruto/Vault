 #GoTest #Golang #GoExtra 

[Table-driven](https://go.dev/wiki/TableDrivenTests) testing is a common and powerful pattern in Go that helps you write clearer, more concise, and easily extensible tests. Instead of writing a separate test function for every single case, you define a collection (a "table") of test cases and iterate through them in a single test function.

## 1. The Problem: Repetitive Test Functions

Imagine you have a simple function you want to test, like `Add()`.

```
// add.go
func Add(a, b int) int {
    return a + b
}
```

Without table-driven tests, you might write a separate test for each scenario. This quickly becomes repetitive and hard to manage.

```
// add_test.go (the repetitive way)
import "testing"

func TestAddPositiveNumbers(t *testing.T) {
    if Add(2, 3) != 5 {
        t.Errorf("Expected 5, got %d", Add(2, 3))
    }
}

func TestAddNegativeNumbers(t *testing.T) {
    if Add(-2, -3) != -5 {
        t.Errorf("Expected -5, got %d", Add(-2, -3))
    }
}

func TestAddZero(t *testing.T) {
    if Add(5, 0) != 5 {
        t.Errorf("Expected 5, got %d", Add(5, 0))
    }
}
```

This approach is verbose. Adding a new test case requires writing a whole new function.

## 2. The Solution: Table-Driven Tests

The table-driven approach consolidates all these cases into a single, clean structure.

**The core idea involves three steps:**

1. Define a `struct` that describes a single test case, including inputs and the expected output.
    
2. Create a slice of these structs (the "table") to hold all your test cases.
    
3. Loop through the slice, executing the test logic for each case.
    

### Example: A Table-Driven Test for `Add()`

Here is how the previous tests can be refactored into a single, clean, table-driven test.

```
// add_test.go (the table-driven way)
import "testing"

func TestAdd(t *testing.T) {
    // 1. Define the test case struct
    testCases := []struct {
        name   string // A name for the test case
        a      int    // First input
        b      int    // Second input
        want   int    // The expected result
    }{
        // 2. Create the "table" of test cases
        {name: "Positive numbers", a: 2, b: 3, want: 5},
        {name: "Negative numbers", a: -2, b: -3, want: -5},
        {name: "Adding zero", a: 5, b: 0, want: 5},
        {name: "Mixed numbers", a: -10, b: 4, want: -6},
    }

    // 3. Loop through the test cases
    for _, tc := range testCases {
        t.Run(tc.name, func(t *testing.T) {
            got := Add(tc.a, tc.b)
            if got != tc.want {
                t.Errorf("Add(%d, %d) = %d; want %d", tc.a, tc.b, got, tc.want)
            }
        })
    }
}
```

### Using `t.Run()`

In the example above, `t.Run()` is used to create a **sub-test** for each case in the table. This is highly recommended because:

- **Clearer Output:** It provides more descriptive test output, naming each test case that fails.
    
- **Isolation:** Each sub-test is treated as a separate test. You can run a specific sub-test using the `go test -run` command (e.g., `go test -run TestAdd/"Negative numbers"`).
    
- **Better Reporting:** Even if one test case fails, the test runner will continue to execute the others.
    

## 3. Why Use Table-Driven Tests?

- **Concise and Readable:** All test data is in one place, making it easy to see the scenarios being tested.
    
- **Easily Extensible:** Adding a new test case is as simple as adding a new line to the table. There is no need to write more boilerplate logic.
    
- **Separation of Concerns:** The test data (the table) is separate from the test execution logic (the loop), which improves clarity.
    
- **Reduces Code Duplication:** You avoid writing the same assertion logic over and over again.
    

This pattern is the idiomatic way to write tests in Go and is used extensively in the standard library and the wider Go ecosystem.