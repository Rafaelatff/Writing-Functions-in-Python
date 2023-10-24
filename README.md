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
