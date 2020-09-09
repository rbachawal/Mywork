# Motr Code Style

This document describes code style for the Motr repository. However, this document does not cover code documentation practices. The primary purpose of a code style guide is to make your code human readable and easy to understand. Thus, your code submissions have to be uniform and idiomaticcally similar to the human speech. The Motr code style guide is based on the [Linux Kernel Coding Style](https://www.kernel.org/doc/Documentation/process/coding-style.rst).

Use British English in documents, comments, and variable names.

- [Syntax](#Syntax)
  + [Indentation](#Indentation)
  + [Placement of braces and spacing](#Placement-of-Braces-and-Spacing)
  + [Breaking Long Lines and Strings](#Breaking-Long-Lines-and-Strings)
  + [Comments](#Comments)
  + [Variable Declarations](#Variable-Declarations)
- [Idioms](#Idioms)
  + [Loops](#Loops)
  + [Conventions](#Conventions)
    * [Name](#Name)
    * [typedefs](#Typedefs)
    * [Expr Size](#Expression-Size)
    * [Array Size](#Array-Size)
    * [Formatting](#Formatting)
   + [Things to Consider](#Things-to-Consider)
- [Code Organization Guidelines](#Code-Organization-Guidelines)

## Syntax

#### Indentation

- Tabs are 8 characters, hence indentations are 8 characters.
- Use tabs for block indentation.

#### Placement of Braces and Spacing

<details>
  <summary>Click to expand!</summary>
  <p>
    
- Do not add a blank space at the end of a line.
- You can omit braces around single statement blocks. The preferred way of placing braces, as ascertained by Kernighan and Ritchie, is to put the opening brace last on the line, and put the closing brace first:
        
  ```c
        
   if (condition) {
           branch0;
   } else {
           branch1;
   }

   func(arg0, arg1, ...);

   while (condition) {
                 body;
   }

   switch (expression) {
   case VALUE0:
   branch0;
   case VALUE1:
           branch1;
   ...
   default:
           defaultbranch;
   }
   ```
   
   </p>
   </details>
   
#### Breaking Long Lines and Strings

<details>
  <summary>Click to expand!</summary>
  <p>

- A continuous line starts a column after the last unclosed opening parenthesis.

  ```c
          
  M0_ASSERT(ergo(service != NULL,
                 m0_rpc_service_invariant(service) &&
                 service->svc_state == M0_RPC_SERVICE_STATE_INITIALISED));
  ```

- You should not begin a new line with an operator.

  ```c
          
  if (pl_oldrec->pr_let_id != stl->ls_enum->le_type->let_id ||
     pl_oldrec->pr_attr.pa_N != pl->pl_attr.pa_N) {
   }
  ```
  
  </p>
  </details>
 
#### Comments

Put the comments at the head of the function, telling people what it does, and possibly WHY it does it.
    
#### Variable Declarations

<details>
  <summary>Click to expand!</summary>
  <p>
    
- Align the identifier names and not the asterisks or type-declaration related decorations.
- This rule is applicable to block-level variable declarations as well.

     ```c
         
        struct foo {
              const char        *f_name;
              uint32_t           f_id;
              const struct bar  *f_bar;
              int                (*f_callback)(struct foo *f, int flag);
         };
     
     ```
     
     </p>
     </details>
     
## Idioms

It is essential that you adhere to these higher level idiomatic styles.

#### Loops

<details>
  <summary>Click to expand!</summary>
  <p>
    
- To write a loop that is repeated N times: 
        
  ```c
  
     for (i = 0; i < N; ++i) {
                          body;
     }

     or, if appropriate,

     m0_forall(i, N, body);   
  ```

- An infinite loop is written as:

  ```c
  while (1) {
        ...
  }
  ```
  </p>
  </details>

#### Conventions 

##### Name

<details>
  <summary>Click to expand!</summary>
  <p>
    
Add a short (1--4 characters) prefix to the struct and union member names. 

  ```c
  
  struct misc_imperium_translatio {
         destination_t mit_rome[3]; /* there shall be no fourth Rome */
         enum reason   mit_why;
  };
  ```

**Rationale:** Prefixes make searching for field names easier.

</p>
</details>

##### Typedefs 

Typedefs are used only for *scalar* data types. This includes function pointers and excludes enums. Compound data types like structs and unions, should never be aliased with a typedef.

`Names introduced by typedef end with '_t'`

##### Expression Size

<details>
  <summary>Click to expand!</summary>
  <p>
    
The size of expression is preferred to the size of type.

```c

   struct foo *bar = m0_alloc(sizeof *bar);
```

**Rationale:** Code changes remain impact when the bar type changes.  

</p>
</details>

##### Array Size 

<details>
  <summary>Click to expand!</summary>
  <p>
    
To iterate over indices of an array `X` use the `ARRAY_SIZE(X)` macro instead of explicitly writing the array size.

 ```c
 
     #define MAX_DEGREE_OF_SEPARATION (7)
     int degrees_of_separation[MAX_DEGREE_OF_SEPARATION + 1];
     for (i = 0; i < ARRAY_SIZE(degrees_of_separation); ++i) {
         body;
     }
  ```
**Rationale:** Use the Array_Size(X) macro to ensure that the code remains correct when the array declaration changes. 

:page_with_curl: **Note:** Always ensure that your code is autonomous to keep the code correct and consistent despite changes.

</p>
</details>

#### Formatting

<details>
  <summary>Click to expand!</summary>
  <p>
    
- Ensure that you differentiate NULL, 0, and `false` to emphasize a pointer, boolean, and integer—including code success or failure.

```c

   if (p == NULL) { /* assumes that p is a pointer */
   } else if (q != 0) { /* q is an integer */
   } else if (r) { /* r is a boolean */
   }
```
- Never use `if (r == true)`
- Use `?:` form of ternary operator—a gcc-extension like:
  `a ?: b ?: c ?: ...` - this expression will return the first non-zero value among a, b, c. 
  - Operands, including `a` can have any suitable type.
- Wherever possible, simplify.

  `return q != 0` - to return q and,
  `return expr ? 0 : 1` - to return !expr. Specifically, never use `(x == true)` or `(x == false)` instead of `(x)` or `(!x)` respectively.

   **Rationale:** If `(x == true)` is clearer than `(x)`. Then `((x == true) == true)` is even more clearer.
- Use `!!x` to convert a *boolean* integer into an *arithmetic* integer.
  - Use C99 bool type.

- Provide globally visible names.

  ```c
  
      struct m0_<module>_<noun> { ... }; /* data-type */
      int    m0_<module>_<noun>[size];   /* variable */
      int    m0_<module>_<noun>_<verb>(...); /* function */
      bool   m0_<module>_<noun>_is_<adjective>(...); /* predicate function */
   ```

- Static names don't have `m0_` prefix. 
- Function pointers within *operation structs* count as *static*. 
- Capitalize the names of constants. 
- Functions that are not static and globally exported, and shared only across multiple files within a module are prefixed with `m0_<module-name>__`. This rule applies to invariants as well.
- Use C99 designated initializers.

  ```c
  
     static const struct foo bar = { /* initialize a struct */
              .field0 = ...,
              .field1 = { /* initialize an array */
                       [3] = ...,
                       [0] = ...
               },
               ...
      };
  ```

- Avoid implicit field initialization using designated initializers.
  
  **Rationale:** This helps you to find all struct fields while letting you document the default value of a field.
- Use enums to define numerical constants.

  ```c
  
      enum LSD_HASHTABLE_PARAMS {
              LHP_PRIME   = 32416190071ULL,
              LHP_ORDER   = 11,
              LHP_SIZE    = 1 << LHP_ORDER,
              LHP_MASK    = LHP_SIZE - 1,
              LHP_FACTOR0 = 0.577215665,
              LHP_FACTOR1,
              LHP_FACTOR2
      };
   ```

  **Rationale:** enums as opposed to #defines, have types that are visible in a debugger.

- Prefer using inline functions over macros.
  
  **Rationale:** This is due to evaluation rules that perform type-checking and check for sane arguments.
- Prefer using Non-inline functions over inline functions, unless performance measurements show otherwise.

  **Rationale:** breakpoint can be placed within a non-inline function. Stack trace is more reliable with non-inline functions. Instruction cache pollution is reduced.
- Use macros only when you cannot achieve the end goal with other language constructs. 
  - While creating a macro ensure that you:
    - Evaluate arguments only once
    - Perform type-check. For more information, refer to the `min_t()` macro in the [Linux Kernel Coding Style](https://www.kernel.org/doc/Documentation/process/coding-style.rst)
    - Never affect control flow from a macro.
    - Capitalize the macro name.
    - Ensure that you correctly parenthesize a macro so that it works across any context.
    - Use the following return code conventions:
      - Return 1 for success
      - Use 0 for failure
      - Use positive values for other non failure conditions
- Use `const` for documentation and help for type-checker. 
  - Do not use casts to trick the type-checking system into believing your consts. 
    - A typical scenario is where the function doesn't let you modify its *input struct argument*, except for locking and unlocking within a struct. In this case, don't use *const*, instead, document why this argument isn't a constant.
- Ensure that the *control flow statement conditions* have no impacts.

  ```c
  
      alive = qoo_is_alive(elvis);
      if (alive) { /* rather than if (qoo_is_alive(elvis)) */
               ...
      }
  ```
  **Rationale:** with this convention statement coverage metric is more adequate.

- Use **C** precedence rules to omit noise in `_obvious_` expressions.

  `(left <= x && x < right)  /* not ((left <= x) && (x < right)) */` 
  
  But don't overdo it, for example: `(mask << (bits & 0xf)) /* not (mask << bits & 0xf) */`.

- Use assertions freely to verify state invariants. An asserted expression should have no impacts.
- Factor common code—always prefer creating a common helper function than copying code.

  **Rationale:** This helps you avoid duplication of bugs.
- Use standard scalar data type with explicit width, instead of using `long` or `int`. 
  **Example:**
  
  Use `int32_t, int64_t, uint32_t, uint64_t` - to represent 32-bits, 64-bits integers, unsigned 32-bits, and unsigned 64-bits integers respectively.
  
  **Rationale:** Lets you avoid inconsistent data structures on different arch.
- There is no comparison between signed and unsigned qualifiers without an explicit casting. The canonical order of type qualifiers in declarations and definitions is

  `{static|extern|auto} {const|volatile} typename`
  - when using long or long long qualifiers, use `omit int`.
  - Declare one variable per line.
  - Avoid bit-fields and use explicit bit manipulations with integer types.
  
    **Rationale:** This eliminates non-atomic access to bit-fields and implicit integer promotion.

- Avoid dead assignments and initializations—assignments that get overwritten before the variable is read.

  ```c
  
      int x = 0;
      if (y)
        x = foo();
          else
        x = bar();
    ```
  - Instead, initialize a variable with a meaningful value, when the latter is known.
  
    **Rationale:** Dead initializations potentially hide errors. If, after the code restructuring, the variable remains un-initialized in a conditional branch or in a loop that might execute 0 times, the initializer will suppress compiler warning.

- All header files should begin with `#pragma once`, followed by a conventional `#ifndef` include guard.

  ```c
  
      #pragma once
      #ifndef __MOTR_SUBSYS_HEADER_H__
      #define __MOTR_SUBSYS_HEADER_H__
      ...
      #endif /* __MOTR_SUBSYS_HEADER_H__ */
   ```

    Notice, that the include guards should use names conforming to the regular expression: `__MOTR_\w+_H__`. This is required for a build script which automatically checks for the correctness of include guards and reports duplicates.
- Specify invariants as a conjunction of positive properties that are reliable than using disjunction of exceptions. Use `m0_*_forall()` macros to build conjunctions over containers and sequences.
  - In invariants use `_0C()` macro to record a failed conjunct.
- A header file should include only headers that are necessary for the header to pass compilation. Use forward declarations instead of includes wherever possible. `.c` files should include all necessary headers directly, without relaying on headers that are already included. 
  - Do not include headers unnecessarily. 
  - If the included header is for a few definitions as opposed to the whole interface, these symbols should be mentioned in the comment on the `#include` line.
  
    **Rationale:** This reduces dependencies between modules, makes it easy to restructure the inclusion tree, and results in faster compilation faster.
- Use `M0_LOG()` from `lib/trace.h` instead of `printf(3)/printk()` in all source files which are part of `libmotr.so library` or `m0tr.ko module` helper utilities like UT, ST. Modules should use `printf(3)/printk())`.
  - Consider using `M0_LOG()` with meaningful information to describe important error conditions. Preferably, this should be included near the place where the error is detected.
  
    ```c
  
    reply = m0_fop_alloc(&m0_reply_fopt, NULL);
    if (reply == NULL)
               M0_LOG(M0_ERROR, "failed to allocate reply fop");
    ```

  - Try to describe the error using current and not the low-level context as this might already have been logged by the other func.
    
    **Example:** `failed to allocate memory` is not an ideal error message.
  - Choose an appropriate trace level for each `M0_LOG()`. You can find the general guidelines for the same at `lib/trace.h` - `m0_trace_level` enum documentation.
  - Consider using `M0_ENTRY()/M0_LEAVE()` at function's entry and exit points. 
  - Use `M0_RC()` and `M0_ERR_INFO()` to explicitly return from function, which conforms to the standard return code convention.
  - Leaf level error is an error returned by a non-Motr function. Wrap the error code for a *leaf level* function error that is initially produced by the function in `M0_ERR()`.

    ```c
    
        result = M0_ERR(-EFAULT);
        ...
        return M0_RC(result);
        or
        return M0_ERR(-EIO);
    ```
  - A non-leaf error should be reported rarely. This is to avoid artificial code complication arising out of reporting an error just for the sake of reporting. 
  
    **Example:**
    
    You can report no-leaf errors like:  
     
     ```c
     
        result = m0_foo(bar);
        if (result != 0)
                return M0_ERR(result);
     ```

     Avoid reporting errors like: 

     ```c
     
        result = m0_foo(bar);
        ...
        return result == 0 ? M0_RC(0) : M0_ERR(result);
     ```

      **Rationale:** Error reporting through `M0_ERR()` is important for log analysis. Reporting leaf errors is more important, because you can trace upward call-chain easily.
 
 </p>
 </details>

## Things to Consider

<details>
  <summary>Click to expand!</summary>
  <p>

- Locks should outlive the object(s) they are protecting. The code below illustrates a common mistake:

    ```c
  
    struct foo {
    ...
    /* Protects foo object from concurrent modifications. */
    struct m0_mutex f_lock;
    };
    int foo_init(struct foo * foo) {
    m0_mutex_init( & foo -> f_lock);
    m0_mutex_lock( & foo -> f_lock);
    /* ... Initialize foo ... */
    m0_mutex_unlock( & foo -> f_lock);
    }
    void foo_fini(struct foo * foo) {
    m0_mutex_lock( & foo -> f_lock);
    /* ... Finalize foo ... */
    m0_mutex_unlock( & foo -> f_lock);
    m0_mutex_fini( & foo -> f_lock); /* <--- Thread A */
    }
    int foo_modify(struct foo * foo, ...) {
    m0_mutex_lock( & foo -> f_lock);
    /* ... Modify field(s) of foo ... */
    /* <--- Thread B */
    m0_mutex_unlock( & foo -> f_lock);
    }
    ```
    
  Here it is possible that some thread (B) tries to unlock the mutex, which is already finalized by another thread (A). A general rule of thumb is that object creation and destruction should be protected by *existential lock(s)*, with a lifetime that's longer than that of the object.
  
  </p>
  </details>

## Code Organization Guidelines

<details>
  <summary>Click to expad!</summary>
  <p>

Traditional code organization techniques, taught in universities, include modularity, layering, information hiding, and maintaining abstraction boundaries. They tend to produce code, which is easy to modify and re-factor, and are, hence, very important. Their utility is highest in the projects that experience constant frequent modifications. Such projects or phases of projects cannot be long. In a long term project, where code lives around for many years, different considerations start playing an increasing role.

Consider an example, a stable project sees relatively infrequent addition of the new features, the most typical use of source code by a programmer in bug analysis. The programmer starts with a failure report, performance degradation, or test failure and looks through the area of code that is suspect. If the Programmer fails to find the problem  in this identified area, which is usually the case sice all obvious bugs are already ironed out: the programmer proceeds to the other modules, recursively.

There are two observations that can be drawn out of this:

1. Code is mostly read, not written. The stabler the project, the more predominant the reads. This is because its hard to find bugs that remain in the code and to fix one bug, one has to analyze many lines of code.
2. The assumption while reading the code is that it is incorrect.

The second point is contrary to the principles of information hiding and abstraction boundaries, where module A, which uses module B, is analyzed
under the assumption that neither of them has bugs. Abstraction boundary is not only not helpful, but directly detrimental, because every call from A to B has needs to be followed and one cannot rely on invariants. The more rigorous the abstraction, the more effort is spent jumping around the abstraction wall.

Over time, large and long term projects, such as Lustre and Linux kernel, have demonstrated that after a certain threshold, readability is at least as important as modifiability. In such projects, abstraction and modularity are properties of the software *design*. In turn, the code that is produced from the design, is optimised for long term readability. 

It is therefore necessary that you:

- Keep the code *visually* compact. The amount of code that is visible on the screen is very important. 
- Blank lines are a precious resource.
- Avoid using all forms of redundant Hungarian notations. For example, don't put information about parameters in the function name as parameters are already present at the call-site. 
  - A typical call for `m0_mod_call_with_bar()` would look like `m0_mod_call_with_bar(foo, bar)`. Not only is *bar* is redundant, it is also ill-favoured. Use a thesaurus for describing `call_with_x" vs. "call_with_y"`.
- Wrapping the field access in an accessor function is a gratuitous abstraction, which should be used sparingly and only if it makes code more compact. Field accesses have great properties such a side-effect freedom that are important for code analysis, which the function wrapper hides. 
  - Besides that, the C type system doesn't handle constants in this case, unless you have *two* wrappers.
- More generally, abstractions should be introduced for design purposes. For example, to mark a point of possible variability. 
  - Do not create Sub-modules, data-structures, and operation vectors to keep things small. 
  - Remember, in the long term, refactoring is easy.
