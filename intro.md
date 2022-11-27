NOTES LINK - [HERE](https://theprimeagen.github.io/fem-algos)

# Intro to Big 0

`Big O notation is a mathematical notation that describes the limiting behavior of a function when the argument tends towards a particular value or infinity.`

<br />
In our terms it measures how our program behaves when the input of the programs changes.

<br />

```
const arr = [1, 2, 3, 4, 5];
for (let i = 0; i < arr.length; i++) {
console.log(arr[i]);
}

for (let i = 0; i < arr.length; i++) {
console.log(arr[i]);
}

```

Always count the loops, as here there are 2 loops we say it has `2N` right? \
but thats not the case why?

In case of BIG 0 notation we always ignore the constants a good example why we do it is this...

In case where \
N = 1, 0(10N) = 10, O(N^2) = 1 \
N = 5, 0(10N) = 50, O(N^2) = 25 \
N = 100, 0(10N) = 1000, O(N^2) = 10000 \
N = 1000, 0(10N) = 100000, O(N^2) = 1000000

As the size of the input grows the the significance of the constant diminishes.

### Important Concepts

- Important concepts
- growth is with respect to the input
- Constants are dropped
- Worst case is usually the way we measure

### Different time of Big - 0 Complexity

![](https://theprimeagen.github.io/fem-algos/images/big-o-face.png)

More Examples

```
function sum_char_codes(n: string): number {
    let sum = 0;
    for (let i = 0; i < n.length; ++i) {
        for (let j = 0; j < n.length; ++j) {
            sum += charCode;
        }
    }

    return sum;
}
```

COUNT THE LOOPS!!!!! Loop inside a loop N^2 another loop inside? N^3 likewise...
