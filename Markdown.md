# Markdown Tutorial for Obsidian

Markdown is a lightweight markup language that lets you format text easily. Obsidian uses Markdown as its core.

---

## Headings

Use `#` for headings:

```markdown
# H1 - Largest
## H2
### H3
#### H4
##### H5
###### H6 - Smallest
Text Formatting
Bold: **bold** → bold

Italic: *italic* → italic

Bold + Italic: ***bold italic*** → bold italic

Strikethrough: ~~strikethrough~~ → strikethrough
```

## Lists
### Unordered List
```markdown
- Item 1
- Item 2
  - Subitem 2.1
  - Subitem 2.2
```

### Ordered List
```markdown
1. First
2. Second
3. Third
```

Links
```markdown
[Link Text](https://example.com)
Example: Obsidian
```

Images
```markdown
![Alt text](image_path_or_url)
```
Example:

![Cute cat](https://placecats.com/300/200) 

## Code
### Inline code
Use backticks `inline code`

Example: `printf("Hello World");`

### Code blocks
Use triple backticks ``` with optional language for syntax highlighting:

```c
#include <stdio.h>
int main() {
    printf("Hello, Markdown!\n");
    return 0;
}
```

### Blockquotes
```markdown
> This is a quote.
> Multiple lines supported.
```

### Horizontal Rule
```markdown
---
Tables
| Header 1 | Header 2 |
| -------- | -------- |
| Cell 1   | Cell 2   |
| Cell 3   | Cell 4   |
```

### Checklists (Task Lists)
```markdown
- [x] Completed task
- [ ] Incomplete task
```

### Footnotes
```Markdown
Here is a footnote reference[^1].

[^1]: This is the footnote.
```

Tips for Obsidian
Use Ctrl+E or Cmd+E to toggle between Preview and Edit mode.

Use [[Note Title]] to link to other notes.

Use tags with #tag to categorize notes.

Use the command palette (Ctrl+P / Cmd+P) for quick commands.

