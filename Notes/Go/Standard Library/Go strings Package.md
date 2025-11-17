#Golang #IMP_PACKAGE

A quick reference guide to the most commonly used functions in Go's built-in `strings` package. This package provides a rich set of functions for string manipulation.

For more details, always refer to the [official Go documentation](https://pkg.go.dev/strings "null").

### 1. Comparison Functions

#### `Compare`

Performs a lexicographical comparison of two strings.

- **Returns:** `0` if `a == b`, `-1` if `a < b`, and `+1` if `a > b`.
    
- **Signature:** `func Compare(a, b string) int`
    
- **Example:**
    
    ```
    fmt.Println(strings.Compare("a", "b")) // -1
    fmt.Println(strings.Compare("a", "a")) // 0
    fmt.Println(strings.Compare("b", "a")) // 1
    ```
    

#### `EqualFold`

Compares two UTF-8 strings, reporting whether they are equal while ignoring case.

- **Signature:** `func EqualFold(s, t string) bool`
    
- **Example:**
    
    ```
    fmt.Println(strings.EqualFold("Go", "go")) // true
    ```
    

### 2. Searching Functions

#### `Contains`

Checks whether a substring is present within a string.

- **Signature:** `func Contains(s, substr string) bool`
    
- **Example:**
    
    ```
    fmt.Println(strings.Contains("seafood", "foo")) // true
    fmt.Println(str#1-comparison-functionsings.Contains("seafood", "bar")) // false
    ```
    

#### `ContainsAny`

Checks whether any character from a given set (`chars`) is present within a string.

- **Signature:** `func ContainsAny(s, chars string) bool`
    
- **Example:**
    
    ```
    fmt.Println(strings.ContainsAny("team", "i"))     // false
    fmt.Println(strings.ContainsAny("failure", "a & o")) // true
    ```
    

#### `HasPrefix` & `HasSuffix`

Checks if a string starts or ends with a specific prefix or suffix.

- **Signature:** `func HasPrefix(s, prefix string) bool`
    
- **Signature:** `func HasSuffix(s, suffix string) bool`
    
- **Example:**
    
    ```
    fmt.Println(strings.HasPrefix("Gopher", "Go")) // true
    fmt.Println(strings.HasSuffix("Amigo", "go"))  // true
    ```
    

#### `Index` & `LastIndex`

Finds the first or last index of a substring.

- **Returns:** The index of the first instance of the substring, or `-1` if not found.
    
- **Signature:** `func Index(s, substr string) int`
    
- **Signature:** `func LastIndex(s, substr string) int`
    
- **Example:**
    
    ```
    fmt.Println(strings.Index("chicken", "ken")) // 4
    fmt.Println(strings.Index("chicken", "dmr")) // -1
    fmt.Println(strings.LastIndex("go gopher", "go")) // 3
    ```
    

### 3. Splitting & Joining

#### `Split`


Splits a string into a slice of substrings separated by a separator.

- **Signature:** `func Split(s, sep string) []string`
    
- **Example:**
    
    ```
    s := "a,b,c"
    parts := strings.Split(s, ",")
    fmt.Printf("%q\n", parts) // ["a" "b" "c"]
    ```
    

#### `SplitN`

Splits a string into a slice of substrings but limits the number of splits.

- **`n` > 0:** At most `n` substrings; the last substring will be the unsplit remainder.
    
- **`n` == 0:** The result is `nil` (zero substrings).
    
- **`n` < 0:** All substrings are returned.
    
- **Signature:** `func SplitN(s, sep string, n int) []string`
    
- **Example:**
    
    ```
    fmt.Printf("%q\n", strings.SplitN("a,b,c", ",", 2)) // ["a" "b,c"]
    ```
    

#### `Join`

Concatenates the elements of a string slice into a single string, with a separator.

- **Signature:** `func Join(elems []string, sep string) string`
    
- **Example:**
    
    ```
    s := []string{"foo", "bar", "baz"}
    fmt.Println(strings.Join(s, ", ")) // "foo, bar, baz"
    ```
    

### 4. Trimming & Spacing

#### `TrimSpace`

Removes all leading and trailing white space from a string.

- **Signature:** `func TrimSpace(s string) string`
    
- **Example:**
    
    ```
    fmt.Println(strings.TrimSpace(" \t\n I am a Gopher \n\t ")) // "I am a Gopher"
    ```
    

#### `Trim`

Removes all leading and trailing characters contained in the `cutset`.

- **Signature:** `func Trim(s, cutset string) string`
    
- **Example:**
    
    ```
    fmt.Println(strings.Trim("¡¡¡Hello, Gophers!!!", "!¡")) // "Hello, Gophers"
    ```
    

#### `TrimPrefix` & `TrimSuffix`

Removes a leading prefix or a trailing suffix from a string.

- **Signature:** `func TrimPrefix(s, prefix string) string`
    
- **Signature:** `func TrimSuffix(s, suffix string) string`
    
- **Example:**
    
    ```
    fmt.Println(strings.TrimPrefix("Goodbye, world", "Goodbye, ")) // "world"
    fmt.Println(strings.TrimSuffix("Hello, Gophers", ", Gophers"))  // "Hello"
    ```
    

### 5. Case Conversion

#### `ToLower` & `ToUpper`

Converts a string to lower or upper case.

- **Signature:** `func ToLower(s string) string`
    
- **Signature:** `func ToUpper(s string) string`
    
- **Example:**
    
    ```
    fmt.Println(strings.ToLower("Gopher")) // "gopher"
    fmt.Println(strings.ToUpper("gopher")) // "GOPHER"
    ```
    

#### `ToTitle`

Converts a string to title case (capitalizing the first letter of each word).

- **Note:** This function is deprecated. For more complex title casing, use the `golang.org/x/text/cases` and `golang.org/x/text/language` packages.
    
- **Signature:** `func ToTitle(s string) string`
    
- **Example:**
    
    ```
    fmt.Println(strings.ToTitle("her royal highness")) // "Her Royal Highness"
    ```
    

### 6. Replacing Substrings

#### `Replace`

Replaces instances of an old substring with a new one. The `n` argument specifies the maximum number of replacements.

- **`n` < 0:** Replace all occurrences.
    
- **Signature:** `func Replace(s, old, new string, n int) string`
    
- **Example:**
    
    ```
    fmt.Println(strings.Replace("oink oink oink", "oink", "moo", 2)) // "moo moo oink"
    fmt.Println(strings.Replace("oink oink oink", "oink", "moo", -1)) // "moo moo moo"
    ```
    

#### `ReplaceAll`

Replaces all occurrences of an old substring with a new one. It's equivalent to `Replace` with `n = -1`.

- **Signature:** `func ReplaceAll(s, old, new string) string`
    
- **Example:**
    
    ```
    fmt.Println(strings.ReplaceAll("oink oink oink", "oink", "moo")) // "moo moo moo"
    ```
    

### 7. Other Utilities

#### `Count`

Counts the number of non-overlapping instances of a substring.

- **Signature:** `func Count(s, substr string) int`
    
- **Example:**
    
    ```
    fmt.Println(strings.Count("cheese", "e")) // 3
    ```
    

#### `NewReader`

Creates a new `io.Reader` that reads from a string. This is useful for functions that expect a reader instead of a raw string.

- **Signature:** `func NewReader(s string) *Reader`
    
- **Example:**
    
    ```
    reader := strings.NewReader("Hello, Reader!")
    b := make([]byte, 5)
    n, err := reader.Read(b)
    fmt.Printf("%s, n=%d, err=%v\n", b, n, err) // "Hello, n=5, err=<nil>"
    ```
    

#### `NewReplacer`

Creates a `Replacer` object that can perform multiple find-and-replace operations efficiently in a single pass.

- **Signature:** `func NewReplacer(oldnew ...string) *Replacer`
    
- **Example:**
    
    ```
    r := strings.NewReplacer("<", "&lt;", ">", "&gt;")
    fmt.Println(r.Replace("This is <b>HTML</b>!")) // "This is &lt;b&gt;HTML&lt;/b&gt;!"
    ```