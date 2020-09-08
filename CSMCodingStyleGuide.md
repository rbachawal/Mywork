# Coding Guidelines

## Typescript

- File-name should be in kebab-case. 

  **Example:** **single-file**.
- Every file should have a Header with Author and other details.
- Components
  - 1 file per logical component (e.g. parser, scanner, emitter, checker).
  - Use PascalCase for type names/Component Name.
  - Use CapitalCASE for enum values.
  - Use camelCase for function names.
  - Use camelCase for property names and local variables.
  - Use whole words in names when possible.
  - Add a Comment for functions which are performing complex logic
  - Variable names should be meaningful
  - Single line code should not have more than 80 chars. If required break line.
- Types
  - Do not export types/functions unless you need to share it across multiple components.
  - Do not introduce new types/values to the global namespace.
  - Within a file, type definitions should come first.
- Use undefined. Do not use null.
- Comments
  - Every function which has complex logic or initiates business logic should have detailed comment
  - Use JSDoc style comments for functions, interfaces, enums, and classes.
- Style
  - Use arrow functions over anonymous function expressions.
  - Only surround arrow function parameters when necessary.
    - Example: Instead of `(x) => x + x`, you can use: 
      - `x => x + x`
      - `(x,y) => x + y`
      - `<T>(x: T, y: T) => x === y`
  - Always surround loop and conditional bodies with curly braces. Statements on the same line are allowed to omit braces.
  - Open curly braces always go on the same line as whatever necessitates them.
  - Parenthesized constructs should have no surrounding whitespace. A single space follows commas, colons, and semicolons in those constructs. 
    **Example:**
    
    ```javascript
    for (var i = 0, n = str.length; i < 10; i++) { }
    if (x < 10) { }
    function f(x: number, y: string): void { }
    ```
   
  - Use a single declaration per variable statement. Use `var x = 1; var y = 2; over var x = 1, y = 2;`.
  - `else` goes on a separate line from the closing curly brace.
  - Use 4 spaces per indentation.

## [Vue.JS](https://vuejs.org/v2/style-guide/#Single-file-component-filename-casing-strongly-recommended)

- Components that should only ever have a single active instance should begin with `The` prefix. 
- Child components that are tightly coupled with their parent should include the parent component name as a prefix. In general, we should avoid such instances.
- Component names should always be PascalCase in single-file components and string templates - but kebab-case in DOM templates.
- Elements with multiple attributes should span multiple lines, with one attribute per line.

  ```javascript
  <MyComponent
  foo="a"
  bar="b"
  baz="c"
  />
  ```
- Component templates should only include simple expressions, with more complex expressions refactored into computed properties or methods.
- Complex computed properties should be split into as many simpler properties as possible.
- Use Single Quotes for String.
- Directive shorthands `:` for `v-bind:`, `@` for `v-on:` and `#` for `v-slot` can be used always or never.

## [Python](https://www.python.org/dev/peps/pep-0008/)

### File Names

- All small case letters in the file name
- Separator: “_” 
- **Example:** `some_file.py`

### Indentation

- Use 4 spaces per indentation level.
- Continuation lines should align wrapped elements either vertically using Python's implicit line joining inside parentheses, brackets, and braces, or using a hanging indent.
- Add 4 spaces - an extra level of indentation to distinguish arguments from the rest.
  
  ```python
  def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)
    ```

- Hanging indents should add a level.

  ```python
    foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
  ```

- Line length restriction to be 100 characters.
- Break Characters before binary operators, after comma and Brackets
- Use ‘...’  by default for strings.

### Imports

- Imports should usually be on separate lines.
  
  **Incorrect method:** `import sys, os, time` 
  **Correct method:**
  
  ```python
  import sys
  import os
  import time
  ```

- Imports are always put at the top of the file, just after any module comments and docstrings, and before module globals and constants.
- Imports should be grouped in the following order:
  - Standard library imports.
  - Related third party imports.
  - Local application/library specific imports.
- You should put a blank line between each group of imports.

### Docstrings

- It is mandatory to have comments/docstrings attached to each and every function except for @property or equivalent. 
- Docstrings should be informative.
- Follow the following Format 

    ```python
    def complex(real=0.0:float, imag=0.0: float) -> str:
    
    """
    Text description of what function does

       :param real:Real Number  
       :param imag:  imaginary number 
       :return:  Returns Complex value 
     """
     
       if imag == 0.0 and real == 0.0:
       return complex_zero
    ```

### Typings

Follow [3.6 Typings in Python](https://www.python.org/dev/peps/pep-0484/) to define function and methods as it enables easy understanding of the data fetched in a function

```python
def greeting(name: str) -> str:
return 'Hello ' + name
```

### String Concatenation

[Python 3.6](https://www.python.org/dev/peps/pep-0498/) supports `f` string hence using `f` strings format is preferred than adding two strings as they create an extra object. 

**Example:**

```python
import datetime
name = 'Fred'
age = 50
anniversary = datetime.date(1991, 10, 12)
```

**Example:**

```python
f'My name is {name}, my age next year is {age+1}, my anniversary is {anniversary:%A, %B %d, %Y}.'
```

**Example:**

```python
'My name is Fred, my age next year is 51, my anniversary is Saturday, October 12, 1991.'
```

**Example:**

```python
f'He said his name is {name!r}.'
```








