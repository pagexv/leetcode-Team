## What is Python

High level (cannot understood by machine) interpreted (during execution each line is executed into machine language on the go) languages.

It is featured for the readability and efficiency. Python is widely used in

- Web application
- Artificial Intelligence
- Data Science
- Image Processing
- Operating Systems

## First Code (print)

Input

> print(50, 1000, 3.142, "Hello World")

Output

```
50 1000 3.142 Hello World
```

By default print statement prints texts in a new line

> print("Hello")
>
> print("World")

Output

```
Hello 
World
```

But you could use **end = " "** to print in one line

Input

> print("Hello", end=" ")
>
> print("World")

Output

```
Hello World
```

## Data Types

Python has three main data types

- Numbers

  - Integers

    - 0 takes 24 bytes, 1 takes 28 bytes of memory
    - Eg. 5

  - Floating point number

    - A float takes 24 bytes of memory
    - Eg. 5.0

  - Complex number

    - A complex number usually takes 32 bytes of memory

    - Usually expressed as complex(real, imaginary)

    - ```python
      print(complex(10,20))
      >(10+20j)
      ```

    - Note that Python follows electrical engineering convention. The imaginary part is denoted by j

  - 

- Boolean

  - The first letter of bool needs to be capitalized

- String

  - A string is a collection of characters closed within single, double or triple quotation marks

  - Accessing characters using []

  - Indexing

    ```python
    batman = "Bruce Wayne"
    
    first = batman[0]  # Accessing the first character
    print(first) # output B
    space = batman[-1] # output e
    ```

  - Reversing Indexing

    ```python
    batman = "Bruce Wayne"
    print(batman[-1])  # Corresponds to batman[10] -> e
    print(batman[-5])  # Corresponds to batman[6] -> W
    ```

  - String Immutability

    - Once we assign a value to a string, we can't update it later

      ```python
      string = "Immutability"
      string[0] = 'O' # Will give error
      ```

  - Unicode

    - All strings are unicode in Python 3.x

    - Python 2.x only supports ASCII, the Unicode in Python 2.x

      ```
      string = u"This is unicode"
      ```

  - Slicing a String

    - `string[start:end]` or `string[start:end:step]`

    - ```python
      my_string = "This is MY string!"
      print(my_string[0:7])  # A step of 1 => This is
      print(my_string[0:7:2])  # A step of 2 => Ti s
      print(my_string[0:7:5])  # A step of 5 => Ti
      
      ```

  - Reversing Slicing

Input: 

> multiple_lines = '''Triple quotes allows
>
> multi-line string.'''
>
> print(multiple_lines)

Output

```
Triple quotes allows
multi-line string.
```

