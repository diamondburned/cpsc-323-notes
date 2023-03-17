# Handout 7

**Continuation of Handout 6:**

$$
\begin{aligned}
E &\to E + T \\
E &\to E - T \\
E &\to T \\
\end{aligned}
\ \underset{
\begin{aligned}
Q &= E' \\
\beta &= T \\
\alpha &= +T \\
\alpha &= -T
\end{aligned}
}{\longrightarrow} \
\begin{aligned}
E &\to TQ \\
Q &\to +TQ \\
Q &\to -TQ \\
Q &\to \lambda \\
\end{aligned}
$$

$$
\begin{aligned}
T &\to T * F \\
T &\to T / F \\
T &\to F \\
\end{aligned}
\ \underset{
\begin{aligned}
R &= T' \\
\alpha &= *F \\
\alpha &= /F \\
\beta &= F
\end{aligned}
}{\longrightarrow} \
\begin{aligned}
T &\to F R \\
R &\to *FR \\
R &\to /FR \\
R &\to \lambda \\
\end{aligned}
$$

$$
\begin{aligned}
F &\to (E) \\
F &\to a \\
\end{aligned}
$$

|      | $\text{FIRST}$  | $\text{FOLLOW}$     |
| ---- | --------------- | ------------------- |
| $E$  | $\{(, a\}$      | $\{\$, ) \}$        |
| $Q$  | $\{+, -, \lambda\}$ | $\{\$, ) \}$        |
| $T$  | $\{(, a\}$      | $\{+, -, \$, ) \}$ |
| $R$  | $\{*, /, \lambda\}$ | $\{+, -, \$, ) \}$ |
| $F$  | $\{(, a\}$      | $\{+, -, \$, *, /, ) \}$ |

<br>

To find the entries of table 2, we use these rules:

1. If $A \to B$, for every $b \in \text{FIRST}(B)$, $[A, b] = B$
2. If $A \to \lambda$, for every $a \in \text{FOLLOW}(A)$, $[A, a] = \lambda$

So:

$$
\begin{aligned}
&E \to TQ,& &\text{FIRST}(TQ) = \text{FIRST}(T) = \{(, a\}& &\therefore [E, (] = [E, a] = TQ \\
&Q \to +TQ,& &\text{FIRST}(+TQ) = \{+\}& &\therefore [Q, +] = +TQ \\
&Q \to -TQ,& &\text{FIRST}(-TQ) = \{-\}& &\therefore [Q, -] = -TQ \\
&T \to FR,& &\text{FIRST}(FR) = \text{FIRST}(F) = \{(, a\}& &\therefore [T, (] = [T, a] = FR \\
&R \to *FR,& &\text{FIRST}(*FR) = \{*\}& &\therefore [R, *] = *FR \\
&R \to /FR,& &\text{FIRST}(/FR) = \{/\}& &\therefore [R, /] = /FR \\
&F \to (E),& &\text{FIRST}((E)) = \{(\}& &\therefore [F, (] = (E) \\[1em]

&Q \to \lambda,& &\text{FOLLOW}(Q) = \{\$, ) \}& &\therefore [Q, \$] = \lambda \\
&R \to \lambda,& &\text{FOLLOW}(R) = \{+, -, \$, ) \}& &\therefore [R, +] = [R, -] = [R, \$] = [R, )] = \lambda \\[1em]
\end{aligned}
$$

|     | $a$  | $+$       | $*$   | $($   | $)$       | $\$$      | $-$       | $/$   |
| --- | ---- | --------- | ----- | ----- | --------- | --------- | --------- | ----- |
| $E$ | $TQ$ |           |       | $TQ$  |           |           |           |       |
| $Q$ |      | $+TQ$     |       |       | $\lambda$ | $\lambda$ | $-TQ$     |       |
| $T$ | $FR$ |           |       | $FR$  |           |           |           |       |
| $R$ |      | $\lambda$ | $*FR$ |       | $\lambda$ | $\lambda$ | $\lambda$ | $/FR$ |
| $F$ | $a$  |           |       | $(E)$ |           |           |           |       |

Suppose we want to trace `(a+a)*a$` to determine whether the CFG accepts or
rejects it.

```
# initialize the stack
push '$', E
	stack: '$', E

# pop the first item
pop -> E
	stack: '$'

# read the first character, then begin matching using
# our lookup table
read -> '(':
	[E, '('] -> TQ

	push Q, T
		stack: '$', Q, T
	
	pop -> T
		stack: '$', Q
	
	[T, '('] -> FR

	push R, F
		stack: '$', Q, R, F
	
	pop -> F
		stack: '$', Q, R
	
	[F, '('] -> '(' E ')'

	push ')', E, '('
		stack: '$', Q, R, ')', E, '('
	
	pop -> '('
		stack: '$', Q, R, ')', E
		matched

# pop the next character
pop -> E
	stack: '$', Q, R, ')'

# read and match again
read -> 'a':
	[E, 'a'] -> TQ

	push Q, T
		stack: '$', Q, R, ')', Q, T
	
	pop -> T
		stack: '$', Q, R, ')', Q
	
	[T, 'a'] -> FR

	push R, F
		stack: '$', Q, R, ')', Q, R, F
	
	pop -> F
		stack: '$', Q, R, ')', Q, R
	
	[F, 'a'] -> 'a'

	push 'a'
		stack: '$', Q, R, ')', Q, R, 'a'

	pop -> 'a'
		stack: '$', Q, R, ')', Q, R
		matched

pop -> R
	stack: '$', Q, R, ')', Q

read -> '+':
	[R, '+'] -> λ

	pop -> Q
		stack: '$', Q, R, ')'

	[Q, '+'] -> +TQ

	push Q, T, '+'
		stack: '$', Q, R, ')', Q, T, '+'

	pop -> '+'
		stack: '$', Q, R, ')', Q, T
		matched

pop -> T
	stack: '$', Q, R, ')', Q

read -> 'a':
	[T, 'a'] -> FR

	push R, F
		stack: '$', Q, R, ')', Q, R, F
	
	pop -> F
		stack: '$', Q, R, ')', Q, R
	
	[F, 'a'] -> 'a'

	push 'a'
		stack: '$', Q, R, ')', Q, R, 'a'

	pop -> 'a'
		stack: '$', Q, R, ')', Q, R
		matched

pop -> R
	stack: '$', Q, R, ')', Q

read -> ')':
	[R, ')'] -> λ

	pop -> Q
		stack: '$', Q, R, ')'

	[Q, ')'] -> λ

	pop -> ')'
		stack: '$', Q, R
		matched

pop -> R

read -> '*':
	[R, '*'] -> *FR

	push R, F, '*'
		stack: '$', Q, R, F, '*'

	pop -> '*'
		stack: '$', Q, R, F
		matched

pop -> F

read -> 'a':
	[F, 'a'] -> 'a'

	push 'a'
		stack: '$', Q, R, 'a'

	pop -> 'a'
		stack: '$', Q, R
		matched

pop -> R

read -> '$':
	[R, '$'] -> λ

	pop -> Q
		stack: '$'

	[Q, '$'] -> λ

	pop -> '$'
		stack: <empty>
		matched
```

For `a(a+a)$`:

```
# initialize the stack
push '$', E
	stack: '$', E

# pop the first item
pop -> E
	stack: '$'

# read the first character, then begin matching using
# our lookup table
read -> 'a':
	[E, 'a'] -> TQ

	push Q, T
		stack: '$', Q, T
	
	pop -> T
		stack: '$', Q
	
	[T, 'a'] -> FR

	push R, F
		stack: '$', Q, R, F
	
	pop -> F
		stack: '$', Q, R
	
	[F, 'a'] -> 'a'

	push 'a'
		stack: '$', Q, R, 'a'

	pop -> 'a'
		stack: '$', Q, R
		matched

pop -> R

read -> '(':
	[R, '('] -> null
		panic

```
