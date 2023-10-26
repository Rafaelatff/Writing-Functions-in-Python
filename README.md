# Writing-Functions-in-Python
Repository created during the Writing Functions in Python course of the DataCamp.

Write docstring notes together with the function, something like:

```py
def function_name(var1, var2=5):
""" 
This function does that!

Args: 
	var1 (DataFrame): var1 does ...
	var2 (int, optional): var2 does ...

Returns:
	DataFrame

I alsou could add here the description of erros raised, if any and optional 
extra notes or examples of usage.

"""
```

To retrieve the docstrings, we can:

```py
print(function_name.__doc__)
```
or also:
```py
import inspect
print(inspect.getdoc(function_name))
```

We need to use the principe:

Do One Thing! We cannot create functions that does a lot of stuff. Better to 
create several functions isntead of one big function.

Python pass the information to the functions **by assignment**.

In Python, **integers are inmutable**. 

![image](https://github.com/Rafaelatff/Writing-Functions-in-Python/assets/58916022/52d53712-4ec4-46de-89b6-deb7bfd15a66)

| Immutable | Mutable | 
| :---: | :---: | 
| int | list |
| float | dict |
| bool | set |
| string | bytearray |
| bytes | objects |
| tuple | functions |
| frozenset | almost everything else |
| None | .. |

## Context managers

With context managers, it goes to the function and when it finishes, it removes the context of that function.

For example:
```py
with open('filename.txt') as file:
	text = file.read()
```

I can leave the function withou closing the file.

* with <context-manager>(<args>): 
or
* with <context-manager>(<args>) as <returned-value-here>: 


* with timer(): 
At the end, this function will return: "Elapsed: x.xy seconds"

Creating context manager

```py
# 1. Define a function
def variable_name():

# 2. (optional) Add any set up code your context needs

# 3. Use the "yield" keyword
	yield 

# 4. (optional) Add any teardown code your context needs
""" Database disconnection for example """ 

# 5. Add the '@contextlib.contextmanager' decorator at the top
```
Applying those steps:

```py
@contextlib.contextmanager
def fnc_name(url_path):
	db = postgres.connect(url)
	yield db
	db.disconnect()

#calling the function

with fnc_name('http://link') as my_db:
	list = my_db.execute('SELECT * FROM column')

```
## Nested contexts

This is done by adding a for loop. For example copyting a file:

```py
def copy(src, dst):
	with open(src) as f_src:
		with open(dst, 'w') as f_dst:
			for line in f_src:
				f_dst.write(line)
```
## Handling erors

we can use:
```py
try:
	# code that might raise and error
except:
	# do something about error
finally:
	# code that runs no matter what (disconnection for example)
```
## Functions

Functions are a type of object. Meaning that I can do all the stuff I usually do with objects. Python objects are:

```py
def x():
	pass

x = [1,2]
x = pd.DataFrame()
variable = x 
type(variable) # It will return <type 'function'>
variable() 
```

I could do stuff like:
```py
variable_name = print
variable_name('Hello') # It will print the message 'Hello'
```

And also:

```py
list_of_fnc = [x, open, print]
list_of_fnc[2]('This message will be printed')
```

Since function is an object, I can pass the function as argument also.

```py
def has_docstring(func):
	return func.__doc__ is not None

def no():
	return 42

def yes():
	"""It has docstream"""
	return 42

has_docstring(no)	# It returns 'False'
has_docstring(no)	# It returns 'True'
```

## Referencing a function

When I just call ```my_func()```, I am calling the function and it will run the code inside it. When I type ```my_func```, I am referencing the function itself. In the last case it will return the message <function my_func at 0x012345678912>.

## Scope

Scope determine which variables can be accessed at different points of your code.
Variable are search first inside the **Local scope** (inside the function), then, if he can't find, the variable will be looked on the **Global scope**. If interpreter can't find in both, it will be searched on the **Builtin scope**.

When we want to call a value outside the function, we need:

```py
x = 7

def foo():
	global x
	x = 42
	print(x) # it will return 42

print(x) # it will also return 42, not 7!!
```

Note: If I were inside a nested function, I should use ```nonlocal``` instead of ```global```.

## Closure
![image](https://github.com/Rafaelatff/Writing-Functions-in-Python/assets/58916022/97d03f6e-f1f1-4496-87ca-05bf70bd59be)

## Decorators

Decorators are used when we want to add common behaviour for multiple functions.
Better seing in practice:

```py
@double_args # Note that I need to define this function also
def multiply(a,b)
	return a*b

multiply(1,5) # It will return 20, because of the @double_args decorator

```

![image](https://github.com/Rafaelatff/Writing-Functions-in-Python/assets/58916022/400d7966-1e29-413b-9894-1bf0b40e5e4c)

Now, let's time a function, by creating a decorator that checks how long the function being called takes to run.

```py
import time

def timer(func):
	"""A decorator that prints how long a function took to run."""
	def wrapper(*args, **kwargs): # Takes all number of position and key args 
		# Get the current time		
		t_start = time.time()
		# Call the function and store the result
		result = func(*args, **kwargs)	
		# Get the time that it takes
		t_total = time.time() - t_start
		print('{} took {}s'.format(func.__name__, t_total)) ## __name__ return its name, __defaults__ returns default args
		# Different function I could use: return func(*args, **kwargs)
		return result
	return wrapper
```

Now testing:

```py
@timer
def sleep_n_seconds(n):
	time.sleep(n)

sleep_n_seconds(5)
# For example it would return 'sleep_n_seconds took 5.00509..s'
```

![image](https://github.com/Rafaelatff/Writing-Functions-in-Python/assets/58916022/6cc8d108-9ec8-4f17-809b-b23951bc62c4)

![image](https://github.com/Rafaelatff/Writing-Functions-in-Python/assets/58916022/aedd5163-1fe9-40ef-a1b2-8491ede493c4)

## Decorators and metadata

It can generate problemns with the docstrings, name, defaults.
Usually it tries to return the metadata from the wrapper function.
To fix that:

```py
import functools import wraps

...
# And becore calling the wrapper function:
	@wraps(func)
	def wrapper(*args, **kwargs):
```

## Decorator that receives the function + arguments:

```py
def run_n_times(n)
	def decorator(func):
		def wrapper(*args, **kwargs):
			for i in range(n):	
				func(*args, **kwargs)
			return wrapper
		return decorator

```

Both works:

```py
@run_n_times(3)
def foo():

run_two_times = run_n_times(2)

@run_two_times
def foo():
```
Now let's modify an existing function: 

```py
print = run_n_times(5)(print)

print('What is happening?!?!') # It will print 5 times
```
