# Betty Style Documentation Guide
## General Rules

Indentation: Always use tabs, never spaces.

Functions: Limited to 5 functions per file.

Lines: Maximum 40 lines per function.

Line length: Maximum 80 characters per line.

## File structure:

Functions, structs, enums, macros should all have proper comments.

Each .c or .h file starts with a header comment block.

## File Header

At the top of each file:
```c
/*
 * File: <filename>
 * Auth: <author>
 * Desc: <Short description of the file>
 */
```

### Rules:

Use a block comment (/* ... */)

Include file name, author, and description

Keep description concise, wrapping at 80 characters

## Function Documentation

Above each function:
```c
/*
 * <function_name> - <Short description of what the function does>
 * @<param1>: <Description>
 * @<param2>: <Description>
 *
 * Return: <What the function returns>
 */
```

### Rules:

Always document all parameters with @param:

Always document the return value

Keep lines within 80 characters

Use tabs for indentation inside function bodies

## Structs

Document structs like this:
```c
/*
 * struct <StructName> - <Description>
 * @<field1>: <Description>
 * @<field2>: <Description>
 */
typedef struct <StructName>
{
    <type> <field1>;
    <type> <field2>;
} <StructName>;
```

### Rules:

Each field gets an @ description

Tab-indented

Use meaningful struct and field names

## Enums

Document enums like this:
```c
/*
 * enum <EnumName> - <Description>
 * @<VALUE1>: <Description>
 * @<VALUE2>: <Description>
 */
typedef enum <EnumName>
{
    VALUE1,
    VALUE2
} <EnumName>;
```



### Rules:

Document all enum values

Tab-indented

Keep names descriptive

## Macros / Defines

Document macros:
```c
/*
 * <MACRO_NAME> - <Description>
 */
#define <MACRO_NAME> <value>
```

### Rules:

Uppercase for macro names

Short description of purpose

## Notes

Consistency is key: Every file, function, struct, enum, and macro should follow this format.

Betty linter will enforce tabs, header blocks, parameter documentation, and line lengths.

Keep comments clear, concise, and informative.