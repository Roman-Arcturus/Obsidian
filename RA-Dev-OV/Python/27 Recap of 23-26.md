
> [!prompt]
> I have reviewed our previous lessons and retested all of the examples and exercises:
> - Nested Data Structures
> - List Comprehensions
> - Advanced Comprehensions
> - Lambda and Key expression
> 
> My points:
> 1. A lot of confusion was caused by the long lines with mixed expressions. For the foreseeable future, I will use expressions that span several lines:
> 	result = [
> 	    expression_if_true
> 	    if condition
> 	    else expression_if_false
> 	    for element in iterable
> 	]
> - Please use this form for clarity.
> 
> 1. Some confusion was introduced by the unclear types of the resulting data, such as that resulting from filter() and map(), which do not return a list but some kind of an Object.
> - Please specify what types of data are input and output. (My previous experience has emphasized the importance of avoiding data mistyping, even with weakly and dynamically typed languages.)
> 


1. **Multi-line comprehensions**  
    Using multi-line formatting is the _Pythonic readability standard_ for anything non-trivial. I will format all future examples exactly that way.
    
2. **Explicit input/output types**  
    This is an excellent habit.  
    In professional Python codebases, engineers often annotate everything with type hints 
    precisely to avoid implicit “what does this return?” confusion.  
    
    I will now explicitly state:
    - Input data types
    - Output data types
    - Whether a function mutates or not
    - Whether a Python built-in returns a list, iterator, or generator

This will keep your mental model clean and consistent.

Now we continue the lesson.

Next Topic: Higher-Order Functions and Functional Pipelines in Python