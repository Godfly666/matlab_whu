## Problem:

Fill in _ _ _ _ * _ = _ _ _ _ using digit from 1 to 9. Repeat not allowed.

### Method 1:

Generate the permutation by the built-in function `perms()`, and see if each permutation satisfies the equation.

For example, `perm_all = perms([1, 2, 3])` generate all permutations of `1, 2, 3`, i.e., `[[1 2 3];[1 3 2];[2 1 3];[2 3 1];[3 1 2];[3 2 1]]`.

#### Source Code:

```matlab
disp("The required numbers are:");
tic;
rst = main_method1();
disp(rst);
toc;

%{
    input: none
    output: n by 3 matrix containing all the solutions
%}
function rst = main_method1()
    rst = [];
    perm_all = perms([1, 2, 3, 4, 5, 6 ,7, 8, 9]);
    for i = 1 : size(perm_all, 1)
        perm = perm_all(i, :);
        a = from_digit_4(perm(1 : 4));
        m = perm(5);
        b = from_digit_4(perm(6 : 9));
        if (a * m == b)
            rst = [rst; a, m, b];
        end
    end
end

%{
    description: return a four-digit decimal number given an array of its digits.
    input: a row vector of length 4
    output: a four-digit decimal number
%}
function rst = from_digit_4(digits)
    x3 = digits(1); x2 = digits(2); x1 = digits(3); x0 = digits(4);
    rst = x3 * 1000 + x2 * 100 + x1 * 10 + x0;
end
```



### Method 2:

Iterate through 1-9 and all four-digit numbers, multiply them, and see if the result has any repeated numbers.

#### Source Code

```matlab
disp("The required numbers are:");
tic;
rst = main_method2();
disp(rst);
toc;

%{
    description: solution, method 2
    input: none
    output: n by 3 matrix containing all the solutions
%}
function rst = main_method2()
    rst = [];
    for m = 2 : 9  % 1 is impossible
        for a = 1234 : 4999  % 2 * 5000 = 10000
            [a3, a2, a1, a0] = get_digit_4(a);
            
            if ismember(0, [a3, a2, a1, a0])  % exclude the digit 0
                continue
            end
            
            b = m * a;
            [b3, b2, b1, b0] = get_digit_4(b);
            
            if ismember(0, [b3, b2, b1, b0])  % exclude the digit 0
                continue
            end
            
            lst = [a3, a2, a1, a0, m, b3, b2, b1, b0];
            uniq = length(lst) - length(unique(lst));  % to check if there is no repetition
            
            if uniq == 0  % if there is no repetition
                rst = [rst; a, m, b];  % append the solution
            end
        end
    end
end

%{
    description: get the digit of a four-digit decimal number
    input: a four-digit decimal number ranged from 1234 to 9999
    output: a row vector of length 4 ([x3, x2, x1, x0])
%}
function [x3, x2, x1, x0] = get_digit_4(num)
    % robustness
    if (num > 9999 || num < 1234)
        x3 = -1; x2 = -1; x1 = -1; x0 = -1;
        return
    end
    
    x3 = floor(num / 1000);
    num = num - 1000 * x3;
    x2 = floor(num / 100);
    num = num - 100 * x2;
    x1 = floor(num / 10);
    x0 = num - 10 * x1;
end
```



