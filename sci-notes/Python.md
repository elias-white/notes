# Python
- Python is an interpreted language, meaning it is a programming language whose implementations execute instructions directly and freely, without previously compiling a program into machine-language instructions.
- Python is dynamically typed


### Strings
#### String Formatting
The old way of formatting strings (Python 2) is with the `%` operator:
```python
'Hey %s, there is a 0x%x error!' % (name, errno)
# Prints: 'Hey Bob, there is a 0xbadc0ffee error!'
```
The new (Python 3) way is with the `.format()` method:
```python
'Hey, {}. Are you {}?'.format(name, gender)
```
Or, for better maintainability you can assing variable names to the substitutions:
```python
'Hey {name}, there is a 0x{errno:x} error!'.format(
...     name=name, errno=errno)
```
In Python 3.6, String Interpolation or f-Strings were added which allows for an alternative way to format strings and add expressions directly into the string:
```python
f'Hello, {name}.'
```
For user supplied input, the use of **Template Strings** are recommended:
```python
from string import Template
t = Template('Hey, $name!')
t.substitute(name=name)
'Hey, Bob!'
```
Template Strings are safer, as Format Strings can sometimes pose a security risk[^1].   

#### Type Conversion
```python
int("5")
str(5)
float(3) #3.0
```

## Collection Data Types
 ### Lists
 Lists are sequences of arbitrary objects (arrays in Javascript), indexed from zero. 
 ```python
# two ways to create empty lists
list_one = []
list_two = list()

male_names = ["Elias", "Bob", "Frank"]
male_names.append("Michael")
male_names.insert(2, "Thomas") # Inserts after position 2
male_names[:3] # Lists can also be sliced
female_names = ["Sarah", "Lily"]
names = male_names + female_names # Plus operator concatenates lists - ["Elias", "Bob", "Frank", "Sarah", "Lily"]
```

 ### Tuples
 To create simple data structures, you can pack a collection of values together into a single object using a tuple.
 
Tuples are immutable, meaning you cannot change the value after its assignment. 
Tuples and lists share a lot of common features, but since tuples are immutable they are more memory efficient if you intend to create a large number of small lists (<12 elements) as lists overallocate memory to optimise new add operations. 

 ```python
stock = ('GOOG', 100, 490.10) # can contain many data types
name, shares, price = stock # unpacking tuples
# an easy way to iterate over lists
total = 0.0  
for name, shares, price in portfolio:
	total += shares * price
```

### Sets
A set is used to contain an unordered collection of objects. Unlike lists and tuples, sets are unordered and cannot be indexed by numbers. Moreover, the elements of a set are never duplicated. Sets are mostly used for mathematical operations.

```python
# to create a set
s = set([3, 5, 9, 10])
t = set("Hello")
t # since set elements cannot be duplicated, t will return 'helo'
```

### Dictionaries
A dictionary is an associative array or hash table that contains objects indexed by keys.
```python
prices = {} # An empty dict 
prices = dict() # An empty dict
stock = {
		 "name": "GOOG",
		 "shares": 100,
		 "price": 490.10
}
# dict membership can be tested using the in operator
if "SCOX" in prices: 
	p = prices["SCOX"]
else:
	p = 0.0
# or, more compactly
p = prices.get("SCOX",0.0)
# to get list of dict keys
syms = list(prices) # syms = ["AAPL", "MSFT", "IBM", "GOOG"] 
del prices["MSFT"] # delete element from dict
```

### Generators
Instead of a function returning a single result, a function can generate an entire sequence if it uses the `yield` statement. These types of functions are called **generators**.

Calling a generator function creates an object that produces a sequence of results through successive calls to a next() method (or __next__() in Python 3). For example:
```python
c = countdown(5)
c.next()
# 5 
c.next()
# 4
c.next()
# 3

# Normally, you would not manually call `next()`, rather, you would use a `for` loop:
for i in countdown(5):
	print(i)
```

Generators are great for functions that do a lot of work but occasionally want to report back to the caller. Generators are an extremely powerful way of writing programs based on processing pipelines, streams, or data flow. 

### Coroutines
A coroutine is a function that operates on a series of inputs sent to it. They benefit from the ability to keep their data throughout their lifetime and, unlike functions, can have several entry points for suspending and resuming execution.
```python
def print_matches(matchtext): 
	print "Looking for", matchtext 
	while True:
		line = (yield) # Get a line of text if matchtext in line:
		print line

matcher = print_matches("python")
matcher.next()
# looking for python
matcher.send("Hello World")  
matcher.send("python is cool")  
# python is cool  
matcher.send("yow!")  
matcher.close() # Done with the matcher function call

```
A coroutine *consumes* data while a generator *produces* data.

[^1]: It’s [possible for format strings to access arbitrary variables in your program](http://lucumr.pocoo.org/2016/12/29/careful-with-str-format/).

