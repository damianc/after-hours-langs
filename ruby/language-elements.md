# Ruby / Language elements

* [Comments](#comments)
* [Numbers](#numbers)
* [Comparison operators](#comparison-operators)
* [Assignment operators](#assignment-operators)
* [Bitwise operators](#bitwise-operators)
* [Logical operators](#logical-operators)
* [Ternary operator](#ternary-operator)
* [The `range` operators](#the-range-operators)
* [`defined?` operator](#defined-operator)
* [`::` operator](#-operator)
* [Pattern matching operators](#pattern-matching-operators)
* [Ranges](#ranges)
* [Conditional constructs / control flow](#conditional-constructs--control-flow)
* [Handling exceptions](#handling-exceptions)
* [Predefined variables and constants](#predefined-variables-and-constants)
* [Running OS commands](#running-os-commands)
* [Defining functions](#defining-functions)

## Comments

```
# this is a comment

=begin
this is a
multiline comment
=end
```

## Numbers

| Symbol | Example |
|--------|---------|
| + | 5 + 2 => 7 <br/>5 + 2.0 => 7.0 |
| - | 5 - 2 => 3 |
| * | 5 * 2 => 10 |
| / | 5 / 2 => 2  <br/>5 / 2.0 => 2.5 |
| % | 5 % 2 => 1 |
| ** | 5 ** 2 => 25 |

| Function | Example |
|----------|---------|
| abs | -3.abs => 3  <br/>3.abs => 3 |
| div | 10.div(2) => 5 |
| zero? | 10.zero? => false |
| ceil | 2.6.ceil => 3 |
| floor | 2.6.floor => 2 |
| round | 2.635.round(2) => 2.64 |

## Comparison operators

* `==`
* `!=`
* `>`
* `<`
* `>=`
* `<=`

### General comparison operator

```
3. <=> 5
=> -1

5 <=> 3
=> 1

'abc' <=> 'abc'
=> 0
```
### Reference and value equality

* `eql?` - the same type and equal value
* `equal?` - the same object id

```
a = 'ed'
b = 'ed'

puts a.eql?(b)
=> true

puts a.equal?(b)
=> false
```

### Checking if an object is `nil`

```
a = [1]
a[1].nil?
=> true
a[0].nil?
=> false
```

## Assignment operators

* `=`
* `+=`
* `-=`
* `*=`
* `/=`
* `%=`
* `**=`

### Mass assignment

```
a, b, c, d = 1, 2, 3, 4

a, b, c = 1, 2, 3, 4
# 4 is ignored

a, b, c, d = 1, 2, 3
# d will be nil
```

## Bitwise operators

| Operator | Description |
|----------|-------------|
| `&` | AND |
| `|` | OR |
| `^` | XOR |
| `~` | ones complement |
| `<<` | left-shift |
| `>>` | right-shift |

## Logical operators

* `&&`
* `||`
* `!`

* `and`
* `or`
* `not`

```
true && 5
=> 5

false && 5
=> false

true || 5
=> true

false || 5
=> 5
```

## Ternary operator

```
x = 3
y = (x > 0) ? 'positive' : 'non-positive'
puts y
=> "positive"
```

## The range operators

* `1..5` - range from 1 to 5
* `1...5` - range from 1 to 4

## `defined?` operator

```
a = 1
defined? a
=> "local-variable"

defined? b
=> nil

defined? 3
=> "expression"
```

## `::` operator

It is used to access:
* constants
* instance methods
* class methods accessed outside of the class or module in which it is defined

## Pattern matching operators

* `=~` - has a match
* `!~` - has no match

```
'hello' =~ /el/
=> 1

'hello' =~ /xx/
=> nil

'hello' !~ /lo/
=> false
```

## Ranges

A range can be used in Ruby as *intervals*, *sequences* or *conditions*.

### Interval

```
print (1..9) === 5
=> true
# 5 present in 1-9

print (1...5) === 5
=> false
# 5 not present in 1-4
```

### Sequences

```
a = 1..5
print a
=> 1..5
```

```
arr = (1..5).to_a
print arr
=> [1, 2, 3, 4, 5]

arr = ('a'..'e').to_a
print arr
=> ["a", "b", "c", "d", "e"]

arr = ('car'...'cat').to_a
print arr
=> ["car", "cas", "cat"]
```

```
digits = 0..9

print digits.min
=> 0

print digits.max
=> 9

print digits.include?(5)
=> true
```

### Conditions

Ranges can be used as conditional expressions in various control flow structures, e.g. `case` statement.

```
marks = 65
remark = case marks
		when 0..49 then 'below average'
		when 51..100 then 'above average'
		else 'average'
end

print remark
=> "above average"
```

## Conditional constructs / control flow

### `if` block

```
a = 0
if a == 0
	print 'zero'
elsif a > 0
	print 'positive'
else
	print 'negative'
end
```

#### inline `if`

```
a = 5
if a > 0 then print 'positive' end
```

#### inline reversed `if`

```
a = 1
print 'positive' if a > 0
```

### `nil` check

```
a = 'abc' =~ /d/
print a.nil?
=> true
```

### `unless`

```
a = -5
unless a >= 0
	print 'negative'
else
	print 'positive or zero'
end
```

### `case`

```
a = 7
status = case
	when a % 6 != 0 then 'not divisible by 6'
	when a % 3 != 0 then 'not divisible by 3'
	when a % 2 != 0 then 'not divisible by 2'
	else 'odd'
end

print status
=> "not divisible by 6"
```

```
a = 7
status = case
	when a > 6
		'more than 6'
	when a > 4
		'more than 4'
	else 'less than 5'
end

print status
=> "more than 6"
```

```
a = 6
status = case a
	when 1..4, 6...10 then 'not equal to 5'
	when 5 then 'equal to 5'
	else 'above 9'
end

print status
=> 'not equal to 5'
```

#### Case forms

```
# form 1
[variable =] case
	when condition
		statements
	when condition
		statements
	[else
		statements]
end
```

```
# form 2
[variable =] case expression
	when nil
		statements
	when Type1[, Type2]
		statements
	when value1[, value2]
		statements
	when /regexp1/[, /regexp2/]
		statements
	when min1..max1[, min2..max2]
		statements
	[else
		statements]
end
```

### while

```
a = 0
while a < 5
	a = a + 1
	print a
end
=> 12345
```
### break / redo / next

```
a = 0
while a < 4
	a = a + 1
	break if a > 2
	print a
end
=> 12
```

```
a = 0
while a < 4
	a = a + 1
	next if a > 2
	print a
end
print ' after while'
=> 12 after while
```

```
a = 0
while a < 4
	a = a + 1
	redo if a == 2
	print a
end
print ' after while'
=> 134 after while
```

When `a` became `2`, `redo` sent the control back to the beginning statement in the loop (`a = a + 1`).
The statement after `redo` (the `print`) was not executed for that iteration.

### until

```
a = 0
until a > 3
	a = a + 1
	print a
end
=> 1234
```

>
> `until` has a similar relation to `while` as `unless` to `if`
>

### for

```
for a in 1...3
	print a
end
=> 12
```

```
for b in 'w'..'z'
	print b
end
=> wxyz
```

## Handling exceptions

### `rescue`/`ensure`

```
f = File.open('input.txt')
begin
	# code for processing
rescue
	# code for handling errors
ensure
	f.close unless f.nil?
end
```

### raising an exception

```
raise
raise 'no file found'
raise ArgumentError, 'Too big name', caller
```

### Single line `rescue`

```
a = [1, 2]
a[2] * 2 rescue 'No such element'
=> "No such element"

a[1] * 1 rescue 'No such element'
=> 4
```

### `catch` and `throw`

```
catch (:testit) do
	i = 0
	while i < 5
		i = i + 1
		j = 0
		while j < 5
			j = j + 1
			print i.to_s + ',' + j.to_s + ': '
			throw :testit if i * j > 5
		end
	end
end

=> 1,1: 1,2: 1,3: 1,4: 1,5: 2,1: 2,2: 2,3:
```

`catch` defines a block with a label that executes until a `throw` is encountered.
If `throw` is called with an option second parameter, that value is returned as the value of the catch.  
  
`catch` and `throw` come in handy for arbitrarily jumping out of many levels of nesting and so on.

## Predefined variables and constants

```
'abracadabra'.match(/rac/)
puts $&
puts $`
puts $'

=> rac
=> ab
=> adabra
```

### `$;` - default separator of string#split

```
$; = ','
puts 'a,b,c'.split
=> a
=> b
=> c
```

### `$*` - synonym for `argv`

#### Processing command-line arguments

```
# argvhello.rb file
first_name = ARGV[0]
last_name = ARGV[1]
puts "Hello #{first_name} #{last_name}."
```

```
=> ruby argvhello.rb John Doe
Hello John Doe.
```

### Predefined constants

One such os `ARGV`.  
These include `STDIN`, `STDOUT`, `STDERR`, `RUBY_VERSION` and `ENV`.

## Running OS commands

```
val = `ls *.txt`
print val
=> coond.txt
=> inplines.txt
```

## Defining functions

```
def say_hello
	print 'hello world'
end

say_hello
=> "hello world"
```

### Arguments

```
def sum_of (a, b)
	c = a + b
end

d = sum_of 2, 2
print d
=> 4
```

### Arguments with default values

```
def sum_of (a = 2, b = 3)
	c = a + b
end
```

### Variable number of arguments

```
def count_of (*numbers)
	c = numbers.length
end

print count_of 1, 3, 5
=> 3
```

### Returning value

```
return
# nil

return 1
# 1

return 1, 3, 4
# [1, 3, 4]

return 2 + 5
# 7
```

Every method in Ruby returns a value by default (even when explicit return is not specified).
If a `return` statement is not specified, the value returned is the value of the last statement.

```
def seeit
	a = 0
	b = a + 1
	'xy' + 'z'
end

str = seeit
puts str
=> "xyz"
```

### Recurrence

```
def factorial(n)
	if n <= 1
		1
	else
		n * factorial(n - 1)
	end
end

puts factorial(3)
=> 6
```
