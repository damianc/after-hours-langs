# Ruby / Regular expressions

* [Searching within a file](#searching-within-a-file)
* [Finding only the matched string](#finding-only-the-matched-string)
* [Common character classes](#common-character-classes)
* [Predefined character classes](#predefined-character-classes)
* [Finding significant positions in a string](#finding-significant-positions-in-a-string)
* [Word boundary and non-word boundary](#word-boundary-and-non-word-boundary)
* [Using non-capturing groups](#using-non-capturing-groups)
* [Finding repeated patterns](#finding-repeated-patterns)
* [[Negative] Lookahead/Lookbehind assertion](#negative-lookaheadlookbehind-assertion)
* [Comments in a regular expression](#comments-in-a-regular-expression)
* [`\i` and `\m` modifiers](#i-and-m-modifiers)
* [Non-backtracking groups](#non-backtracking-groups)
* [`gsub()` with block structure](#gsub-with-block-structure)
* [Using the `scan()` function with regular expressions](#using-the-scan-function-with-regular-expressions)

## Searching within a file

```
infile = File.open 'names.txt', 'r'
while line = infile.gets
	puts line if line.match(/^[N-Z]/)
end
infile.close
```

## Finding only the matched string

```
# a)
infile = File.open 'names.txt', 'r'
while line = infile.gets
	if matched = line.match(/(^[N-Z])/)
		retarr = matched.captures
		puts retarr[0]
	end
end
infile.close
=> R
=> R
=> S
=> R
```

```
# b)
infile = File.open 'names.txt', 'r'
while line = infile.gets
	matched = line.match(/(^[N-Z])/)
	puts matched.captures if matched
end
infile.close
```

## Common character classes

| construct | meaning |
|-----------|---------|
| `[ABC]` | A, B or C |
| `[^ABC]` | any character except A, B or C |
| `[A-Z]` | any character from A to Z |
| `[A-Za-z]` | any character from A to Z or from a to z |
| `[A-P[N-S]]` | union of characters from A-P and N-S |
| `[A-Z&&[DEF]]` | intersection of characters from A-Z and D-F (only D, E and F) |
| `[A-Z&&[^D-F]]` | any characters from A to Z except any characters from D to F |

## Predefined character classes

* `.` - any character
* `\d` - a digit `[0-9]`
* `\D` - a non-digit `[^0-9]`
* `\s` - a whitespace character
* `\S` - a non-whitespace character
* `\w` - a word character `[A-Za-z0-9_]`
* `\W` - a non-word character

## Finding significant positions in a string

* `^` - start of string or line
* `$` - end of string or line
* `\A` - start of string only
* `\Z` - end of string but for the final terminator, if any
* `\z` - end of stirng only
* `\B` - non-word boundary
* `\b` - word boundary

## Word boundary and non-word boundary

A word boundary (`\b`) can match a zero-width place that can match:
* between a word character (`\w`) and a non-word character (`\W`)
* between a word character and the start or end of the string

```
|bread|, |and| |jam|.
```

A non-word boundary (`\B`) can match a zero-width place that is:
* between two word characters
* between two non-word characters
* between a non-word character and the start or end of the string
* the empty string

```
b|r|e|a|d,| a|n|d j|a|m.|
```

### Looking for multiple groups

```
print 'iteration'.match(/t(...)t(...)/).captures
=> ["era", "ion"]
```

## Using non-capturing groups

```
print 'white and black'.match(/(wh(eat|ite))/).captures
=> ["white", "ite"]

print 'white and black'.match(/(wh(?:eat|ite))/).captures
=> ["white"]

print 'white, black, or yellowish'.match(
	/(white)(?:.*)(yellow).*/
).captures
=> ["white", "yellow"]
```

## Finding repeated patterns

```
print 'matched' if 'ab1ab2ab'.match(/(ab|cd|ef).\1.\1/)
=> matched
print 'matched' if 'ab1cd2cd'.match(/(ab|cd|ef).\1.\1/)
=>
```

### Octal codes and backreferences

```
print "\121"
=> Q
```

* The expressions `\1` through `\9` are always interpreted as backreferences and not as octal codes.
* If the first digit of a multi-digit expression starts with 8 or 9, the expression is interpreted as a literal.

```
print "\89"
=> 89
```

* Expressions from `\10` onward (except the 8 and 9 beginning digits) are considered backreferences if there is a backreference corresponding to that number; otherwise, they are treated as octal codes.

### Named backreferences

```
'cd1abcab3cd'.match(/(?<Hansel>cd).(?<Gretel>ab).\k<Greter>.\k<Hansel>/)
```

```
'cd1cd2cd'.match(/(ab|cd|ef).\1.\1/)
# =
'cd1cd2cd'.match(/(?<x>ab|cd|ef).\k<x>.\k<x>/)
```

## [Negative] Lookahead/Lookbehind assertion

```
# LA
print 'aqua'.match(/q(?=u)/)
=> q

# NLA
print 'aqua'.match(/q(?!u)/) #
print 'aqa'.match(/q(?!u)/) # q

# LB
print 'auqa'.match(/(?<=u)q/) # q
print 'aqa'.match(/(?<=u)q/) #

# NLB
print 'auqa'.match(/(?<!u)q/) #
print 'aqa'.match(/(?<!u)q/) # q
```

## Comments in a regular expression

```
print 'test this'.match(/te(?#this is comment)/)
=> te
```

Another way uses free-spacing mode. In this mode, a pattern is followed by an `x` modifier and can be spread over multiple lines. The comment (at any line) starts at `#` and goes to the end of the line.

```
print 'ababcabcd'.match(/ab # the characters a and b
		c # followed by c
		a # and then another a/x)
=> abca
```

## `\i` and `\m` modifiers

```
print 'ababcabcd'.match(/AB/) #
print 'ababcabcd'.match(/AB/i) # ab
```

Ignoring case behavior can be used on a subpattern, in which case it can be specified with a construct such as `/R(?i)uby/` or `/R(?i:uby)/` - both of which match the word *Ruby* with *uby* in any case.

```
print 'ababcabcd'.match(/a(?i)BC/)
=> abc

# =

print 'ababcabcd'.match(/a(?i:BC)/)
=? abc
```

The multiline modifier (`m`) in Ruby is not really a multiline modifier by general standards. It simply means dot (`.`) will match newlines also.

```
print "ababcabcd\n".match(/cd./) #
print "ababcabcd\n".match(/cd./m) # cd
```

## Non-backtracking groups

An atomic group fails fast. If one of the alternatives for such a group has matched a part of the source string and the subsequent subpattern (outside the group) does not match, it gives up without trying any other alternatives.

```
'armchair'.match(/\b(arm|armchair|armour)\b/)
=> armchair
'armchair'.match(/\b(?>arm|armchair|armour)\b/)
=>
```

After matching the leftmost alternative - `arm`, with that part in the source string, the group exits and the subsequent `\b` fails to match.

```
print 'ababcabcd'.match(/(a(bc|b)c)/)
=> abc
print 'ababcabcd'.match(/(a(?>bc|b)c)/)
=>
```

## `gsub()` with block structure

```
print '12 10 16'.gsub(/(\d+)/) { |m| m.to_i * 2 }
=> 24 20 32
```

## Using the `scan()` function with regular expressions

```
str = 'this is the theatre'
rslt = str.scan(/th/)
puts rslt.inspect
=> ["th", "th", "th"]
```
