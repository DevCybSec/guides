### Easy Guide to Learning Emmet for HTML

Emmet is a powerful tool that helps you write HTML (and CSS) faster by using abbreviations that automatically expand into full code blocks. Here’s a quick and simple guide to get you started:

---

### 1. **Basic HTML Boilerplate**
   - **Abbreviation**: `!` + `Tab`
   - **Result**: Expands to a complete HTML5 document structure:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Document</title>
     </head>
     <body>
     </body>
     </html>
     ```

### 2. **Tags and Nesting**
   - **Single Tag**: Type `div` + `Tab` to get:
     ```html
     <div></div>
     ```
   - **Nesting**: You can create nested elements with the `>` symbol:
     - **Abbreviation**: `div>ul>li`
     - **Result**:
       ```html
       <div>
           <ul>
               <li></li>
           </ul>
       </div>
       ```

### 3. **Classes and IDs**
   - **Class**: Use a `.` followed by the class name:
     - **Abbreviation**: `div.my-class`
     - **Result**:
       ```html
       <div class="my-class"></div>
       ```
   - **ID**: Use a `#` followed by the ID name:
     - **Abbreviation**: `div#my-id`
     - **Result**:
       ```html
       <div id="my-id"></div>
       ```

   - **Multiple Classes/IDs**: Combine them:
     - **Abbreviation**: `div#my-id.my-class`
     - **Result**:
       ```html
       <div id="my-id" class="my-class"></div>
       ```

### 4. **Sibling Elements**
   - Use `+` to create sibling elements:
     - **Abbreviation**: `div+p`
     - **Result**:
       ```html
       <div></div>
       <p></p>
       ```

### 5. **Multiple Elements with Multiplication**
   - Use `*` to create multiple elements:
     - **Abbreviation**: `ul>li*3`
     - **Result**:
       ```html
       <ul>
           <li></li>
           <li></li>
           <li></li>
       </ul>
       ```

### 6. **Text Content**
   - Use `{}` to add text content inside elements:
     - **Abbreviation**: `h1{Hello World}`
     - **Result**:
       ```html
       <h1>Hello World</h1>
       ```

### 7. **Attributes**
   - Add attributes inside `[]`:
     - **Abbreviation**: `input[type="text" placeholder="Enter name"]`
     - **Result**:
       ```html
       <input type="text" placeholder="Enter name">
       ```

### 8. **Grouping Elements**
   - Use `()` to group elements:
     - **Abbreviation**: `div>(header>h1)+(main>p)+footer`
     - **Result**:
       ```html
       <div>
           <header>
               <h1></h1>
           </header>
           <main>
               <p></p>
           </main>
           <footer></footer>
       </div>
       ```

### 9. **Custom Repetition**
   - You can repeat elements and use `$` to indicate a counter:
     - **Abbreviation**: `ul>li.item$*3`
     - **Result**:
       ```html
       <ul>
           <li class="item1"></li>
           <li class="item2"></li>
           <li class="item3"></li>
       </ul>
       ```

### 10. **Comments**
   - **Abbreviation**: `!` inside tags creates comments.
   - **Example**: `<!-- This is a comment -->`

---