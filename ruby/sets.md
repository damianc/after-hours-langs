# Ruby / sets

* [Creating sets](#creating-sets)
* [Checking and changing](#checking-and-changing)
* [Set operations](#set-operations)
* [Flattening and conversion](#flattening-and-conversion)

## Creating sets

```
require 'set'

s1 = Set.new
s1.add(1)
s1.add('a')

s2 = Set.new [1, 2, 'c'] # ...new<space>[...
s3 = [1, 2, 'd'].to_set
s4 = Set[1, 2]
```

## Checking and changing

```
s = Set[1, 2]
s.length # 2
s.size # 2
```

### `empty?`

```
s.empty?
=> false
```

### `include?`

```
s.include?(1)
=> true
```

### `clear`

```
s.clear
=> #<Set: {}>
```

### `<<` (`add`)

```
s << a
=> #<Set: {"a"}>
```

### `merge`

```
s.merge(['b', 'c', 'd', 'e', 'f'])
=> #<Set: {"a", "b", "c", "d", "e", "f"}>
```

### `delete`

```
s.delete('a')
=> #<Set: {"b", "c", "d", "e", "f"}>
```

### `subtract`

```
s.subtract(['c', 'd'])
=> #<Set: {"b", "e", "f"}>
```

### `==`

```
s2 = Set[2, 3]
s3 = Set[3, 2]
s2 == s3
=> true
```

## Set operations

### `+`  (`|`, `union`)

```
s1 = Set[1, 2, 3, 4]
s2 = Set[3, 4, 5, 6]
s1 + s2
# {1, 2, 3, 4, 5, 6}
```

### `&` (`intersection`)

```
s1 & s2
# {3, 4}
```

### `intersect?`

```
s1.intersect?(s2)
=> true
```

### `disjoint?`

```
s1.disjoint?(s2)
=> false
```

>   
> It is essentially the exact opposite of `intersect?`.
>   

### `-` (`difference`)

```
s1 - s2
# {1, 2}
# present in 1st but not in 2nd set
```

### `^` (not common)

```
s1 ^ s2
=> {5, 6, 1, 2}
```

### `subset` and `superset`

```
s1 = Set[1, 2, 3]
s2 = Set[1, 2]

s1.superset?(s2) # true
s1 >= s2 # true

s1 >= s1 # true

# a proper superset of a set should be a superset of the set,
# but should have at least one more element
s1.proper_superset?(s2) # true
s1 > s2 # true
s1 > s1 # false

s2.proper_subset?(s1) # true
s2.subset(s1) # true

s1 <= s1 # true
s2 <= s1 # true
s2 < s1 # true
s1 < s1 # false
```

## Flattening and conversion

```
s = Set[Set[1,2], Set[3,4], Set[2,3], Set[4,5]]
s.flatten!
=> #<Set: {1, 2, 3, 4, 5}>
```

```
Set['a', 'c', 'b', 'e', 'd'].to_a
=> ["a", "c", "b", "e", "d"]
```
