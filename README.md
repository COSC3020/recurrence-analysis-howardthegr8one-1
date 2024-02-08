[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/OlW38W4k)
# Recurrence Analysis -- Mystery Function

Analyze the running time of the following recursive procedure as a function of
$n$ and find a tight big $O$ bound on the runtime for the function. You may
assume that each operation takes unit time. You do not need to provide a formal
proof, but you should show your work: at a minimum, show the recurrence relation
you derive for the runtime of the code, and then how you solved the recurrence
relation.

```javascript
function mystery(n) {
    if(n <= 1)
        return;
    else {
        mystery(n / 3);
        var count = 0;
        mystery(n / 3);
        for(var i = 0; i < n*n; i++) {
            for(var j = 0; j < n; j++) {
                for(var k = 0; k < n*n; k++) {
                    count = count + 1;
                }
            }
        }
        mystery(n / 3);
    }
}
```

Add your answer to this markdown file. [This
page](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)
might help with the notation for mathematical expressions.

# My Answer

```javascript
function mystery(n) {
    if(n <= 1) // T(1)
        return;
    else {
        mystery(n / 3); // T(n/3)
        var count = 0;  // T(1)
        mystery(n / 3); // T(n/3)
        for(var i = 0; i < n*n; i++) {         // T(n^2)
            for(var j = 0; j < n; j++) {       // T(n)
                for(var k = 0; k < n*n; k++) { // T(n^2)
                    count = count + 1;         // T(1)
                }
            }
        }
        mystery(n / 3); // T(n/3)
    }
}
```
In the base case $T(1) = 1$, when not in the base case the recurrence relation for this function would be $T(n) = 3 \cdot T(n/3) + n^2 \cdot n \cdot n^2$ 

or

$T(n) = 3 \cdot T(n/3) + n^5$

$T(n/3) = 3(3 \cdot T(n/3/3) + (n/3)^{5}$

$= 9 \cdot T(n/9) + (n/3)^{5}$

$= 27 \cdot T(n/27) + (n/9)^{5}$

$= 81 \cdot T(n/81) + (n/27)^{5}$

Using this pattern we can see that our recurrence relation is:

$T(n) = 3^i \cdot T(n/3^i) + (n/i)^{5}$ where $i = \log_{3}n$, because each recursive call splits the array into thirds.

$T(n) = 3^{\log_{3}n} \cdot T(n/3 \cdot \log_{3}n) + (n/ \log_{3}n)^{5}$

$T(n) = n \cdot T(1) + (n/ \log_{3}n)^{5}$

$T(n) = n + (n / \log_{3}n)^{5}$

Ignoring lower order terms leaves us with 

$T(n) \in O(n^5)$

