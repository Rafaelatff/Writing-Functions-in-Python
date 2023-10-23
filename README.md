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
