# Handout 6

## Remove all left-recursive CFG

**Note (IMPORTANT!)**:

$$
\boxed{
\begin{aligned}
A &\to A \alpha \\
A &\to \beta
\end{aligned}
\ \rightarrow \
\begin{aligned}
A &\to \beta A' \\
A' &\to \alpha A' \\
A' &\to \lambda
\end{aligned}
}
$$

**Example**:

$$
\begin{aligned}
E \to E + T \\
E \to E - T \\
E \to T
\end{aligned}
$$

Introduce $E'$:

$$
\begin{aligned}
\alpha = +T \\
\alpha = -T \\
\beta = T \\
\end{aligned}
$$

$$
\begin{aligned}
E  &\to TE' \\
E' &\to +TE' \\
E' &\to -TE' \\
E' &\to \lambda
\end{aligned}
$$

<br>

**Example**:

$$
\begin{aligned}
T &\to T * F \\
T &\to T / F \\
T &\to F
\end{aligned}
\ \
\overset{\tiny{
\begin{aligned}
\alpha = *F \\
\alpha = /F \\
\beta  = F \\
\text{Introduce T'} \\
\end{aligned}
}}{\longrightarrow}
\ \
\begin{aligned}
T &\to FT' \\
T'&\to *FT' \\
T'&\to /FT' \\
T'&\to \lambda
\end{aligned}
$$

Repeat for:

$$
\begin{aligned}
E &\to E + T \\
E &\to E - T \\
E &\to T \\
T &\to T * F \\
T &\to T / F \\
T &\to F \\
F &\to (E) \\
F &\to a
\end{aligned}
\
\longrightarrow
\
\begin{aligned}
E  &\to TE' \\
E' &\to +TE' \\
E' &\to -TE' \\
E' &\to \lambda \\
T  &\to FT' \\
T' &\to *FT' \\
T' &\to /FT' \\
T' &\to \lambda \\
F  &\to (E) \\
F  &\to a
\end{aligned}
$$

<br>

**Example**: Tracing the derivation of $a*(a+a)$:

Using the regular CFG:

-   $E$
    -   $T$
        -   $T$
            -   $F$
                -   $a$
        -   $*$
        -   $F$
            -   $($
            -   $E$
                -   $E$
                    -   $T$
                        -   $F$
                            -   $a$
                    -   $+$
                    -   $T$
                        -   $F$
                            -   $a$
            -   $)$

Using the CFG with no left recursion:

-   $E$
    -   $T$
        -   $F$
            -   $\underline{a}$
        -   $T'$
            -   $\underline{*}$
            -   $F$
                -   $\underline{(}$
                -   $E$
                -   $T$
                    -   $F$
                        -   $\underline{a}$
                    -   $T'$
                        -   $\underline{\lambda}$
                -   $E'$
                    -   $\underline{+}$
                    -   $T$
                        -   $F$
                            -   $\underline{a}$
                        -   $T'$
                            -   $\underline{\lambda}$
                    -   $E'$
                        -   $\underline{\lambda}$
                -   $\underline{)}$
            -   $T'$
                -   $\underline{\lambda}$
    -   $E'$
        -   $\underline{\lambda}$

## Rules to find members of set $\text{FIRST}$ and $\text{FOLLOW}$

### Examples from Pearson's book

**Example**:

$$
\begin{aligned}
S &\to Ab | Bc \\
A &\to Df | CA \\
B &\to gA | e \\
C &\to dC | c \\
D &\to h | i
\end{aligned}
$$

-   Terminals: $\{a, b, c, d, e, f, g, h, i\}$
-   Non-terminals: $\{S, A, B, C, D\}$

Tree:

-   $S$
    -   $Ab$
        -   $Dfb$
            -   $hfb$
            -   $ifb$
            -   $\implies \text{FIRST}(D) = \{h, i\}$
        -   $CAb$
            -   $dCAb$
            -   $cAb$
            -   $\implies \text{FIRST}(C) = \{d, c\}$
        -   $\implies \text{FIRST}(A) = \text{FIRST}(D) \cup \text{FIRST(C)} = \{h, i, d, c\}$
    -   $Bc$
        -   $gAc$
        -   $ec$
        -   $\implies \text{FIRST}(B) = \{g, e\}$
    -   $\implies \text{FIRST}(S) = \text{FIRST}(A) \cup \text{FIRST}(B) = \{h, i, d, c, g, e\}$

**Example**:

$$
\begin{aligned}
&S &&\to&& ABCd&& \rightarrow \text{FIRST}(S) &&=&& \text{FIRST}(A) \cup \text{FIRST}(B) \cup \text{FIRST}(C) \\
&  &&   &&     &&                     &&=&& \{e, f, \not\lambda \} \cup \{g, h, \not\lambda \} \cup \{p, q\} \\
&  &&   &&     &&                     &&=&& \{e, f, g, h, p, q\} \\
&A &&\to&& e | f | \lambda&& \rightarrow \text{FIRST}(A) &&=&& \{ e, f, \lambda \} \\
&B &&\to&& g | h | \lambda&& \rightarrow \text{FIRST}(B) &&=&& \{ g, h, \lambda \} \\
&C &&\to&& p | q&& \to \text{FIRST}(C) &&=&& \{ p, q \}
\end{aligned}
$$

### Rules for finding $\text{FIRST}$

1. If:

   $$
   \begin{aligned}
   A &\to a,&       & a \in \text{FIRST(A)} \\
   A &\to \lambda,& & \lambda \in \text{FIRST(A)} \\
   A &\to aB,&      & a \in \text{FIRST(A)} \\
   \end{aligned}
   $$

2. If:

   $$
   \begin{aligned}
   A &\to BD,& && &\text{FIRST}(B) \subseteq \text{FIRST(A)} \\
     &       & && &\text{if } B \to \lambda \text{ then } A \to D \implies \text{FIRST}(D) \subseteq \text{FIRST(A)} \\
   \end{aligned}
   $$

**Example**:

$$
\begin{aligned}
E &\to TE' \\
E' &\to +TE' \\
E' &\to -TE' \\
E' &\to \lambda \\
T &\to FT' \\
T' &\to *FT' \\
T' &\to /FT' \\
T' &\to \lambda \\
F &\to (E) \\
F &\to a
\end{aligned}
\ \longrightarrow \
\begin{aligned}
E &\to TQ \\
Q &\to +TQ \\
Q &\to -TQ \\
Q &\to \lambda \\
T &\to FR \\
R &\to *FR \\
R &\to /FR \\
R &\to \lambda \\
F &\to (E) \\
F &\to a
\end{aligned}
$$

### How to find members of $\text{FIRST}$ set

**Example**:


$$
\begin{aligned}
&&
\left.
\begin{aligned}
E &\to TQ \\
\end{aligned}
\right\} &\ \text{FIRST}(E) = \text{FIRST}(T) = \{a, (\}

\\[1em]
&&
\left.
\begin{aligned}
Q &\to +TQ \\
Q &\to -TQ \\
Q &\to \lambda \\
\end{aligned}
\ \right\} &\ \text{FIRST}(Q) = \{+, -, \lambda\}

\\[1em]
&&
\left.
\begin{aligned}
T &\to FR \\
\end{aligned}
\right\} &\ \text{FIRST}(T) = \text{FIRST}(F) = \{a, (\}

\\[1em]
&&
\left.
\begin{aligned}
R &\to *FR \\
R &\to /FR \\
R &\to \lambda \\
\end{aligned}
\right\} &\ \text{FIRST}(R) = \{*, /, \lambda\}

\\[1em]
&&
\left.
\begin{aligned}
F &\to (E) \\
F &\to a \\
\end{aligned}
\right\} &\ \text{FIRST}(F) = \{(, a\}
\end{aligned}
$$
|      | $\text{FIRST}$  |
| ---- | --------------- |
| $E$  | $\{(, a\}$      |
| $T$  | $\{(, a\}$      |
| $F$  | $\{(, a\}$      |

Removing left-recursive CFG:

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

|      | $\text{FIRST}$  |
| ---- | --------------- |
| $E$  | $\{(, a\}$      |
| $Q$  | $\{+, -, \lambda\}$ |
| $T$  | $\{(, a\}$      |
| $R$  | $\{*, /, \lambda\}$ |
| $F$  | $\{(, a\}$      |

### How to find members of $\text{FOLLOW}$ set

1. If $E$ is starting symbol, then $\$$ is in $\text{FOLLOW}(E)$
2. If $A \to BD$, then:
	1. $\text{FIRST}(D) \subseteq \text{FOLLOW}(B)$
	2. $\text{FOLLOW}(A) \in \text{FOLLOW}(D)$
	3. $\text{if } D \to \lambda \text{ then } A \to B \text{ and } \text{FOLLOW}(A) \in \text{FOLLOW}(B)$

$$
\begin{aligned}
E &\to E + T&& \text{FOLLOW(E)} = \{ +, -, \$, ) \} \\
E &\to E - T&& \text{FOLLOW(E)} = \{ +, -, \$, ) \} \\
E &\to T&& \text{FOLLOW(T)} = \{ +, -, \$, *, /, ) \} \\
T &\to T * F \\
T &\to T / F \\
T &\to F \\
F &\to (E)&& \text{FOLLOW(F)} = \{ +, -, \$, *, /, ) \} \\
F &\to a
\end{aligned}
$$

|      | $\text{FIRST}$  | $\text{FOLLOW}$ |
| ---- | --------------- | --------------- |
| $E$  | $\{(, a\}$      | $\{ +, -, \$, ) \}$ |
| $T$  | $\{(, a\}$      | $\{ +, -, \$, *, /, ) \}$ |
| $F$  | $\{(, a\}$      | $\{ +, -, \$, *, /, ) \}$ |

Remove left-recursive CFG:

$$
\begin{aligned}
&&
\left.
\begin{aligned}
E &\to TQ \\
\end{aligned}
\right\} &\ \text{FIRST}(E) = \text{FIRST}(T) = \{a, (\}

\\[1em]
&&
\left.
\begin{aligned}
Q &\to +TQ \\
Q &\to -TQ \\
Q &\to \lambda \\
\end{aligned}
\ \right\} &\ \text{FIRST}(Q) = \{+, -, \lambda\}

\\[1em]
&&
\left.
\begin{aligned}
T &\to FR \\
\end{aligned}
\right\} &\ \text{FIRST}(T) = \text{FIRST}(F) = \{a, (\}

\\[1em]
&&
\left.
\begin{aligned}
R &\to *FR \\
R &\to /FR \\
R &\to \lambda \\
\end{aligned}
\right\} &\ \text{FIRST}(R) = \{*, /, \lambda\}

\\[1em]
&&
\left.
\begin{aligned}
F &\to (E) \\
F &\to a \\
\end{aligned}
\right\} &\ \text{FIRST}(F) = \{(, a\}
\end{aligned}
$$

- $\text{FOLLOW}(E) = \{\$, ) \}$
	- $E$ is starting symbol, so $\$ \in \text{FOLLOW}(E)$
	- List all rules with $E$ on the right-hand side:
		- $F \to (E)$
		- $) \in \text{FOLLOW}(E)$

- $\text{FOLLOW}(Q) = \{\$, ) \}$
	- $E \to TQ$
		- $\text{FOLLOW}(E) \in \text{FOLLOW}(Q)$
		- $Q \to +TQ$ doesn't add anything to $\text{FOLLOW}(Q)$
		- $Q \to -TQ$ doesn't add anything to $\text{FOLLOW}(Q)$

- $\text{FOLLOW}(T) = \{+, -, \lambda, \$, ) \}$
	- $E \to TQ$
		- $\text{FIRST}(Q) \in \text{FOLLOW}(T)$
	- $Q \to \lambda$, $E \to TQ$ becomes $E \to T$
		- $\text{FOLLOW}(E) \in \text{FOLLOW}(T)$

- $\text{FOLLOW}(R) =
	- $T \to FR$
		- $\text{FOLLOW}(T) \in \text{FOLLOW}(R)$
		- $R \to *FR$ doesn't add anything to $\text{FOLLOW}(R)$
		- $R \to /FR$ doesn't add anything to $\text{FOLLOW}(R)$

- $\text{FOLLOW}(F) = \{+, -, *, /, \lambda, \$, ) \}$
	- $T \to FR$, $R \to \lambda$ becomes $T \to F$
		- $\text{FOLLOW}(T) \in \text{FOLLOW}(F)$
	- $T \to FR$
		- $\text{FIRST}(R) \in \text{FOLLOW}(F)$

