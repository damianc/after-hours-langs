# Ruby / input-output

* [Structures (`Struct`)](#structures-struct)
* [Working with directories](#working-with-directories)
* [Dividing files into subdirectories](#dividing-files-into-subdirectories)

## Structures (`Struct`)

```
# a)
Struct.new('Customer', :name, :addr, :tel)

# b)
Customer = Struct.new(:name, :addr, :tel)
```

```
john = Customer.new('John Connor', '123 Rachel Close', 3456)
john.name
=> "John Connor"
john.tel = 1111
john.tel
=> 1111
```

### Structure with methods

```
Customer = Struct.new(:name, :addr, :tel) do
	def greeting
		puts "Hello #{name}!"
	end
end

john = Customer.new('John Connor', '123 Rachel Close', 3456)
john.greeting
=> "Hello John Connor!"
```

### Accessing struct fields

```
john.name
john['name']
john[:name]
john[0]
```

```
john.each_pair do |name, val|
	puts("#{name} => #{val}")
end
=> name => John Connor
=> addr => 123 Rachel Close
=> tel => 3456
```

### Iterating structs

```
Customer = Struct.new(:name, :addr, :tel)
cust = []

cust[0] = Customer.new('John Connor', '123  Rachel Close', 3456)
cust[1] = Customer.new('Jane Greystoke', '12 Jungle House', 4568)
cust[2] = Customer.new('Sarah Turnbull', '50 Sunset Boulevard', 1254)

cust.each { |c| puts c.name }
=> John Connor
=> Jane Greystoke
=> Sarah Turnbull
```

### Structs comparison

```
Customer = Struct.new(:name, :addr, :tel)
cust = []

cust[0] = Customer.new('John Connor', '123  Rachel Close', 3456)
joe = Customer.new('Joe Smith', '123 Maple St', 12345)
j2 = Customer.new('John Connor', '123 Rachel Close', 3456)

puts j2 == joe # false
puts j2 == cust[0] # true
```

## Working with directories

### `mkdir`

```
# new directory under the current working directory
Dir.mkdir('test1')
Dir.mkdir('test2', 777)
```

### `rmdir`

Removes the named directory, if empty.

```
Dir.rmdir('/tmp/tst') # absolute path
Dir.rmdir('test2') # relative path
```

### `pwd`

```
currDir = Dir.pwd
puts currDir
=> /Users/Shared/chap02
```

### `chdir`

Changes the directory programmatically. Once changed, the new directory becomes current in an execution context.

```
Dir.chdir('test1')
puts Dir.pwd
=> /Users/Shared/chap06/test1
```

>   
> Without an argument, it changes to the `HOME` directory (the `HOME` variable should be set in environment).
>   

```
puts Dir.pwd
Dir.chdir('test1') {
	puts Dir.pwd
	2 + 2
}
puts Dir.pwd

=> /Users/Shared/chap06
=> /Users/Shared/chap06/test1
=> /Users/Shared/chap06
```

### `home`

```
Dir.home # current user's home directory
Dir.home('root') # root's home directory
```

### `exist?`

```
Dir.exist?('test2')
# checks if 'test2' directory exists directly under the current directory
```

### `entries`

```
Dir.entries('.')
=> [".", "..", "test1", "test2"]
Dir.entries('test1')
=> [".", "..", "x.txt", "y.txt"]
```

### `each`

```
dir = Dir.new('test1')
print dir.entries
puts
dir.each { |x| puts 'Got ' + x }
dir.close

[".", "..", "test3", "x.txt", "y.txt"]
Got .
Got ..
Got test3
Got x.txt
Got y.txt
```

>   
> The `new` function returns a new directory object for the named directory. This can be used as a handle for further action. It can use the `close` function on this handle (directory stream) to close it after the job is done.
>   

### `foreach`

```
Dir.foreach('test1') { |x| puts "Name: #{x}" }

Name: .
Name: ..
Name: test3
Name: x.txt
Name: y.txt
```

### `glob`

```
Dir.glob('*')
=> ["chdir.rb", "each.rb", "foreach.rb", "test1", "test2"]

Dir.glob('*.rb')
=> ["chdir.rb", "each.rb", "foreach.rb"]
```

| pattern/regexp | files/directories affected |
|----------------|----------------------------|
| `**/*.rb` | any `.rb` file in any subdirectory under the current directory |
| `**/test3/*.rb` | any `.rb` file in a subdirectory with a particular name |
| `**/test1/**/*.rb` | any `.rb` file at any sublevel of any directory named `test1` |
| `t*` | any files/directories that start with `t` |
| `*each*` | any file/directory that has `each` in it |
| `*.{rb,txt}` | all files or directories which have either extension `.rb` or `.txt` |
| `*.[^r]*` | all files or directories that have an extension whose first character is not `r` |

The default file separator may vary based on the operating system, so you can use `File.join` to build up the path, instead of a direct string.

```
path = File.join('**', '*.rb')
Dir.glob(path)
=> ["chdir.rb", "each.rb", "foreach.rb", "test1/test3/z.rb", "test1/x.rb", "test1/y.rb"]
```

## Dividing files into subdirectories

```
require 'fileutils'
Dir.mkdir('tbl') unless File.directory?('tbl')
Dir.mkdir('proc') unless File.directory?('proc')

arr = Dir.glob('*.sql')
arr.each { |filename|
	infile = File.open(filename, 'r')
	firstline = infile.gets
	infile.close

	FileUtils.mv(filename, 'tbl') if firstline =~ /table/
	FileUtils.mv(filename, 'proc') if firstline =~ /procedure/
}
```
