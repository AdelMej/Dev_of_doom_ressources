# Python Docstring Guide (PEP 257 + PEP 8)
## 1️⃣ Module Docstrings

- Position: Top of the file, before imports

- Purpose: Describe the module’s purpose and contents

- Style: Triple double-quotes """
```python
"""Roman numeral conversion utilities.

Provides functions for converting Roman numerals to integers,
validating Roman numeral strings, and related helper functions.
"""
```

## 2️⃣ Class Docstrings

- Position: Immediately after the class line

- Purpose: Describe the class, its purpose, and main attributes
```python
class RomanConverter:
    """Convert Roman numerals to integers and validate them.

    Attributes:
        roman_values (dict): Mapping from Roman numerals to integer values.
    """
```

## 3️⃣ Function / Method Docstrings

- Position: Immediately after the def line

- Structure:
	- Short summary first line (≤79 chars)
	- Optional extended description
	- Optional sections: Args, Returns, Raises, Example
```python
def roman_to_int(roman_string):
    """Convert a Roman numeral string to an integer.

    Skips invalid characters. Returns 0 if input is None or not a string.

    Args:
        roman_string (str): The Roman numeral string.

    Returns:
        int: The integer value of the Roman numeral.

    Raises:
        TypeError: If input is not a string (optional).

    Example:
        >>> roman_to_int("XIV")
        14
    """
```

## 4️⃣ Inline Comments vs Docstrings

- Docstrings: Describe what a function/module/class does

- Inline comments: Describe how or why a line/block works

- total = 0  # accumulator for final integer value

## 5️⃣ Docstring Formatting Tips

- Use triple double-quotes """

- Keep first line ≤79 chars

- Use imperative mood for functions (Convert, Return)

- Separate extended description with a blank line

- Indent multi-line descriptions

- Use - type hints in docstring or Python typing

## 6️⃣ Docstring Styles (Optional)
### NumPy style
```python
def roman_to_int(roman_string):
    """
    Convert Roman numeral string to integer.

    Parameters
    ----------
    roman_string : str
        The Roman numeral string.

    Returns
    -------
    int
        The integer value of the Roman numeral.

    Raises
    ------
    ValueError
        If the input contains invalid characters.
    """
```

### Google style
```python
def roman_to_int(roman_string):
    """Convert a Roman numeral string to an integer.

    Args:
        roman_string (str): The Roman numeral string.

    Returns:
        int: Integer value.

    Raises:
        ValueError: If the input is invalid.
    """
```

## 7️⃣ Quick Reference Table

| Extended desc  | Optional, after blank line         |
| -------------- | ---------------------------------- |
| Module         | Top of file, before imports        |
| Class          | Immediately after `class` line     |
| Function       | Immediately after `def` line       |
| Short summary  | First line, ≤79 chars              |
| Extended desc  | Optional, after blank line         |
| Args           | Parameter name, type, description  |
| Returns        | Type + description                 |
| Raises         | Exceptions raised                  |
| Examples       | Optional, show usage               |
| Quotes         | Always triple double-quotes `"""`  |
| Inline comment | `#` for line-specific explanations |
