# Ruby / strings

* [Manipulating strings](#manipulating-strings)
* [String formatting](#string-formatting)
* [String interpolation](#string-interpolation)
* [Chars sequence](#chars-sequence)
* [Converting numbers to a string](#converting-numbers-to-a-string)
* [puts vs. print](#puts-vs-print)

## Manipulating strings

### length/size

```
'hello'.length
=> 5
```

### empty?

```
'hello'.empty?
=> false
''.empty?
=> true
```

### strip (also: lstrip/rstrip)

```
'  hello  '.strip
=> "hello"
```

### Concatenation - `<<`

```
a = 'hello'
a << ' world'
=> "hello world"
```

```
'hello' + ' world'
=> "hello world"
```

### Comparison - `<=>`

Returns:
* `1` if left one is greater
* `-1` if right one is greater
* `0` if both equal

```
10 <=> 20
=> -1

'd' <=> 'c'
=> 1
```

### capitalize, downcase and upcase

```
'hello'.capitalize
=> "Hello"

'Hello'.downcase
=> "hello"

'Hello'.upcase
=> "HELLO"
```

### chars

```
'hello'.chars
=> ["h", "e", "l", "l", "o"]
```

### index

```
'hello'.index('l')
=> 2

'hello'.index('l', 3)
=> 3

'hello'.index('l', 4)
=> nil
```

### insert

```
'hello'.insert(2, '--')
=> "he--llo"
```

### delete

```
# delete any 'l'
'hello'.delete 'l'
=> "heo"

# delete any 'l' and 'o'
'hello'.delete 'lo'
=> "he"

# delete any of 'a', 'e', 'i', 'o', 'u' except 'e'
'hello'.delete 'aeiou', '^e'
=> "hell"

# delete any 'e' and any char from range k-m
'hello'.delete 'ek-m'
=> "ho"
```

### include?

```
'hello world'.include?('world')
=> true
```

### slice

```
# here: (start, length)
'hello'.slice(1, 3)
=> "ell"
```

### count

```
'hello'.count('l')
=> 2

'hello'.count('lo')
=> 3

'hello'.count('a-h')
=> 2

'hello'.count('^a-h')
=> 3
```
### partition

```
'hello'.partition('l')
=> ["he", "l", "lo"]
```

### tr

```
'hello'.tr('l', 'm')
=> "hemmo"

'hello'.tr('a-f', 'x')
=> "hxllo"
```

### reverse

```
'hello'.reverse
=> "olleh"
```

### sub/gsub

```
'hello'.sub('h', 'w')
=> "wello"

'hello'.sub('ll', 'x')
=> "hexo"

'hello'.sub('l', 'x')
=> "hexlo"

'hello'.gsub('l', 'x')
=> "hexxo"
```

### scan

```
'hello world'.scan(/[a-z]+/)
=> ["hello", "world"]

'hello world'.scan(/.../)
=> ["hel", "lo ", "wor"]
```

### split

```
'hello world'.split
=> ["hello", "world"]

'abc,def,ghi'.split(',')
=> ["abc", "def", "ghi"]

'abc,def,ghi'.split(',', 2)
=> ["abc", "def,ghi"]
```

## String formatting

```
x = '%05d' % 123
=> "00123"

y = '%.2f' % 34.5
=> "34.90"
```

## String interpolation

```
name = 'world'

puts "hello #{name}"
=> "hello world"

puts 'hello #{name}'
=> "hello #{name}"
```
### Interpolating expression

```
"The value is #{2 + 3}"
=> "The value is 5"
```

### Interpolating code

```
"Look: #{
	j = 0
	for i in 1..5
		j = j + i
	end
	j
}"
=> "Look: 15"
```

## Chars sequence

```
a = 'hello'
a[1]
=> "e"
```

## Converting numbers to a string

```
'12'.to_i
=> 12
'12'.to_f
=> 12.0

'12.5'.to_i
=> 12
'12.5'.to_f
=> 12.5

'abc'.to_i
=> 0
'abc'.to_f
=> 0.0
```

## puts vs. print

`puts` adds a newline character at the end of the string

```
puts 'a'; puts 'b'
> a
> b

print 'a'; print 'b'
> ab
```
