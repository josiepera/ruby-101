## Defining methods that `yield` blocks

Assume we want to define a method that does a thing with some probably.

For example, if we want to something to happen with a 1 in 5 chance, we may want to execute

```ruby
maybe_do_thing(5) { puts '1 in 5 chance!' }
```

We could define the method like this:


```ruby
def maybe_do_thing(odds, &blk)
  if rand(odds) == 0
    blk.call
  end
end
```
> `rand(n)` returns a random integer from 0 to n exclusive.  (In this case from 0 to 5)

The `&` indicates that `blk` is a block

Or we can drop the last arg and just use the `yield` keyword:

```ruby
def maybe_do_thing(odds)
  if rand(odds) == 0
    yield
  end
end
```

`yield` will execute whatever block is passed in

### Blocks with arguments

We can also execute blocks with arguments.

Let assume we want to define our own `map`

```ruby
names = ['Stacey', 'Tracey', 'Macey', 'Lacey']
my_map(names) { |name| name.upcase }
# => ["STACEY", "TRACEY", "MACEY", "LACEY"]
```

To implement this we could do

```ruby
def my_map(arr, &blk)
  mapped = []
  arr.each do |e|
    mapped.push blk.call(e)
  end
  mapped
end
```

Or more simply

```ruby
def my_map(arr)
  mapped = []
  arr.each do |e|
    mapped.push yield(e)
  end
  mapped
end
```

### Checking for a block

We don't automatically get errors if we don't pass in a block.

Instead we can check [`block_given?`](https://apidock.com/ruby/Kernel/block_given%3F) to see if a block was passed in

Here is a method that will return `'No block!'` if no block is given and execute the block otherwise

```ruby
def block_or_message
  if block_given?
    yield
  else
    'No block!'
  end
end
```
> Modified from the [link](https://apidock.com/ruby/Kernel/block_given%3F) above. (`try` is not native Ruby and is defined differently in Active Support)
