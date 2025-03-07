# Closures

V also supports closures.
This means that anonymous functions can inherit variables from the scope they were created in.

## Variables capture

However, unlike a lot of languages, captured variables **must be explicitly listed** after the `fn`
keyword inside square brackets:

```v play
num := 1
str := 'Hello!'

my_closure := fn [num, str] () {
	//           ^^^^^^^^^^ this is a list of captured variables
	println(num)
	println(str)
}

my_closure()
// 1
// Hello!
```

Captured variables are **copied** when the anonymous function is created.
This means that if the original variable is modified after the creation of the function,
the modification won't be reflected in the function.

```v play
mut i := 1
func := fn [i] () int {
	return i
}

println(func() == 1) // true

i = 123

println(func() == 1) // true, captured variable `i` still has the value 1
```

However, the variable can be modified inside the anonymous function.
The change won't be reflected outside, but will be in the later function calls.

```v play
fn new_counter() fn () int {
	mut i := 0
	return fn [mut i] () int {
		i++
		return i
	}
}

c := new_counter()
println(c()) // 1
println(c()) // 2
println(c()) // 3
```

If you need the value to be modified outside the function, use a reference.

```v play
mut i := 0
mut ref := &i
print_counter := fn [ref] () {
	println(*ref)
}

print_counter() // 0
i = 10
print_counter() // 10
```
