# Java Regex Cheat Sheet (with Examples)

## Basic Syntax

-   `.` → Any character except newline
-   `\d` → Digit (0-9)
-   `\w` → Word character
-   `\s` → Whitespace
-   `\` → Escape character (double escaped in Java strings)

## Quantifiers

-   `*` → 0 or more
-   `+` → 1 or more
-   `?` → 0 or 1
-   `{n}` → Exactly n times
-   `{n,m}` → Between n and m times\
    Example:

``` regex
\d+
```

## Anchors

-   `^` → Start of string
-   `$` → End of string
-   `\b` → Word boundary\
    Example:

``` regex
^\d+$
```

## Groups

-   `(abc)` → Capturing group
-   `(?:abc)` → Non-capturing group\
    Example:

``` regex
(\d{4})-(\d{2})-(\d{2})
```

## Alternation

``` regex
cat|dog
(red|blue|green)
```

## Lookarounds

-   `(?=X)` → Positive lookahead
-   `(?!X)` → Negative lookahead
-   `(?<=X)` → Positive lookbehind
-   `(?<!X)` → Negative lookbehind\
    Example:

``` regex
\d+(?=€)
```

## Greedy vs Lazy

-   `.*` → Greedy
-   `.*?` → Lazy\
    Example:

``` regex
<.*?>
```

## Java Usage Example

``` java
Pattern p = Pattern.compile("\\d+");
Matcher m = p.matcher(text);
if (m.find()) {
    System.out.println(m.group());
}
```

## Common Patterns

-   Email:

``` regex
^[\w.-]+@[\w.-]+\.[a-zA-Z]{2,}$
```

-   Username:

``` regex
^[a-zA-Z0-9_]{3,20}$
```

-   Password:

``` regex
^(?=.*[A-Z])(?=.*\d).{8,}$
```

## Rules of Thumb

-   Avoid `.*` when possible
-   Prefer `find()` over `matches()` unless matching whole string
-   Regex is for matching, not parsing
-   Escape **twice** in Java strings
