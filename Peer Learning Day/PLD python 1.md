# Python - Hello, World

## How to use the Python interpreter ?
SheBang #!/usr/bin/python3
```python
#!/usr/bin/python3
```
run python3 if python3 is installed
```bash
python3 mycode.py
```

## How to print text and variables using print
### without variables
```python
print("My text")
```
### with variables
```python
print(f"{variable_name}")
print("my variable : {}".format(my_variable))
print("my variable :", my_variable)
```
### How to use strings
define a variable with `""` or `''`
concat with `+`

## What are indexing and slicing in Python

### slicing
```python
mystring[start:end]
```
### indexing
```python
mystring[0]
```


### What is the official Python coding style and how to check your code with pycodestyle
PEP 8 is the official coding style for python

alias pystyle="pycodestyle --first --show-source --show-pep8"

## What is the purpose of the shebang `#!/usr/bin/python3`, and how does it affect script execution?
This uses the configuration of python for the user, can cause dependencies conflict you can use 
```bash
python3 -m venv venv
```
 to create a virtual environement then use
```bash
source venv/bin/activate
```
and then you can download modules with pip
## How do you make a Python script executable on Ubuntu (permissions and invocation)?
```bash
chmod u+x myfile.py
./myfile.py
```

## What are f-strings, and how do they compare to str.format() and % formatting?
```python
>>> test_variable = f"{ma_variable:.1f}"
>>> print(test_variable)
10.4
>>> test_variable = "{:.1f}".format(ma_variable)
>>> print(test_variable)
10.4
```

f strings and format don't have the same syntax but they are functionaly the same

## What’s the difference between writing to stdout vs stderr, and how do you write to each in Python?
```python
import sys
print("This is an error message", file=sys.stderr)   
```

`print` automatically prints to stdout

## Are Python strings mutable or immutable, and what are the practical implications for slicing and concatenation?
python string are immutable 

```python
>>> monstring = "abcdefgh"
>>> monstring + " this is added"
'abcdefgh this is added'
>>> monstring[2:4]
'cd'
>>> print(monstring)
abcdefgh
```

# python - if/else, loops, functions
## Why is indentation important in Python?
Indentation is important in python because python doesn't use braces `{}` like in c

## How do you use if and if ... else statements?
```python
if condition:
   # code to run
else:
    # code to run
```

## How do you write comments?
```python
# single line comment

'''
multiline
comment
'''
```

## How do you assign values to variables?
use `=` operator
```python
my_variable = "abcd"
```

## How do you use while and for loops?
```python
while condition:
    # code to run

for i in range(0, 10):
    # run from 0 to 9 because 10 is excluded

my_list = ["a", 10, "c"]
for i in my_list:
    # do whatever you want with the content of my_list


```
## How do break and continue work?
break gets out of a loop
continue skip the rest of the code and start the loop again

## How do else clauses on loops work, and when are they useful?
else statement for a loop runs when the condition of the loop becomes false so basically when the loop ends, you can debug with it and give instructions on when the loop ended and how

## What does the pass statement do, and when should you use it?
pass is a temporary placeholder you use it when you want the code to run but does nothing, generaly will be used for testing if you didn't implement a functionality yet
## How do you use range() effectively?
```python
range(start, end, increment)
```
end is not included so if you want to print from 0 to 10
```python
for i in range(0, 11):
    print(i)
```

## What is a function, and how do you define and call one?
a function is a code that you can call and give parameters too useful for repeated tasks
```python
def myfunctionname():
	print("myfunction")

myfunctionname()
```
## What does a function return if it has no return statement?
return the None special value when nothing is returned

## What is variable scope (local vs global), and how does it affect name resolution?
local scope is variables that are only used inside a function while global onse can be used by everything
example local variable x:
```python
def myfunction():
	x = 18
```
example global variable x:
```python
x = 23
```
global variable can be accessed by any functions throughout the code while local ones are locked into a specific function
## What is a traceback, and how do you read one?
traceback are the way python handle errors they generaly state the type of error and exceptions, and they give the line and file
```python
 Traceback (most recent call last):
  File "<python-input-13>", line 1, in <module>
    print("{:d}".format("oops"))
          ~~~~~~~~~~~~~^^^^^^^^
ValueError: Unknown format code 'd' for object of type 'str'
```

## What are the arithmetic operators in Python, and how do they behave?
- `+` can be used for concatenation and basic addition
- `-` can be used for substracting
- `/` can be used for dividing
- `%` can be used for modulo
- `*` can be used for multiplying
- `**` can be used for exposant operation like 2²
- `//` can be used for floor division (no float result takes the smaller value)

## How do logical operators (and, or, not) and short-circuit evaluation work in conditionals?
- `and` return True if both sides are true false otherwise
- `or` return True if at least one of the side is true
- `not` negate the condition return False if True, True if False


# Python - import & modules
## Why is Python programming considered “awesome” (productivity, readability, batteries-included)?
It's because python have extensive access to a ton of libraries and modules, leading to high versatility, and it's also very readable since it doesn't use boiler plates nor types (can be an issue for scalability)

## How do you import a function from another file?
```python
from file import function
```

## How do you use imported functions once they are imported?
```python
from file import function

function()
```

## How do you create a module and what file structure does it require?
to create a module all you need is a .py file
the structure is like that :
```yaml
sample/__init__.py
sample/core.py
sample/helpers.py
```
## How do you use the built-in function dir() and what does it reveal?
it reveals all of the methods of an object and you use it like that
```python
doc(obj)
```

## How do you prevent code in a script from executing when the file is imported?
to prevent execution while importing you do :
```python
if __name__ == "__main__":
```
this is a guard it will only execute when run and not imported

## How do you handle command-line arguments in Python programs?
to handle comand line argument you need to import argv from sys
```python
from sys import argv
```

## What is the difference between a module and a package, and what is the role of __init__.py?
a module is a simple .py file while a package can contain multiple .py files than can be accessed via the directoryname.packagename
init.py is implicitely executed when a package is imported try doing
```python
import this
```

## How does Python resolve imports (import system, sys.path, and PYTHONPATH)?
uses PYTHONPATH to search for libraries you can access PYTHONPATH in sys.path, import tries to first find the library using a finder and then load it with a loader

## What are absolute vs relative imports, and when should each be used?
absolute imports start from the root of the project while relative import start from the location of the file it can be used in packages if you need a function inside that package, it is also useful when the project start having large libraries,
common issues will be when the structure change the path might break and cause issues

absolute path start within the root directory
supose this structure
```yaml
└── project
    ├── package1
    │   ├── module1.py
    │   └── module2.py
    └── package2
        ├── __init__.py
        ├── module3.py
        ├── module4.py
        └── subpackage1
            └── module5.py
```
absolute import of module 5
```python
from package2.subpackage1.modules5 import somefunction
```
supose we are in module3 and want to access a function in module5
```python
from .subpackage.module5 import somefunction
```
suppose we are in module5 and want access to a function in module4
```python
from ..module4 import somefunction
```
