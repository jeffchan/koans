# Ruby Koan notes

Things that were noteworthy to me, as someone coming from JavaScript, Python, and PHP.

### Nil
- `nil` is an Object
- `nil.to_s` is empty string (makes sense!)
- `nil.inspect` is `nil` (different than `to_s`, interesting)

### Objects
- Everything is an object
- Test with `.is_a?(Object)`
- Call `.to_s` to convert to string
- Call `.inspect` to inspect. Example: `123.inspect` results in `"123"`
- Every object has object_id which is class of `Fixnum`
    - Integers have fixed ids 2n+1
- Call `.clone` to clone the object

### Arrays
- Create an array with a literal `[]` or with `Array.new`
- Just like in Python, negative indexing is allowed. Example `arr[-1]`
- Or access elements with helpers like `.first` or `.last`
- Slice with bracket syntax `arr[index, count]`
- Slice with ranges`[1..2]` is inclusive, `[1…2]` is exclusive
- Use `<<` shovel operator to push elements
- `(1…5)` is a Range, which can be converted to an array with `to_a`
- Standard methods like `#shift`, `#pop`, `#push`, and `#unshift` exist. They modify the array.

### Array assignments
- Like in ES6 and Python, destructuring is allowed. Technically called "parallel assignments".
- If index out of bound, `nil` is set.
- `*` is the splat operator. Useful for getting a list of arguments.
    - Another example: `first_name, *last_name = ["John", "Smith", "III"]` where `last_name` becomes an array `["Smith", "III"]`.

### Hashes
- Like a dictionary in Python or object in JS. Use `Hash.new` or `{}` literal to create
- Use `.size` to get the size of a hash
- Two ways to do lookups. `hash.fetch` and `hash[:key]`.
- `#keys` and `#values` to access arrays for each.
- `#merge` to merge two hashes
- `Hash.new(default_val)` for a default value if key does not exist
    - Block is supported to access the key. Example: `Hash.new {|hash, key| hash[key] = [] }`
- If key does not exist, `#fetch` returns a `KeyError` where as `#[]` returns `nil`.
- Caution: hashes may be confused with a block
- Hash is unordered.

### Strings
- Pretty standard in terms of single and double quotes, as well as escaping.
- Single quotes do not interpret escape characters like `\n`
- Double quotes do string interpolation but not single quotes.
    - Example: "The value is #{value}"
- Heredocs are supported
- Flexible quotes are a thing. `%{…}` Use this for multi-lines. Other examples: `%!…!` or `%(…)`
- `+` operator can be used for concatenation. (Creates a new string)
- `<<` shovel operator works for concatenation too. (Modifies string)
- Use `#[]` brackets to access a specific character at an index
- In Ruby 1.9+, single characters are represented by strings, whereas prior versions are represented by ascii.
- `#split` can be used to split (pass in string or regex). Default splits by space.
- Strings are unique objects. Example `"a".object_id != "a".object_id`


### Symbols
- Part of `Symbol` class
- Symbols are the same internal object. Example `:a.object_id != :a.object_id`
- All method names become symbols. Woah.
- `#to_sym` on a String to convert it to a Symbol.
- Symbols can have spaces O_O. Example: `:"cats and dogs"`
- And can do interpolation. Weird. :"cats #{value} dogs"
- Dynamically create them too. `("cats" + "dogs").to_sym`
- Symbols are not strings. They do not have string methods.
    - "It's important to realize that symbols are not 'immutable strings', though they are immutable."
    - Example: Cannot use plus operator to concat two symbols.

### Regular expressions
- Part of `Regexp` class. Use two `/` to create a regex literal.
- Pretty standard regex syntax.
- Can be used with bracket syntax. Woah. `"some matching content"[/match/]`
- Failed match returns `nil`. Example: `"some matching content"[/missing/]`
- Groups
    - `"Gray, James"[/(\w+), (\w+)/, 1]` allows you to access just the first group.
    - Or use `$1`, `$2`, etc. to access.
- `#sub` and `#gsub` can be used to substitution (or global substitution). Example: `"one two-three".sub(/(t\w*)/) { $1[0, 1] }`
- `#scan` is like a `find_all` 

### Methods
- Wrong number of arguments is a runtime error (doh). `ArgumentError`
- Splat operator `*args` to collect a bunch of arguments into an array
- When calling a class's own method, `self.` (explicit receiver) is not required. 
- NB: Cannot call a private method with an explicit receiver e.g. `self.`

### Keyword arguments
- New in Ruby 2
- Allows you to specify method arguments by keyword, and hence in any order.
- Wrong number of arguments still raises `ArgumentError`
- As with other languages, keyword arguments always have a default value, making them optional to the caller.

### Constants
- Constants follow lexical scoping over inheritance hierarchy
- Use :: to access top-level global constants
- Defined with initial uppercase letter
- Use `const_get` to dynamically access constants in a scope

### Control statements
- Every statement in Ruby will return a value, not just if-statements
- Two ways to do ternary. Inline `if…else…end` or C-style `bool ? :a : :b`
- `break` statements can return a value too
- `next` is equivalent to `continue` in other languages
- `10.times` statement for a fixed number of loop

### True/false
- `nil` is treated as `false`
- `0` is treated as `true`, as with most things including empty string

### Exceptions
- Use `.ancestors` to access parent/ancestor class (it’s a list)
- All exceptions inherit from `Exception` which is an `Object`
- `rescue` is the same as a `catch` in other languages
- Use `ex.message` to access the exception message
- `raise` and `fail` as synonyms  
- `ensure` is the same as `finally` in other languages
- Create a new error with `MyError.new` and you can `raise` is directly

### Iteration
- `#break` works instead do-end blocks too
- `#collect` is the same thing as `map` (both methods exist)
- `#find_all` and `#select` is the same thing as `filter` in JavaScript
- `#find` finds the first matching element
- `#inject` is like `#reduce` in JavaScript
- You can `map` over a file, ranges, etc (any collection)

### Blocks - should revisit this topic
- See: https://reactive.io/tips/2008/12/21/understanding-ruby-blocks-procs-and-lambdas
- Use `yield` (without arguments) to take in a block/do-end
- Use `yield` (with arguments) to pass argument to block/do-end
- `block_given?` can be used to check if block is passed
- Create a lambda and you can use `lambda[arg]` to call on it (or `.call`) o.o

### Classes
- `#instance_variables` gives you an array of the instance variable names
- `attr_reader` for defining an accessor for instance variables (like a getter)
- `attr_accessor` for defining read+write accessor
- `inspect` is used for printing out a class (a fancier `#to_s`)
- Classes are Objects.
- Instances of classes inherit from Class/Object

### Open classes
- You can “open” an existing class and add to it O_o
- Even built-ins can be extended

### Modules/Scope
- Similar to traits/mixins in other languages
- Cannot instantiate a module
- Modules also act like a namespace
- References to class name assume current scope
- Use `::` prefix to reference global class name

### Class Methods
- Define static methods difference on the class `def Dog.method`
    - Can do it inside OR outside the class
    - Or `def self`
    - Or `class << self`
- Last statement in a class definition is also a return statement O_o

### Messaging Passing
- Summary: Use `#send` or `#__send__` to invoke methods dynamically
- `#respond_to?` as a check to see if methods exist
- `#method_missing` is a dynamic catching when methods called on don’t exist

### to_str
- https://gist.github.com/kinopyo/5682347
- `#to_str` is only implemented by the String class. Everything else should use `#to_s`

