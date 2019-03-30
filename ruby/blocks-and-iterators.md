# Ruby / Blocks and iterators

* [Syntax](#syntax)
* [Associating blocks with functions](#associating-blocks-with-functions)
* [Adding arguments to a block](#adding-arguments-to-a-block)
* [Initializing and finalizing code](#initializing-and-finalizing-code)
* [Iterating over data](#iterating-over-data)
* [`map`/`collect`](#map-collect)

## Syntax

Ruby has a special block feature that has  very interesting usages.  
It is especially useful in the context of iterator methods for collections.

```
3.times { puts 'Hello' } # can be split into 3 lines
=> Hello
=> Hello
=> Hello
```

```
3.times do
	puts Hello
end
=> Hello
=> Hello
=> Hello
```

>   
> There is a convention (but not a syntactic rule) that `do-end` is preferred in multiline code over `{}`.
>  

## Associating blocks with functions

The `yield` signifies a call to a block (executing a code chunk of the block) that is associated with this function at that point in the function.

When you call the function, you have to pass the block in such a way that the beginning of the block (either `do` or `{`) should start at the same line as the function name (any extra arguments should come before the block).

```
def check
	puts 'beginning'
	yield
	puts 'end'
end

puts 'outside the function'
check do
	puts 'ok'
end

=> outside the function
=> beginning
=> ok
=> end
```

The last part could be written in other manner.

```
# a)
check do puts 'ok' end

# b)
check { puts 'ok' }

# c)
check {
	puts 'ok'
}
```

### Function with arguments

```
def check(name)
	puts "processing #{name}"
	yield
	puts 'end'
end

puts 'outside the function'
check ('abcd') {
	puts 'Hello'
}

=> outside the function
=> processing abcd
=> Hello
=> end
```

## Adding arguments to a block

```
def check(id)
	puts "processing empid #{id}"
	yield 'John'
	puts 'end'
end

puts 'outside the function'
check (2) do |str|
	puts "Hello #{str}"
end

=> outside the function
=> processing empid 2
=> Hello John
=> end
```

```
def three
	for i in (1..3)
		yield i
	end
end

three do |iter|
	puts iter
end

=> 1
=> 2
=> 3
```

```
def multipl
	yield 3, 4
end

multipl { |a, b| puts a * b }
=> 12
```

### Returning a value from a block

Below is a demonstration of how a block can return a value (the return value from the last statement executed), which may be used in the associated function.

```
def multipl
	value = yield 3, 4
	puts 'value is ' + value.to_s
end

multipl { |a, b| a * b }
=> value is 12
```

## Initializing and finalizing code

Every Ruby source file can declare blocks of code to be run as the file is being loaded (the `BEGIN` blocks) and after the program has finished executing (the `END` blocks).  
  
`BEGIN` blocks are executed in the order they are encountered.  
`END` blocks are executed in reverse order.

```
BEGIN { x = 'a'; puts x }
BEGIN { y = 'b'; puts y }
puts 'general code'
END { a = 'x'; puts a }
END { b = 'y'; puts b }

=> a b general y x
```

```
BEGIN { $; = ',' }
# ...
line.split # instead of line.split(',')
```

## Iterating over data

### `each`

```
(1..5).each { |i| puts i * i }
=> 1 4 9 16 25
```

```
require 'set'
Set['a', 'b', 'c'].each { |x| puts x }
=> a b c

Set[1, 2].reverse_each { |i| puts i * 2 }
=> 4 2
```

```
[3, 2, 5].each { |i| puts i * 2 }
=> 6 4 10

[3, 2, 5].each_index { |i| print i, ',' }
=> 0,1,2,

[3, 2, 5].reverse_each { |i| puts i * 2 }
=> 10 4 6
```

```
h = {'a' => 100, 'b' => 200}
h.each { |key, value| puts "key #{key} has value #{value}" }
=> key a has value 100
=> key b has value 200

h.each_key { |key| puts key }
=> a
=> b

h.each_value { |value| puts value }
=> 100
=> 200
```

```
{1 => 'a', 2 => 'b'}.reverse_each do |k, v|
	puts k * 2
end
=> 4
=> 2
```

### `step`

```
(0..9).step(2) do |i|
	puts "even number: #{i}"
end
```

```
('a'..'e').step(2) do |i|
	puts "letter: #{i}"
end
```

### `select`/`reject`

Both methods have in-place versions (`select!` and `reject!`), but the in-place versions are not available for range.

```
digits = 0..9
ret = digits.select { |i| i < 5 }
=> [0, 1, 2, 3, 4]
ret = digits.reject { |i| i < 5 }
=> [5, 6, 7, 8, 9]
```

```
a = [1, 2, 3, 4, 5]
b = a.select { |num| num.even? }
=> [2, 4]
c = a.select { |num| num.odd? }
=> [1, 3, 5]
```

```
h = {'a' => 100, 'b' => 200, 'c' => 300}
h.select { |k, v| k > 'a' }
=> {"b" => 200, "c" => 300}
h.select { |k, v| v < 200 }
=> {"a" => 100}
```

>   
> Note that for sets, the non-in-place versions return an array, not a set.
>   

```
require 'set'
s1 = Set[1, 2, 3, 4]
s2 = s1.select { |i| i.even? }
=> [2, 4]
s3 = s1.reject! { |i| i.even? }
=> #<Set: {1, 3}>
```

## `map`/`collect`

There are also in-place (`map!` or `collect!`) versions.  
The non-in-place versions return an array, even when applied to a hash or a set. The in-place version does not apply to a hash.

```
a = [3, 4, 5]
a.map { |i| i + 2 }
=> [5, 6, 7]
a.collect { |i| i + 2 }
=> [5, 6, 7]
```

```
require 'set'
s1 = Set[1, 2]
s2 = s1.map { |i| i * 2 }
=> [2, 4]
```

```
h = {'a' => 1, 'b' => 2}
h.map { |k, v| v + 3 }
=> [4, 5]
h.collect { |k, v| v + 3 }
=> [4, 5]
```

### `delete_if`/`keep_if`

Both methods work in-place (even though they don't have an `!` at the end).

```
a = [3, 4, 5, 8, 9]
a.keep_if { |i| i.even? }
=> [4, 8]
a.delete_if { |i| i.even? }
=> []
```

```
require 'set'
s1 = Set[3, 4, 5, 6, 7]
s1.delete_if { |i| i.even? }
=> #<Set: {3,5,7}>
```

```
h = {'a' => 100, 'b' => 200, 'c' => 300}
h.delete_if { |k, v| k > 'b' }
=> {"a" => 100, "b" => 200}
```

### `sort`

```
Set[4, 3, 5].sort { |a, b| b <=> a }
=> [5, 4, 3]

{1 => 'a', 3 => 'c', 2 => 'b'}.sort { |a, b| b <=> a }
=> [[3, "c"], [2, "b"], [1, "a"]]

[2, 6, 4, 5].sort { |a, b| b <=> a }
=> [6, 5, 4, 2]

# if no block is specified, the default sort order is followed
[2, 6, 4, 5].sort
=> [2, 4, 5, 6]
```
