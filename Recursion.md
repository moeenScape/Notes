# Recursion in Programming: Understanding with Factorial Example

## Introduction
Recursion is a programming technique where a function calls itself to solve a problem. This method is particularly useful for problems that can be broken down into smaller, similar subproblems. A recursive function typically has two main parts:
1. **Base Case**: The condition under which the function stops calling itself, preventing infinite recursion.
2. **Recursive Case**: The part where the function calls itself with modified arguments, moving towards the base case.

## Example: Factorial Calculation

The factorial of a number n is the product of all positive integers less than or equal ton. It is denoted as n!.

### Mathematical Definition
- \( 0! = 1 \) (by definition)
- \( n! = n \times (n-1)! \) for \( n > 0 \)

### Recursive Function for Factorial

Here's the recursive function to calculate the factorial in Java:

```java
public class FactorialExample {
    // Recursive function to calculate factorial
    public static int factorial(int n) {
        // Base case: if n is 0, return 1
        if (n == 0) {
            return 1;
        }
        // Recursive case: n * factorial of (n-1)
        return n * factorial(n - 1);
    }

    public static void main(String[] args) {
        int number = 5; // Example number
        int result = factorial(number);
        System.out.println("Factorial of " + number + " is " + result);
    }
}
```

# Step-by-Step Execution (Debugging)

## Let's go through the execution of `factorial(5)` step-by-step:

1. **Initial Call**: `factorial(5)`
    - Check if `n == 0`: No, so proceed to the recursive case.
    - Calculate: `5 * factorial(4)`

2. **Recursive Call**: `factorial(4)`
    - Check if `n == 0`: No, so proceed to the recursive case.
    - Calculate: `4 * factorial(3)`

3. **Recursive Call**: `factorial(3)`
    - Check if `n == 0`: No, so proceed to the recursive case.
    - Calculate: `3 * factorial(2)`

4. **Recursive Call**: `factorial(2)`
    - Check if `n == 0`: No, so proceed to the recursive case.
    - Calculate: `2 * factorial(1)`

5. **Recursive Call**: `factorial(1)`
    - Check if `n == 0`: No, so proceed to the recursive case.
    - Calculate: `1 * factorial(0)`

6. **Base Case Reached**: `factorial(0)`
    - Check if `n == 0`: Yes, return `1`.

## Backtracking and Computing Results

After reaching the base case, the recursion starts to unwind, and we compute the results step-by-step:

1. **Backtracking**: `factorial(1)`
    - Result from `factorial(0)` is `1`.
    - Compute: `1 * 1 = 1`
    - Return `1`

2. **Backtracking**: `factorial(2)`
    - Result from `factorial(1)` is `1`.
    - Compute: `2 * 1 = 2`
    - Return `2`

3. **Backtracking**: `factorial(3)`
    - Result from `factorial(2)` is `2`.
    - Compute: `3 * 2 = 6`
    - Return `6`

4. **Backtracking**: `factorial(4)`
    - Result from `factorial(3)` is `6`.
    - Compute: `4 * 6 = 24`
    - Return `24`

5. **Backtracking**: `factorial(5)`
    - Result from `factorial(4)` is `24`.
    - Compute: `5 * 24 = 120`
    - Return `120`

## Visualization
```scss
factorial(5)
 -> 5 * factorial(4)
    -> 4 * factorial(3)
       -> 3 * factorial(2)
          -> 2 * factorial(1)
             -> 1 * factorial(0)
                -> 1 (base case)
             <- 1 * 1 = 1 (returns 1)
          <- 2 * 1 = 2 (returns 2)
       <- 3 * 2 = 6 (returns 6)
    <- 4 * 6 = 24 (returns 24)
 <- 5 * 24 = 120 (returns 120)

```

## Summary of Execution Flow

1. **Initial Call**: The function is called with `n = 5`.
2. **Recursive Calls**: The function keeps calling itself with `n` decremented by `1` each time until it reaches the base case (`n = 0`).
3. **Base Case**: When `n = 0`, the function returns `1`.
4. **Backtracking**: The recursive calls start returning their results, and each call computes its value based on the result of the previous call.
