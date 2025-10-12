#Golang [[Go]]  #GoExtra #GoTest 


[Benchmarks](https://pkg.go.dev/testing#hdr-Benchmarks) in **Go**  are a way to measure the performance, or speed, of a specific piece of code, like a function or a method. They help us understand how fast our code runs and how it scales under different conditions.

### Syntax :

```
func Benchmark(b *testing.B) {
	//... setup ...
	for b.Loop() {
		//... code to measure ...
	}
	//... cleanup ...
}
```

![[Pasted image 20251007001741.png]]

### Running Command :

General command to run benchmark in a single file:
```
$ go test -bench=.      // . is used to run all benchmark function
```

to see memory allocation details use  `-benchmem` flag:
```
$ go test -bench=. -benchmem
```

