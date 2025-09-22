## 1️⃣ Basic try/except
```python
try:
	x = int(input("Enter a number: "))
	result = 10 / x
except ZeroDivisionError:
    print("You cannot divide by zero!")
except ValueError:
    print("That’s not a valid number!")
```

- `try`: code that might raise an exception

- `except`: handle specific exception types


---

## 2️⃣ Catching multiple exceptions

```python
try:
    x = int(input("Enter a number: "))
    result = 10 / x 
except (ZeroDivisionError, ValueError) as e:
	print(f"Oops! An error occurred: {e}")
```

- Catch multiple exceptions in one block
 
- `as e` lets you **access the exception object** and its message
  

---

## 3️⃣ Using the exception object
```python
try:
    data = {"a": 1}     
	print(data["b"]) 
except KeyError as e:
    print(f"Key {e} does not exist in the dictionary")
```

- `e` contains information about what went wrong (`'b'` in this case)

---

## 4️⃣ The `else` block
```python
try:
    x = int(input("Enter a number: "))
except ValueError:
    print("Invalid number!") 
else:
    print(f"Success! You entered {x}")`
```
- `else` runs **only if no exception occurred**

- Good for separating “normal execution” from error handling

---

## 5️⃣ The `finally` block
```python
try:
    x = int(input("Enter a number: "))
    result = 10 / x 
except ZeroDivisionError:
    print("Cannot divide by zero!")
finally:
    print("This always runs, no matter what")
```

- `finally` runs **always**, whether an exception occurred or not

- Useful for cleanup (closing files, releasing resources, etc.)


---

## 6️⃣ Raising exceptions with messages

```python
def divide(a, b):
    if b == 0:
		raise ValueError("Cannot divide by zero!")
		return a / b
	try:
		divide(10, 0)
	except ValueError as e:
		print(f"Caught an exception: {e}")
```
- `raise` lets you throw your **own exceptions**

- You can pass a **custom message** for clarity

---

## 7️⃣ Full template combining all
```python
try:
    x = int(input("Enter a number: "))
    if x < 0:
		raise ValueError("Number must be positive")
		result = 10 / x 
except ValueError as e:
	print(f"ValueError: {e}")
except ZeroDivisionError:
	print("Cannot divide by zero!")
else:
	print(f"Success! Result is {result}")
finally:
    print("This always executes at the end")`
```

✅ Handles multiple exceptions  
✅ Allows custom messages  
✅ Uses `else` for successful execution  
✅ Uses `finally` for guaranteed cleanup