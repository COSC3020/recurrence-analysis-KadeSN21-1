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
I relaized that the first and last for loops are running $n^2$ times, and updated my reccurence relation to reflect $n^2 * n * n^2 = n^5$.
$$
T(n) = 
\begin{cases} 1 & \text{if } n \leq 1,\\
3T\left(\frac{n}{3}\right) + \mathcal{O}(n^5) & 
\text{if } n > 1\end{cases}
$$


We can solve this by substitution:
First level:

$$
T(n) = 3T\left(\frac{n}{3}\right) + (n^5)
$$

Second Level:

$$
9T\left(\frac{n}{9}\right) + \left(\frac{n}{3}\right)^5 + n^5
$$

Third level:

$$
27T\left(\frac{n}{27}\right) + \left(\frac{n}{9}\right)^5 + \left(\frac{n}{3}\right)^5 + n^5
$$

This pattern is similar to the one that you solved on slide 22, however it includes a summation, so the general form is as follows:

$$
3^iT\left(\frac{n}{3^i}\right) + \sum_{k=0}^{i-1}\left(\frac{3^k}{3^{5k}}\right)*n^5
$$

If we plug in $log(n)$ for $i$, we have:

$$
3^{log(n)}T\left(\frac{n}{3^{log(n)}}\right) + \sum_{k=0}^{log(n)-1}\left(\frac{3^k}{3^{5k}}\right)*n^5
$$

$$
= nT(1) + (\left(\frac{3^{log(n) - 1}}{3^{5(log(n) - 1)}}\right) + 1) * n^5
$$

Dropping the constant factor $nT(1)$ we now have:

$$
(\left(\frac{3^{log(n) - 1}}{3^{5(log(n) - 1)}}\right) + 1) * n^5
$$

Simplifying:

$$
n^5 * \left(\frac{3^{log(n) - 1}}{3^{5(log(n) - 1)}}\right) + n^5
$$

Since $\left(\frac{3^{log(n) - 1}}{3^{5(log(n) - 1)}}\right)$ aproaches 0 as $n$ approaches $\infty$, $n^5$ dominates this relation, thus $n^5 \in \mathcal{O}(n^5)$


