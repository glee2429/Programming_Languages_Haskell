# Type Inference (To be covered in Quiz 1 on 10/21)

### How to determine type classes?

Given e1 and e2, e1 is the function that takes an argument e2.
Therefore, e1 should be T1 (type 1) that takes argument and gives a result.

#### Unification Process:
1. Check if the type of e2 is equivalent to the type of e1's argument.
2. Additionally, the type of the result should be equivalent to the outcome of e1e2.
3. This unification step is particularly tricky. However, it's heavily used in type inference.

If there's any of these steps fails, a type error occurs.


#### | HINT: Find the *top level* application
*Top level* application is the one evaluated *the last*.


Case 1 [O] --> Correct bottom-up style
```Haskell
      @
     /  \
    @    3
   / \
not  even
```
--> not even 3


Case 2 [X] --> Incorrect bottom-up style
```Haskell
     @
    / \
  not  @
      / \
    even 3
```
--> not (even 3)




### Type Inference Exercise:

Q1
```Haskell
Prelude> :t Just
Just :: a -> Maybe a
```

-- "Just" data constructor takes a and returns "Maybe" data type a

Q2
```Haskell
Prelude> :t not even 3

<interactive>:1:1: error:
    • Couldn't match expected type ‘t0 -> t’ with actual type ‘Bool’
    • The function ‘not’ is applied to two arguments,
      but its type ‘Bool -> Bool’ has only one
      In the expression: not even 3

<interactive>:1:5: error:
    • Couldn't match expected type ‘Bool’ with actual type ‘a0 -> Bool’
    • Probable cause: ‘even’ is applied to too few arguments
      In the first argument of ‘not’, namely ‘even’
      In the expression: not even 3
```
--- | HINT: Which one is the *top level* application?
-- *top level* application is the one evaluated *the last*.

```Haskell
      Case 1: not even 3

            @
           /  \
          @    3
         / \
      not  even


      Case 2: not (even 3)
           @
          / \
        not  @
            / \
          even 3
```

--> Unification Fails


Q3
```Haskell
Prelude> :t not (even 3)
not (even 3) :: Bool
```
- LHS: Int -> Bool
- RHS: Int ->(?) Int [Match], Bool -> Bool [Match]

Q4 Function Composition Example
```Haskell
Prelude> :t not . even

--- We can change this into
--- ((.)) not even :t 
not . even :: Integral a => a -> Bool
```

Q5
```Haskell
Prelude> :t even . not

<interactive>:1:1: error:
    • No instance for (Integral Bool) arising from a use of ‘even’
    • In the first argument of ‘(.)’, namely ‘even’
      In the expression: even . not
```
Q6
```Haskell
Prelude> :t map (Just . even)
map (Just . even) :: Integral a => [a] -> [Maybe Bool]
```
Q7
```Haskell
Prelude> :t map
map :: (a -> b) -> [a] -> [b]
-- argument is (a -> b)
-- result is [a] -> [b]
```

Q8
```Haskell
Prelude> :t even
even :: Integral a => a -> Bool
```

Q9
```Haskell
Prelude> :t map even
map even :: Integral a => [a] -> [Bool]
```
