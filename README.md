# Basic Unpack Toolkit

## What Is It and What Does It Do?
  It is an alternative toolkit to `functools.reduce` -though there are lots of differences between them-. It enables you to unpack iterators without the usage of asterisk and increases readibility level (for those who do not like using an asterisk).
## Installation
  Windows: `py -m pip install unpacktools`
  Ubuntu/Linux: `python3 -m pip install unpacktools`
## Features
*Note: To work with the examples below, you should import the package as: `from unpacktools import *`*
* You can pass unpack objects to 'handled' functions. To handle a single function, you can use a decorator:
  ```python3
  @func_handler
  def foo(a, b, c, d) -> "a + b + c + d" :
      return a + b + c + d
  ```
  Now, check if it works:
  ```python3
  result0 = foo(1, 2, 3, 4) # Please note that it is not not a requirement to pass unpack objects to your handled-or decorated- function. 
  #'Resolving unpack objects' is just an *extra* feature added to your function.
  result1 = foo( unpack([1,2,3,4]) )
  result2 = foo( unpack([ 1,2, unpack([3,4]) ]) ) #Deep `unpack`s are allowed and works fine.
  result3 = foo( unpack([1,2]),3,4 ) #You can use a mixture of unpack and other kind of objects.

  print(result0, result1, result2, result3) # 10, 10, 10, 10
  ```
* You can also handle multiple functions at one time using `multi_func_handler`. Please note that it returns a dictionary by default(You can change the argument "rt_type").
  ### Usage-1 :
    ```python3
    def foo(a, b, c, d) -> "a + b + c + d" :
        return a + b + c + d
        
    def bar(x, y, z) -> "x * y * z":
        return x * y * z
        
    foo, bar = multi_func_handler(foo, bar, rt_type = tuple )
    print( foo( unpack([1,2,3,4]) ) )
    print( bar( unpack([1,2,3]) ) )
    ```
  ### Usage-2 :
    ```python3
    def foo(a, b, c, d) -> "a + b + c + d" :
        return a + b + c + d
        
    def bar(x, y, z) -> "x * y * z":
        return x * y * z
        
    globals().update( multi_func_handler(foo, bar) ) #You can also use locals() instead of globals()
    ```
* You can handle a class and then, you will be able to pass unpack objects to its methods as arguments :
  ```python3
  @class_handler
  class Foo:
      #Your code goes here
  ```
* You can handle multiple classes at one time using `multi_class_handler`. (This "multi-handler" also returns a dict as default.)
  ### USAGE-1:
    ```python3
    class Foo:
        pass
    class Bar:
        pass
    Foo, Bar = multi_class_handler(Foo, Bar, rt_type = tuple )
    ```
  ### USAGE-2:
    ```python3
    class Foo:
        pass
    class Bar:
        pass
    globals().update( multi_class_handler(Foo, Bar) ) #You can also use locals() instead of globals()
    ```
* Lazy? You can handle the whole scope with `scope_handler`. Arguments: `(scope, funcs = True, classes = True, dunders = False)`. Usage:
  ```python3
  def foo_bar(x,y,z):
      pass
      
  class Foo:
      pass
      
  class Bar:
      pass
  
  globals().update( scope_handler(globals()) )
  ```
  Note: `dunders` include everything starts with an underscore.
