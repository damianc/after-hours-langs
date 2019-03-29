# Ruby / arrays

* [Creating arrays](#creating-arrays)
* [Accessing array elements](#accessing-array-elements)
* [Inserting an element at a certain position](#inserting-an-element-at-a-certain-position)
* [Multidimensional array](#multidimensional-array)
* [Working with arrays](#working-with-arrays)

## Creating arrays

```
a1 = []
a2 = Array.new
```

```
puts Array.new(2)
=> [nil, nil]
puts Array.new(2, 'a')
=> ["a", "a"]
```

```
puts Array.[](1, 2, 3, 4, 5)
=> [1, 2, 3, 4, 5]
puts Array[1, 2, 3, 4, 5]
=> [1, 2, 3, 4, 5]
puts Array(1..5)
=> [1, 2, 3, 4, 5]
```

## Accessing array elements

```
a1 = [1, 'cat', 2, 'dog', 3]
puts a1[0]
=> 1
puts a1[10]
=> nil
puts a1[-1]
=> 3

print a1[1,2]	# start, length
=> ["cat", 2]
print a1[1..3]
=> ["cat", 2, "dog"]
```

## Inserting an element at a certain position

```
a1 = [1, 'cat', 2, 'dog', 3]
a1[5] = 'tiger'
# now: [1, "cat", 2, "dog", 3, "tiger"]

a1[3] = 'wolf'
# now: [1, "cat", 2, "wolf", 3, "tiger"]

a1[2,2] = [4, 'bat']
# now: [1, "cat", 4, "bat", 3, "tiger"]

a1[2,2] = 'snake'
# now: [1, "cat", "snake", 3, "tiger"]

a1[2,1] = [5, 'lynx']
# now: [1, "cat", 5, "lynx", 3, "tiger"]

a1[-4..-3] = [2, 'dog']
# now: [1, "cat", 2, "dog", 3, "tiger"]
```

## Multidimensional array

```
a = [1, [2, 3]]
b = [[1, 2, 3], [4, 5, 6]]

puts a.flatten
=> [1, 2, 3]

puts b.transpose
=> [[1, 4], [2, 5], [3, 6]]
```

>   
> `flatten` has an in-place version - `flatten!`, which changes the original array rather than returning a new modified copy.
>   

## Working with arrays

### length/size

```
a = [2, 3, 5, 4, 1]
a.length # 5
a.size # 5
```

### empty?

```
[].empty? # true
[1, 2].empty? # false
```

### fill

```
a = Array.new(5)
a.fill('x')
=> ["x", "x", "x", "x", "x"]

a.fill('y', 2, 2)
=> ["x", "x", "y", "y", "x"]

a.fill('z', 1..2)
=> ["x", "z", "z", "y", "x"]
```

### concat with `+`

```
[1, 2, 3] + [4, 5]
=> [1, 2, 3, 4, 5]
```

### `[] - []`

```
[1, 2, 3, 4, 5] - [2, 3]
=> [1, 4, 5]

[1, 2, 2, 4, 5, 5] - [2, 3]
=> [1, 4, 5, 5]
```

### `[] * N`

```
[3, 4] * 3
=> [3, 4, 3, 4, 3, 4]
```

### `[] << X`

```
[1, 2] << 'a'
=> [1, 2, "a"]

[2, 3] << 'a' << 'b' << [4, 5]
=> [2, 3, "a", "b", [4, 5]]
```

### `[] == []` (or: `eql?`)

```
[1, 2] == [1, 2]	# true
[1, 2] == [2, 3]	# false
```

### `[] <=> []`

```
['a', 'a', 'b'] <=> ['a', 'b', 'c']
# -1

['a', 'a', 'b'] <=> ['a', 'b']
# -1

[4, 5, 6] <=> [2, 3]
# 1
```

### `|` (union), `&` (intersection), `uniq`

```
['a', 'a', 'b'] | ['b', 'c', 'c', 'd']
=> ["a", "b", "c", "d"]
```

```
['a', 'a', 'b', 'b', 'c'] & ['b', 'c', 'c', 'd']
=> ["b", "c"]
```

```
['a', 'a', 'b', 'b', 'c'].uniq
=> ["a", "b", "c"]

['a', 'a', 'b', 'b', 'c'] | []
=> ["a", "b", "c"]
```

### in-place operations

This is a common feature of Ruby (not just restricted to arrays).  
A function that has a `!` at the end usually denotes an in-place operation (meaning it changes the original array).

```
a = ['a', 'a', 'b', 'b', 'c']

b = a.uniq
=> ["a", "b", "c"]
# a = ["a", "a", "b", "b", "c"]

c = a.uniq!
=> ["a", "b", "c"]
# a = ["a", "b", "c"]
```
### `sort` (`sort!`)

```
[1, 3, 4, 2, 6, 3, 9, 5, 4].sort
=> [1, 2, 3, 3, 4, 4, 5, 6, 9]
```

### `reverse` (`reverse!`)

```
[1, 2, 3, 3, 4, 4, 5, 6, 9].reverse
=> [9, 6, 5, 4, 4, 3, 3, 2, 1]
```

### chaining methods

```
[1, 1, 2, 3, 3, 3].uniq.reverse
=> [3, 2, 1]
```

### `include?`

```
['a', 'b', 'c'].include?('d')
=> false

['a', 'b', 'c'].include?('c')
=> true
```

### `index` / `rindex`

```
['a', 'b', 'b', 'c', 'd'].index('c') # 3
['a', 'b', 'b', 'c', 'd'].index('z') # nil

['a', 'b', 'b', 'c', 'd'].index('b') # 1
['a', 'b', 'b', 'c', 'd'].rindex('b') # 2
```

### `values_at`

```
['a', 'b', 'd', 'f', 'j', 'l', 'm'].values_at(0, 2, 5)
=> ["a", "d", "l"]
```

### `fetch`

```
a1 = [1, 'cat', 2, 'dog', 3]
a1.fetch(0) # 1
a1.fetch(6) # IndexError
a1.fetch(6, 'Not found') # "Not found"
```

### `insert`

```
a = ['a', 'b', 'c', 'd']
a.insert(2, 5)
=> ["a", "b", 5, "c", "d"]

a.insert(-2, 6)
=> ["a", "b", 5, "c", 6, "d"]

a.insert(4, 'e', 'f')
=> ["a", "b", 5, "c", "e", "f", 6, "d"]
```

### `delete`

```
a = ['a', 'b', 'b', c', 'd']
a.delete('b')
=> "b"
print a
=> ["a", "c", "d"]

a.delete('z')
# nil

a.delete('z') { 'Value not in array' }
# "Value not in array"
```

### `delete_at`

```
a = ['a', 'b', 'c', 'd', 'e']
a.delete_at(2)
=> "c"

print a
=> ["a", "b", "d", "e"]
```

### `join`

```
['n', 'o', 't', 'e'].join
=> "note"

['abra', 'ca', 'dabra'].join('-')
=> "abra-ca-dabra"
```

### `compact`

```
['a', nil, nil, 'b', nil, 'c'].compact
=> ["a", "b", "c"]
```

### `clear`

```
a = ['x', 'y', 'z']
a.clear
=> []
```
