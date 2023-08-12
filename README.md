## Finding the total amount of blocks that needs to be moved

Let's say there are $n$ pistons. The $n^{th}$ piston will move $0$ blocks, the $(n-1)^{th}$ piston will move 1 block, the $(n-2)^{th}$ piston will move blocks. The block to be moved will be moved $n$ blocks. This means that the total number of blocks that needs to be displaced is : $$ToBeMoved(n)=n+(n-1)+(n-2)+ ... + 1$$
which is equivalent to $$ToBeMoved(n)=\frac{(n^{2} + n)}{2}$$
## Minimizing the number of extensions
To minimize the number of moves, the pistons should push 12 blocks whenever they can. Imagine $27$ pistons. The piston that can push the most amount of blocks is the $12^{th}$ one. Then the $24^{th}$ one. We can push the $27^{th}$ piston. We now have a line of $26$ pistons and have made $3$ extensions so far. We can start building a function that takes as input the number of pistons and outputs the number of extensions needed. $$f(n)=\lceil \frac{n}{12} \rceil + f(n-1)$$
The number added after each call in the recursion $\lceil \frac{n}{12} \rceil$ if you think about it. At $12$ pistons and less you can just push the last piston to go to the next call of the recursion. At $13$ through $24$ you need to do $2$ extensions and so on

## De-recursing the function
Calculating the recursion can be tidious by hand. To de-recurse it, think of the extensions that will be made.

Example for 27 (reads left to right then top to bottom):
```text
12 24 27
12 24 26
12 24 25
12 24
12 23
12 22
12 21
12 20
12 19
12 18
12 17
12 16
12 15
12 14
12 13
12
11
10
9
8
7
6
5
4
3
2
1
```

You can arrange these in blocks. There's a block with $12\cdot1$ extensions, a  block with $12 \cdot 2$ and a $3 \cdot 3$ block. This can be rewritten as $$f(27) = 3 \cdot 3 + 12(2 + 1)$$
More generally for $n$, this can be rewritten as 
$$f(n) = (n\ mod\ 12) \lceil \frac{n}{12} \rceil + 12 \cdot (\frac{(\lceil \frac{n}{12} \rceil - 1)\cdot \lceil\frac{n}{12} \rceil}{2})$$ 
$$f(n) = (n\ mod\ 12)\lceil \frac{n}{12} \rceil + 6\cdot(\lceil \frac{n}{12} \rceil^2-\lceil \frac{n}{12} \rceil)$$
$$f(n) = ((n\ mod\ 12) - 6)\lceil \frac{n}{12} \rceil + 6\lceil \frac{n}{12} \rceil^2$$
python3 code to generate the piston extension sequence would be 
```python
def piston_sequence(n):
	sequence = []
	for i in range(n, 0, -1):
		for j in range(12, i, 12):
			sequence.append(j)
		sequence.append(i)
	return sequence
```
