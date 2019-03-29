# Ruby/files

* [Reading from a file](#reading-from-a-file)
* [Writing to a file](#writing-to-a-file)
* [Reading multiple lines from a file](#reading-multiple-lines-from-a-file)
* [Exception handling](#exception-handling)
* [Creating and deleting directories](#creating-and-deleting-directories)

## Reading from a file

```
infile = File.open('input.txt', 'r')
myword = infile.gets
puts myword
=> welcome
```

### Reading a file in a single string

```
text = File.read 'inplines.txt'
puts text
```

## Writing to a file

```
outfile = File.open('output.txt', 'w')
myword = 'welcome'
outfile.puts myword
outfile.close
```

## Reading multiple lines from a file

```
infile = File.open('inplines.txt', 'r')
while (line = infile.gets)
	puts line
end
infile.close
```

```
# ()-less version

infile = File.open 'inplines.txt', 'r'
while line = infile.gets
	puts line
end
infile.close
```

```
# if there is only one statement inside the `while` - it can be inline

infile = File.open 'inplines.txt', 'r'
puts line while line = infile.gets
infile.close
```

## Exception handling

```
begin
	infile = File.open('input.txt', 'r')
	myword = infile.gets
	puts myword
	infile.close
rescue
	puts 'Could not find file input.txt'
end
```

## Creating and deleting directories

```
requires 'fileutils'
FileUtils.mkdir('dir')
FileUtils.rm_rf('dir')
```

### Creating a whole directory path

```
require 'fileutils'
FileUtils.mkdir_p('abc/def/ghi')
```
