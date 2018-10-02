---
title: Ruby 101
type: lesson
duration: "2:30"
creator:
    name: Ari Brenner
    city: NY
competencies: Programming
---

# Ruby 101

### Objectives
*After this lesson, students will be able to:*
- Be familiar with basic Ruby data types
- Know how to compare and mutate Ruby objects
- Understand basic iterators and control in Ruby
- Compare/contrast Ruby objects/methods to Javascript objects/functions

### Preparation
*Before this lesson, students should:*
- Be comfortable with Javascript
- Have Ruby installed


## What is Ruby?

[Ruby](https://www.ruby-lang.org/en/about/) is a high-level  programming language that gives us a lot of nice features out of the box.  It is super readable.

Ruby does **not** run in the browser.  It is most commonly used for backend web development with frameworks like [Sinatra](http://www.sinatrarb.com/) and [Rails](http://rubyonrails.org/).

## Running Ruby

Just run `ruby FILENAME.rb`.  You can also run [`irb`](https://www.tutorialspoint.com/ruby/interactive_ruby.htm) this is the Ruby `repl`.

> Also check out [`pry`](http://pryrepl.org/)! It is a new and improved Ruby `repl`

## Numbers

Numbers in Ruby are in the [`Numeric`](https://ruby-doc.org/core-2.1.0/Numeric.html) class.  It's subclasses include [`Fixnum`](https://ruby-doc.org/core-2.1.0/Fixnum.html) and [`Float`](https://ruby-doc.org/core-2.1.0/Float.html)

```ruby
1.class # => Fixnum
1.0.class # => Float

2 - 3 # => -1
2 * 3 # => 6
1 + 1 # => 2
1 / 2 # => 0 (rounds down)
1 / 2.0 #=> 0.5 (does not round float)
```

To divide two integers together, convert them to `Float`s first:

```ruby
1.to_f / 2 # => 0.5
```

FixNums have some nice methods, like

```ruby
1.odd? # => true
2.even? # => false
```

Methods that return a boolean often end in a question mark `?` to make the code readable.

## Strings

A [`String`](https://ruby-doc.org/core-2.1.0/String.html) in Ruby is similar to strings in JS.

```ruby
'foo'.length # => 3
'foo'.include?('o') # => true
```

However, strings are **mutable**

```ruby
str = 'foo'
str.upcase # => 'FOO'

str # => 'foo' (upcase does not mutate str)
str.upcase! # => 'FOO'
str # => 'FOO' (upcase! DOES mutate str)
```

Any method you see that ends in a bang (`!`) is Ruby telling you something unexpected or dangerous results when calling it. Mutating a value directly is unexpected.

### Interpolation

To interpolate strings in Ruby, you must use double quotes

```ruby
"I have #{13 * 4} cards" # => "I have 52 cards"
'I have #{13 * 4} cards' # => 'I have #{13 * 4} cards'
```

### Concatenation

You can also concatenate strings but this is NOT recommend

```ruby
'foo' + 'bar' # => 'foobar'
'foo' + 2 # => TypeError: no implicit conversion of Fixnum into String
'foo' + 2.to_s # => 'foo2'
```

There is very little implicit conversion in Ruby! When dealing with types of different types, convert them to types that work together first.

Use single quotes for strings that are not interpolated

## Symbols

A [Symbol](https://ruby-doc.org/core-2.2.0/Symbol.html) is similar to a `String`.  It cannot be mutated or manipulated.  It is used represent _things_ rather than _text_

```ruby
:foo # => :foo
:foo == :foo # => true
:foo == :bar # => false
:foo == 'foo' # false
```

The more you see them the more you will understand the use-case

## Booleans and `nil`

Of course Ruby has two booleans `true` and `false`

Each object has a `==` method that compares to another object.

```ruby
1 == 1 # => true
1 == '1' # => false
1 == 1.0 # => true
[:foo, :bar] == [:foo, :bar] # => true
[:foo, :bar] == [:bar, :foo] # => false
{a: 1, b: 2} == {b: 2, a: 1} # => true
```

> Do NOT use `===`. This is not the same as what it means in JS.  (Look it up if you are curious)

Ruby also has `nil`.  This is similar to `null` or `undefined` in JS. (There is no distinction in Ruby)

### Truthy and Falsy

Ruby only has **two** falsy values: `nil` and `false`.

So unlike JS `0` and `''` are truthy.  (There is no `null`, `undefined`, `NaN`, `-0`)

```ruby
!! false # => false
!! nil # => false

!! 0 # => true
!! '' # => true
```

## Arrays

A Ruby [`Array`](https://ruby-doc.org/core-2.2.0/Array.html) is similar to a JS Array.

```ruby
arr = [:stacey, :tracey, :lacey, :macey]
arr.length # => 3
arr[0] # => :stacey
arr[100] # => nil
arr.include?(:tracey) # => true

arr.push(:jackie)
arr # => [:jackie] # => [:stacey, :tracey, :lacey, :macey]
```

To get the last few elements we can use negative indexes

```ruby
arr[-1] # => :macey
arr[-2] # => :lacey
```

We can also concatenate arrays

```ruby
[1, 2, 3] + [4, 5] # => [1, 2, 3, 4, 5]
# (this is a new array. neither is mutated)
```

## Hashes

A [`Hash`](https://ruby-doc.org/core-2.2.0/Hash.html) is similar to a JS object.

```ruby
ari = { 'name' => 'Ari', 'age' => 24 }
ari['name'] # => 'Ari'
ari['foo'] # => nil
```

We ONLY use bracket notation to get and set values

```ruby
ari['age'] # => 24
ari.age # => NoMethodError: undefined method `age' for Hash

ari['age'] = 25
ari['age'] # => 25

ari.age = 25 # => NoMethodError: undefined method `age=' for Hash
```

`Hash` keys are usually `Symbol`s not `String`s

```ruby
ari = { :name => 'Ari', :age => 24 }
ari[:name] # => 'Ari'
ari['name'] # => nil
```

Since using symbols as keys is so common, there is a nice short-hand

This is exactly the same as what we saw above.

```ruby
ari = { name: 'Ari', age: 24 }
```


## Iteration

In JS if we wanted to print numbers 0 through 3 we would:

```javascript
for(var i = 0; i < 3; i++) {
  console.log(i);
}
// > 0
// > 1
// > 2
```

In Ruby this is much cleaner:

```ruby
3.times { |i| p i }
# > 0
# > 1
# > 2
```


> Yes there _are_ `for` loops in Ruby but we DO NOT use them

We can also iterate over arrays:

```ruby
arr = [10, 20, 30]

arr.each { |num| p num }
# > 10
# > 20
# > 30

arr.map { |num| num / 10 }
# => [1, 2, 3]
```

For blocks with longer lines or multiple lines, replace `{` and `}` with `do` and `end`

```ruby
arr.map do |num|
  "#{num / 10} is great!"
end
# => ["1 is great!", "2 is great!", "3 is great!"]
```

And we can iterate over hashes:

```ruby
hash = { a: 1, b: 2, c: 3 }
hash.each do |key, val|
  p "the value of #{key} is #{val}"
end
# > the value of a is 1
# > the value of b is 2
# > the value of c is 3
```

## Methods

In Ruby everything is a [`Method`](https://ruby-doc.org/core-2.2.0/Method.html).  There are NO functions!


```ruby
def add(a, b)
  a + b
end

add(1, 2) # => 3

add(1, 2, 3)
# > ArgumentError: wrong number of arguments (given 3, expected 2)
```

Notice we do not need the keyword `return`.  The last line hit by the method will always be the return value.  This is called an _implicit return_.

We can add default arguments

```ruby
def add(a, b=10)
  a + b
end

add(1, 2) # => 3
add(1) # => 11 (b defaults to 10)
```

We can also have an arbitrary number of arguments

```ruby
def add(*nums)
  return 0 if nums.empty?
  nums.inject { |sum, n| sum + n }
end
```

Above, the `nums` object is just an array of the arguments given.  If there are no arguments given, the `nums` will be empty and we will `return` early.

We also do NOT use parentheses when calling a method without arguments

```ruby
def say_hi(name = 'World')
  p "Hello, #{name}!"
end

say_hi('Stacey')
# > Hello, Stacey!
say_hi
# > Hello, World!
```

We called the method without using parens!

### Bonus: [defining methods that `yield` blocks](https://git.generalassemb.ly/wdi-nyc-peloton/LECTURE_U04_D01_Ruby_101/blob/master/blocks.md)



## Control Flow

Cake.

### `if`/`elsif`/`else` and ternary


```ruby
def number_message(num)
  if num < 10
    p "what a small number"
  elsif num > 10
    p "That is a big number!"
  else
    p "That number is just right!"
  end
end
```

#### `if` / `unless`
We also have single-line ifs

```ruby
p "you are old!" if age >= 100
```

You may even see `unless`

```ruby
p "you are old!" unless age < 100
```
When you see an `unless foo`, read it as `if !foo`

> Only use `unless` if the condition cannot be written as a positive (without a `!`).

#### Ternary operator  

A ternary operator looks just like we have seen in JS

```ruby
num.even? ? "#{num} is even!" : "#{num} is odd!"
```

### `while` loops

```ruby
a = 10
while a.positive?
  p a
  a -= 1
end
```

> There are also `until` loops. Only use `until` if the condition cannot be written as a positive (without a `!`).

### Binary operators `&&`/`||`

Binary operators are pretty much the same as in JS :)

```ruby
1 && 2 # => 2 (truthy)
nil && 2 # => nil (falsy)
1 && nil # => nil (falsy)

1 || nil # => 1 (truthy)
nil || 2 # => 2 (truthy)
false || nil # => nil (falsy)
```

## Error-handling

Ruby we have `raise` to throw errors `rescue` to catch them.

These are most common when using and creating external APIs but probably don't need to be used in your own internal code.

If you are interested flex your google muscles and learn it on your own ;)

## Style Things

The Ruby community is very opinionated about styling.  As you are starting out, you MUST follow [these rules](https://github.com/bbatsov/ruby-style-guide).

Here are the most important rules

**Casing**

* All variables and methods must use `snake_case`
* All classes and modules must use `CamelCase`
* All constants (besides classes and modules) must use `SCREAMING_SNAKE_CASE`

**Blocks**

* A single line block must use `{` and `}`
* A multi-line must use `do` and `end`
* If an argument is unused it should start with `_` (or just be named `_`)
  * `hash.each { |_key, val| p val }`

**Methods**

* A method should end with a `?` if an only if it always returns a boolean
  * These are called _predicate methods_
* A method ending in `!` should be a _dangerous_ version of the method sans `!`
  * _dangerous_ means either it can mutate or raise an error
* Don't name methods like `get_foo`, `set_foo`. They should be `foo` and `foo=`
* **Do** use `attr_reader` and `attr_writer`
* Do not use parens when calling a method without args
  * `super` is one possible exception

**Other stuff**

* Do not use `for`/`in` loops
* Do not use _class variables_ (`@@these_things`)
* Always use two spaces to indent
  * Your text editor should do this for you when you hit tab
* Don't use semi-colons (unless you are aware of the few exceptions)
* Do not use global variables (`$these_things`). Usually a constant will do



## Lab Time!

Ruby is really fun!

Like anything else, you will only learn if you _do_ it. [Start doing!](https://git.generalassemb.ly/wdi-nyc-peloton/LAB_U04_D01_Ruby_101)

## Resources

* [Ruby docs](http://ruby-doc.org/core-2.4.1/)
* [Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide)
* [Reserved words](http://www.java2s.com/Code/Ruby/Language-Basics/Rubysreservedwords.htm)

## Conclusion
- Why is Ruby awesome?
- What are some differences between Ruby and Javascript?
  - Where can they each run? What are the differences between Ruby objects and JS object?  Ruby methods and JS methods?
