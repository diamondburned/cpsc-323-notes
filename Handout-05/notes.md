### Schedule

- This week (n)
  - ???
  - Passed homework
- Next week (n+1)
  - Lecture x2
- Week after (n+2)
  - Tuesday: Review
  - Thursday: Exam 1 part 1
- Week n + 3
  - Tuesday: Exam 1 part 2

# Handout 5

- Applications of removing $\lambda$ from FA
  - Given $L_1 = ab^*, L_2 = ba^*$, find $L_1 \cup L_2$

## Applications of removing $\lambda$ from FA

**Example**: Given $L_1 = ab^*, L_2 = ba^*$:

1. Find $L_1 \cup L_2$
2. Find $(L_1)(L_2)$
3. Find $L_1 \cap L_2$

#### Find $L_1 \cup L_2$

Suppose FA1 has language $L_1$ and FA2 has language $L_2$. To find $L_1 \cup
L_2$, we can construct a new FA that is FA1 $\cup$ FA2:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	1 [label = "-"];
	2 [label = "+"];
	3 [label = "-"];
	4 [label = "+"];
	0 [label = "-", color = red, fontcolor = red];

	1 -> 2 [label = "b"];
	2 -> 2 [label = "a"];

	3 -> 4 [label = "b"];
	4 -> 4 [label = "a"];

	0 -> 1 [label = "λ", color = red, fontcolor = red];
	0 -> 3 [label = "λ", color = red, fontcolor = red];
}
```

Then, construct a transition table for the new FA:

|         | $a$     | $b$     |
| ------- | ------- | ------- |
| $\{0\}$ | $\{2\}$ | $\{4\}$ |
| $\{1\}$ | $\{2\}$ | $\{\}$  |
| $\{2\}$ | $\{\}$  | $\{2\}$ |
| $\{3\}$ | $\{\}$  | $\{4\}$ |
| $\{4\}$ | $\{4\}$ | $\{\}$  |

Then, construct an FA from the transition table:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	2 [label = "+", xlabel = "{2}"];
	4 [label = "+", xlabel = "{4}"];
	0 [label = "-", xlabel = "{0}"];

	0 -> 2 [label = a]
	0 -> 4 [label = b]
	2:n -> 2 [label = b]
	4:s -> 4 [label = a]
}
```

Note that $\{1\}$ and $\{3\}$ are not reachable from $\{0\}$, so they are not
part of the language.

So

$$
\begin{aligned}
\text{FA} &= \text{FA}_1 \cup \text{FA}_2 & \\
L &= L_1 \cup L_2 \\
  &= ab^* + ba^*
\end{aligned}
$$

#### Find $(L_1)(L_2)$

Suppose 2 FAs. Make the new FA:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	0 [label = "-"];
	1 [label = " "];
	2 [label = " "];
	3 [label = "+"];

	0 -> 1 [label = "a"];
	1 -> 1 [label = "b"];
	1 -> 2 [label = "λ", color = "red", fontcolor = "red"];
	2 -> 3 [label = "b"];
	3 -> 3 [label = "a"];
}
```

To remove $\lambda$, we construct a transition table:

|            | $a$     | $b$                                         |
| ---------- | ------- | ------------------------------------------- |
| $\{0\}$    | $\{1\}$ | $\{\}$                                      |
| $\{1\}$    | $\{\}$  | $\{1, 3\}$ (keep traversing past $\lambda$) |
| $\{2\}$    | $\{\}$  | $\{3\}$                                     |
| $\{3\}$    | $\{3\}$ | $\{\}$                                      |
| $\{1, 3\}$ | $\{3\}$ | $\{1, 3\}$                                  |

Then, construct an FA from the transition table:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	0 [label = "-", xlabel = "{0}"];
	1 [label = " ", xlabel = "{1}"];
	3 [label = "+", xlabel = "{3}"];
	"1_3" [label = "+", xlabel = "{1, 3}"];

	0 -> 1 [label = a]
	1 -> "1_3" [label = b]
	"1_3" -> "1_3" [label = b]
	"1_3" -> 3 [label = a]
	3 -> 3 [label = a]
}
```

So

$$
\begin{aligned}
L &= (L_1)(L_2) \\
  &= abb^* + abb^* aa^* \\
  &= abb^* (\lambda + aa^*) \\
  &= abb^* a^* \\
  &= ab^* ba^*
\end{aligned}
$$

#### Find $L_1 \cap L_2$

$$
\begin{aligned}
L &= L_1 \cap L_2 = \{\} = \emptyset
\end{aligned}
$$

or

$$
\begin{aligned}
L &= L_1 \cap L_2 \\
  &= \text{FA}_1 \cap \text{FA}_2 \\
  &= \overline{\overline{\text{FA}_1 \cap \text{FA}_2}} \\
  &= \overline{\overline{\text{FA}_1} \cup \overline{\text{FA}_2}} \\
\end{aligned}
$$

Construct the FA for $\text{FA}_1$ and complete it:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	1 [label = "-"];
	2 [label = "+"];
	X [label = " ", color = red, fontcolor = red];

	1 -> 2 [label = "a"];
	2 -> 2 [label = "b"];
	1 -> X [label = "b", color = red, fontcolor = red];
	2 -> X [label = "a", color = red, fontcolor = red];
	X:n -> X [label = "a", color = red, fontcolor = red];
	X:s -> X [label = "b", color = red, fontcolor = red];
}
```

Then, construct the FA for $\overline{\text{FA}_1}$:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	1 [label = "±"];
	2 [label = " "];
	X [label = "+"];

	1 -> 2 [label = "a"];
	2 -> 2 [label = "b"];
	1 -> X [label = "b"];
	2 -> X [label = "a"];
	X:n -> X [label = "a"];
	X:s -> X [label = "b"];
}
```

Then, construct the FA for $\text{FA}_2$ and complete it:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	1 [label = "-"];
	2 [label = "+"];
	X [label = " ", color = red, fontcolor = red];

	1 -> 2 [label = "b"];
	2 -> 2 [label = "a"];
	1 -> X [label = "a", color = red, fontcolor = red];
	2 -> X [label = "b", color = red, fontcolor = red];
	X:n -> X [label = "a", color = red, fontcolor = red];
	X:s -> X [label = "b", color = red, fontcolor = red];
}
```

Then, construct the FA for $\overline{\text{FA}_2}$:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	3 [label = "±"];
	4 [label = " "];
	X [label = "+"];

	3 -> 4 [label = "b"];
	4 -> 4 [label = "a"];
	3 -> X [label = "a"];
	4 -> X [label = "b"];
	X:n -> X [label = "a"];
	X:s -> X [label = "b"];
}
```

Then, construct the FA for $\overline{\text{FA}_1} \cup \overline{\text{FA}_2}$:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	1 [label = " ", xlabel = "{1}"];
	2 [label = " ", xlabel = "{2}"];
	3 [label = "+", xlabel = "{3}"];
	4 [label = " ", xlabel = "{4}"];
	5 [label = " ", xlabel = "{5}"];
	6 [label = "+", xlabel = "{6}"];
	0 [label = "±", xlabel = "{0}"];

	0 -> 1 [label = "λ"];
	0 -> 4 [label = "λ"];

	1 -> 2 [label = "a"];
	2 -> 2 [label = "b"];
	1 -> 3 [label = "b"];
	2 -> 3 [label = "a"];
	3:n -> 3 [label = "a"];
	3:s -> 3 [label = "b"];

	4 -> 5 [label = "b"];
	5 -> 5 [label = "a"];
	4 -> 6 [label = "a"];
	5 -> 6 [label = "b"];
	6:n -> 6 [label = "a"];
	6:s -> 6 [label = "b"];
}
```

Then, construct the transition table:

|            | a          | b          |
| :--------- | :--------- | :--------- |
| $\{0\}$    | $\{2, 6\}$ | $\{3, 5\}$ |
| $\{1\}$    | $\{2\}$    | $\{3\}$    |
| $\{2\}$    | $\{3\}$    | $\{2\}$    |
| $\{3\}$    | $\{3\}$    | $\{3\}$    |
| $\{4\}$    | $\{6\}$    | $\{5\}$    |
| $\{5\}$    | $\{5\}$    | $\{6\}$    |
| $\{6\}$    | $\{6\}$    | $\{6\}$    |
| $\{2, 6\}$ | $\{3, 6\}$ | $\{2, 6\}$ |
| $\{3, 5\}$ | $\{3, 5\}$ | $\{3, 6\}$ |
| $\{3, 6\}$ | $\{3, 6\}$ | $\{3, 6\}$ |

Then, construct the FA using the transition table above:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	0 [label="±", xlabel="{0}"];
	26 [label="+", xlabel="{2, 6}"];
	35 [label="+", xlabel="{3, 5}"];
	36 [label="+", xlabel="{3, 6}"];

	0 -> 26 [label="a"];
	0 -> 35 [label="b"];

	26 -> 36 [label="a"];
	26:n -> 26 [label="b"];

	35 -> 36 [label="b"];
	35:s -> 35 [label="a"];
}
```

This is the FA for $\overline{\text{FA}_1} \cup \overline{\text{FA}_2}$. Let's
construct the complement of it:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	0 [label="-", xlabel="{0}"];
	26 [label=" ", xlabel="{2, 6}"];
	35 [label=" ", xlabel="{3, 5}"];
	36 [label=" ", xlabel="{3, 6}"];

	0 -> 26 [label="a"];
	0 -> 35 [label="b"];

	26 -> 36 [label="a"];
	26:n -> 26 [label="b"];

	35 -> 36 [label="b"];
	35:s -> 35 [label="a"];
}
```

There is no starting state in this FA, therefore this machine does not accept
any string. This is $\overline{\overline{\text{FA}_1} \cup
\overline{\text{FA}_2}}$.
