# Handout #3

- Other forms of CFG
- Regular and non-regular languages
- Regular and non-regular CFGs
- Converting non-deterministic FAs to deterministic FAs

## Notations

- $S \to [a]$ means either $S \to a$ or $S \to \lambda$.
- $S \to \{a\}$ means $S \to a^*$.
- $S \to a | b$ means $S \to a$ or $S \to b$.

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];
	O [shape = point, width = 0]
	A -> O
}
```

## BNF (Backus-Naur Form)

CFG:

$$
\begin{aligned}
A &\to aA | bB | \lambda \\
B &\to bB | \lambda
\end{aligned}
$$

BNF:

$$
\begin{aligned}
A &\to aA \\
A &\to bB \\
B &\to bB \\
A &\to \lambda \\
B &\to \lambda
\end{aligned}
$$

Extended BNF: allows $[, ], |, \{, \}$

$$
\begin{aligned}
L &= a^* b^* \\
\text{EBNF: } S &\to \{a\} \{b\}
\end{aligned}
$$

### Examples

- $L = (a+b)^*$, EBNF: $S \to \{a|b\}$
- $L = a^* + b^*$, EBNF: $S \to \{a\} | \{b\}$

$L =a^* (b+c) c^*$

FA:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	A [label = "-", xlabel = A];
	C [label = "+", xlabel = C];

	A -> A [label = a];
	A -> C [label = b];
	A -> C [label = c];
	C -> C [label = c];
}
```

CFG:

$$
\begin{aligned}
A &\to aA | bC | cC \\
C &\to cC | \lambda
\end{aligned}
$$

EBNF:

$S \to \{a\} (b|c) \{c\}$

             --d-v
	S ------|    |-------->
	   ^-a-  --c-^   ^-c-

---

## Regular and non-regular languages

**Defn**: A CFG is regular if each rule is uniform.

CFG is not regular if on the right-hand side there are more than one
non-terminal.

## Non-deterministic FAs

Deterministic:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	A [label = " ", xlabel = A];
	B [label = " ", xlabel = B];

	A -> B [label = a];
}
```

Non-deterministic:

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	A [label = " ", xlabel = A];
	B [label = " ", xlabel = B];
	C [label = " ", xlabel = C];

	A -> B [label = a];
	A -> C [label = a];
	C -> B [label = b];
}
```

Program doesn't know whether to go to B or C.

### Conversion

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	X [label = "-", xlabel = X];
	Y [label = "+", xlabel = Y];
	T [label = " ", xlabel = T];

	X -> Y [label = a];
	X -> T [label = a];
	T -> Y [label = b];
	T:s -> T [label = b];
	Y:n -> Y [label = b];
}
```

```
Input/State		a       b
-----------------------------
0. {X}			{Y, T}  {}
1. {Y}			{}      {Y}
2. {T}			{}      {T, Y}
------------------------------
3. {Y, T}		{}      {Y, T}
```

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	0 [label = "-", xlabel = 0];
	3 [label = "+", xlabel = 3];

	0 -> 3 [label = a];
	3 -> 3 [label = b];
}
```

***

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	A [label = "±", xlabel = "{A}"];
	Y [label = "+", xlabel = "{Y}"];
	X [label = " ", xlabel = "{X}"];

	A -> A [label = a];
	A -> Y [label = a];
	A -> X [label = b];
	Y -> X [label = b];
	X -> Y [label = a];
	X -> X [label = a];
}
```

```
Input/State		a       b
-----------------------------
0. {A}			{A, Y}  {X}
1. {X}			{X, Y}  { }
2. {Y}			{ }     {X}
------------------------------
3. {Y, T}		{A, Y}  {X}
4. {X, Y}		{X, Y}  {X}
```

becomes

```graphviz
digraph {
	rankdir = LR;
	node [shape = circle;];

	A  [label = "±", xlabel = "{A}"];
	AY [label = "+", xlabel = "{A, Y}"];
	X  [label = "+", xlabel = "{X}"];
	XY [label = "+", xlabel = "{X, Y}"];

	A  -> AY [label = a];
	AY -> AY [label = a];
	A  -> X  [label = b];
	AY -> X  [label = b];
	X  -> XY [label = a];
	XY -> X  [label = b];
	XY -> XY [label = a];
}
```
