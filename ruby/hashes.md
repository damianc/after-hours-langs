# Ruby / hashes

* [Creating hashes](#creating-hashes)
* [Adding new elements to a hash](#adding-new-elements-to-a-hash)
* [Working with hashes](#working-with-hashes)

## Creating hashes

```
h1 = Hash.new
h2 = Hash['a' => 100, 'b' => 200]
h2['a']
=> 100
```

```
h = {'a' => 1, 'b' => 2}
puts h
=> {"a" => 1, "b" => 2}
```

### Hash with a default value

```
h['c']
=> nil

h = Hash.new ('unknown')
h['c']
=> "unknown"
```

## Adding new elements to a hash

```
h = Hash.new ('unknown')
h['AUS'] = 'Canberra'
h.length # 1
h.size # 1
h.default # "unknown"
```

```
h.default = 'ABCD'
h['x']
=> "ABCD"
```

```
h.keys # ["AUS"]
h.values # ["Canberra"]
```

## Working with hashes

### `clear`

```
h = {1 => 'a', 2 => 'b'}
h.clear
=> {}
```

### `empty?`

```
h.empty?
=> true
```

### `has_key?`

```
h = {1 => 'a', 2 => 'b'}
h.has_key?(1) # true
h.has_key?(3) # false
```

### `has_value?` (`value?`)

```
h.has_value?('b') # true
h.has_value?('c') # false
```

### `key`

```
{1 => 'a', 2 => 'b'}.key('b')
=> 2
{1 => 'a', 2 => 'b'}.key('c')
=> nil
```

### `fetch`

```
{1 => 'a', 2 => 'b'}.fetch(2, 'invalid key')
=> "b"

{1 => 'a', 2 => 'b'}.fetch(3, 'invalid key')
=> "invalid key"

{1 => 'a', 2 => 'b'}.fetch(3)
=> Error
```

### `values_at`

```
h = {1 => 'a', 2 => 'b', 3 => 'c', 4 => 'd'}
h.values_at(1, 4)
=> ["a", "d"]

h.values_at(2, 5)
=> ["b", nil]

h.default = 'x'
h.values_at(2, 5, 8)
=> ["b", "x", "x"]
```

### `delete`

```
{1 => 'a', 2 => 'b'}.delete(1)
=> "a"
{1 => 'a', 2 => 'b'}.delete(3)
=> nil
```

### `invert`

```
{1 => 'a', 2 => 'b'}.invert
=> {"a" => 1, "b" => 2}
```

### `to_a`

```
{1 => 'a', 2 => 'b'}.to_a
=> [[1, "a"], [2, "b"]]
```

### `{} == {}`

```
{1 => 'a', 2 => 'b'} == {1 => 'a', 3 => 'c'}
=> false

{1 => 'a', 2 => 'b'} == {2 => 'b', 1 => 'a'}
=> true
```

### `merge` (`merge!`)

```
h1 = {1 => 'a', 2 => 'b'}
h2 = {1 => 'd', 3 => 'c'}
h1.merge(h2)
=> {1 => "d", 2 => "b", 3 => "c"}
```
