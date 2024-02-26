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
In the base case $T(1) = 1$, when not in the base case the recurrence relation for this function would be $T(n) = 3 \cdot T(n/3) + n^{2} \cdot n \cdot n^{2}$, or in other terms:

$$\begin{equation}T(n)=\begin{cases}1, & \text{if $n \leq 1$} \\
3T(\frac{n}{3})+n^5+C & \text{if $n>1$}\end{cases}\end{equation}$$

$T(n) = 3T(\frac{n}{3}) + n^{5} + C$

$T(\frac{n}{3}) = 3(3T(\frac{n}{3}) + n^{5} + C) + n^{5} + C$

$= 9T(\frac{n}{9}) + 3(\frac{n}{3})^{5} +3C + n^{5} + C$

$= 9T(\frac{n}{9}) + \frac{n^5}{3^4} + n^5 + 4C$

$T(\frac{n}{3}) = 3(9T(\frac{n/9}{3}) + \frac{n^5/3^4}{3} + (\frac{n}{3})^5 + 4C) + n^5 + C$

$= 27T(\frac{n}{27}) + 3(\frac{n^5}{3^5}) +(\frac{n^5}{3^5}) +9C + n^5 + C$

$= 27T(\frac{n}{27}) + (\frac{n^5}{3^4}) + (\frac{n^5}{3^5}) +n^5 + 10C$

$T(n) = 3^iT(\frac{n}{3^i})$ $+ \sum \limits_{j=0}^{i-1} \frac{n^5}{3^{4j}} +$ some constant $C$

I've ommitted the math for the constant $C$ as this won't affect our runtime asymptotically. Since this relation involves 3 recursive calls where each call uses $\frac{n}{3}$ we know our $i = \log_{3}n$. Subbing this into our relation gives us: 

$T(n) = 3^{\log_{3}n}T(\frac{n}{3^{\log_{3}n}}) + \sum\limits_{j=0}^{\log_{3}n-1}\frac{n^5}{3^{j}} + C$

$= n \cdot T(n/n) + \frac{n^5}{3^{\log_{3}n-1}} + C$

$= n \cdot 1 + n^5 \cdot \frac{1}{3^{\log_{3}n-1}} + C$

Ignoring constants and lower order terms leaves us with the following runtime complexity:

$T(n) \in O(n^5)$
