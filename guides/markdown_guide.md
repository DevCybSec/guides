# Complete Guide to Markdown

Markdown is a lightweight markup language used for formatting text. It’s widely used in writing documentation, creating README files, and writing content for web-based platforms. Markdown is simple and focuses on readability. Below is a complete guide to using Markdown effectively.

---

## Table of Contents
1. [Basic Syntax](#basic-syntax)
    - [Headings](#headings)
    - [Paragraphs](#paragraphs)
    - [Line Breaks](#line-breaks)
    - [Bold and Italics](#bold-and-italics)
    - [Blockquotes](#blockquotes)
    - [Lists](#lists)
    - [Code Blocks](#code-blocks)
2. [Links and Images](#links-and-images)
    - [Adding Links](#adding-links)
    - [Adding Images](#adding-images)
3. [Tables](#tables)
4. [Escaping Characters](#escaping-characters)
5. [Extended Syntax](#extended-syntax)
    - [Task Lists](#task-lists)
    - [Footnotes](#footnotes)
    - [Strikethrough](#strikethrough)
6. [Conclusion](#conclusion)

---

## 1. Basic Syntax

### Headings

Headings in Markdown are created using the `#` symbol. The number of `#` symbols indicates the heading level.

```markdown
# H1 Heading
## H2 Heading
### H3 Heading
#### H4 Heading
##### H5 Heading
###### H6 Heading
```

**Output:**

# H1 Heading  
## H2 Heading  
### H3 Heading  
#### H4 Heading  
##### H5 Heading  
###### H6 Heading

---

### Paragraphs

To create a paragraph, just write your text on a new line. To create multiple paragraphs, leave a blank line between them.

```markdown
This is a simple paragraph.

This is another paragraph.
```

---

### Line Breaks

To create a line break within a paragraph, end a line with two spaces or insert `<br>`.

```markdown
This is the first line.  
This is the second line.
```

---

### Bold and Italics

- **Bold** text is created by wrapping text in `**` or `__`.
- *Italic* text is created by wrapping text in `*` or `_`.
- You can also **_combine bold and italics_**.

```markdown
**This is bold text**
*This is italic text*
__This is also bold__
_This is also italic_
**_Bold and italic_**
```

---

### Blockquotes

Blockquotes are created by placing a `>` symbol in front of a paragraph.

```markdown
> This is a blockquote.
```

**Output:**

> This is a blockquote.

---

### Lists

#### Unordered Lists

Unordered lists use `-`, `*`, or `+` as bullet points.

```markdown
- Item 1
- Item 2
    - Subitem 1
    - Subitem 2
```

**Output:**

- Item 1
- Item 2
    - Subitem 1
    - Subitem 2

#### Ordered Lists

Ordered lists use numbers followed by a period.

```markdown
1. First item
2. Second item
    1. Subitem 1
    2. Subitem 2
```

**Output:**

1. First item  
2. Second item  
    1. Subitem 1  
    2. Subitem 2

---

### Code Blocks

For inline code, wrap text in backticks (\`code\`).

```markdown
Here is some `inline code`.
```

For multi-line code blocks, use triple backticks (```) or indent lines by four spaces.

\`\`\`
function helloWorld() {
    console.log("Hello, world!");
}
\`\`\`

**Output:**

```
function helloWorld() {
    console.log("Hello, world!");
}
```

---

## 2. Links and Images

### Adding Links

To add a link, wrap the link text in square brackets `[ ]` and the URL in parentheses `( )`.

```markdown
[Google](https://www.google.com)
```

**Output:**  
[Google](https://www.google.com)

---

### Adding Images

To add images, use an exclamation mark `!` followed by the alt text in brackets `[ ]` and the image URL in parentheses `( )`.

```markdown
![Alt Text](https://example.com/image.jpg)
```

---

## 3. Tables

Markdown supports tables using pipes `|` and dashes `-` to define the structure.

```markdown
| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Row 1 Col 1 | Row 1 Col 2 | Row 1 Col 3 |
| Row 2 Col 1 | Row 2 Col 2 | Row 2 Col 3 |
```

**Output:**

| Header 1    | Header 2    | Header 3    |
|-------------|-------------|-------------|
| Row 1 Col 1 | Row 1 Col 2 | Row 1 Col 3 |
| Row 2 Col 1 | Row 2 Col 2 | Row 2 Col 3 |

---

## 4. Escaping Characters

Use a backslash `\` to escape special Markdown characters.

```markdown
\*This text will not be italicized\*
```

**Output:**

\*This text will not be italicized\*

---

## 5. Extended Syntax

### Task Lists

You can create task lists using `- [ ]` for unchecked boxes and `- [x]` for checked boxes.

```markdown
- [ ] Task 1
- [x] Task 2 (completed)
```

**Output:**

- [ ] Task 1  
- [x] Task 2 (completed)

---

### Footnotes

Footnotes are created by adding a `[^1]` notation in the text and defining the footnote at the end of the document.

```markdown
This is a sentence with a footnote.[^1]

[^1]: This is the footnote text.
```

---

### Strikethrough

You can strikethrough text using two tildes `~~`.

```markdown
~~This text is crossed out~~
```

**Output:**

~~This text is crossed out~~

---
