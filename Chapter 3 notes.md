[## Digital Communications

# Chapter 3 - Notes

*Academic year 2024-2025*  

---

## Channels with ISI

```mermaid
%%{init: {'forceLegacyMathML':'true'} }%%
flowchart LR
A("$$A[n]$$")
dc["p[n]"]
dn(["z[n]"]) --> nsum(("$$+$$"))
dec["decisor"]
A --> dc --> nsum --> dec
```

In the case where the channel has ISI ($p[n] ≠ δ[n]$), the optimum decisor/detector can **not** make
decision based on a symbol-by-symbol basis. Instead, it must consider the sequence of symbols. We
will use a Maximum Likelihood Sequence Detector. Let our transmitted sequence of $L$ symbols be

$$
\bar{A} = \Big[A[0], A[1], …, A[L-1]\Big]
$$

Where $A[n]$ can take $M$ different values. The number of possible sequences of length $L$ is $M^L$.

Our channel in this case is causal and *does* have ISI. Its discrete representation is

$$
p[n] = p[0] δ[n] + … + p[L_p] δ[n - L_p]
$$

Where $L_p+1$ is the **channel length**

The received sequence vector is

$$
\bar{q} = \Big[q[0], q[1], …, q[L_q-1]\Big]
$$

Where $L_q$ is the **output length** of the channel (yet unknown), and

$$
q[n] = \sum_{k=0}^{L_p} p[k] A[n-k] + z[n]
$$

In order to observe the effect of $p$ on $q$, we'll expand this sum for each $n$.

$$
\begin{aligned}
    q[0] &= p[0] A[0] + p[1] A[-1] + … + p[L_p] A[-L_p] + z[0] \\
    q[1] &= p[0] A[1] + p[1] A[0] + p[2] A[-1] + … + p[L_p] A[-L_p+1] + z[1] \\
    &\vdots \\
    q[L_p] &= p[0] A[L_p] + p[1] A[L_p-1] + … + p[L_p] A[0] + z[L_p] \\
    &\vdots \\
    q[L-1] &= p[0] A[L-1] + p[1] A[L-2] + … + p[L_p] A[L-L_p-1] + z[L-1] \\
    q[L] &= p[0] A[L] + p[1] A[L-1] + … + p[L_p] A[L-L_p] + z[L] \\
    &\vdots \\
    q[L+L_p-1] &= p[0] A[L+L_p-1] + p[1] A[L+L_p-2] + … + p[L_p] A[L-1] + z[L+L_p-1]
\end{aligned}
$$

Note three things about the output sequence:

* The first $L_p$ terms of $q[n]$ depend on symbols transmitted *before* $\bar{A}$: $A[-L_p], …,
  A[-1]$.
* The first output symbol which has information *outside* our transmitted sequence is $q[L]$: it has
  a term that depends on $A[L]$, which is outside of $\bar{A}$.
* The last output symbol that depends on $\bar{A}$ is $q[L+L_p-1]$, which has a term that
  depends on $A[L-1]$.

Now, we can define our output length $L_q = L + L_p$. If we define the noiseless vector $\bar{o}$,
its length will be the same.

We can also give a name to the symbols outside of $\bar{A}$ that affect the output sequence:

$$
\underbrace{A[-L_p], …, A[-1]}_{\displaystyle L_p \text{ past symbols}},
\underbrace{\Big[A[0], …, A[L-1]\Big]}_{\displaystyle \bar{A}},
\underbrace{A[L], …, A[L+L_p-1]}_{\displaystyle L_p \text{ future symbols}}
$$

Where the information ouside of $\bar{A}$ required to determine the output is called the "side
information".

## Maximum Likelihood Sequence Detector

Our possible sequences are $\bar{A}_1, \bar{A}_2, …, \bar{A}_{M^L}$:

$$
\begin{aligned}
    \bar{A}_1 &= \Big[A_1[0], A_1[1], …, A_1[L-1]\Big] \\
    \bar{A}_2 &= \Big[A_2[0], A_2[1], …, A_2[L-1]\Big] \\
    &\vdots \\
    \bar{A}_{M^L} &= \Big[A_{M^L}[0], A_{M^L}[1], …, A_{M^L}[L-1]\Big]
\end{aligned}
$$

In order to find an MLSD, we need to define the **likelihood function**:

$$
f_{\bar{q} | \bar{A}}(\bar{q} | \bar{A} = \bar{A}_i)
$$

Taking into account all possible sequences, the maximum likelihood would be taken from the
folllowing set of likelihoods:

$$
\left.\begin{aligned}
f_{\bar{q} | \bar{A}}(\bar{q} | \bar{A} &= \bar{A}_1) \\
f_{\bar{q} | \bar{A}}(\bar{q} | \bar{A} &= \bar{A}_2) \\
&\vdots \\
f_{\bar{q} | \bar{A}}(\bar{q} | \bar{A} &= \bar{A}_{M^L})
\end{aligned}\right\} \text{ max}
$$

If you take a moment to think about the magnitude of the calculations required to compute this, you
may realize that the number of likelihoods we need to calculate is $M^L$. Given that sequences are
usually long, this is a worrying magnitude.

In order to solve this, let's take a look at this problem from a different perspective. We can
redefine the output symbols as follows

$$
q[n] = \sum_{k=0}^{L_p} p[k] A[n-k] + z[n] = o[n] + z[n]
$$

If the transmitted sequence were known

$$
q[n] \Bigg|_{\bar{A} = \bar{A}_i} = \sum_{k=0}^{L_p} p[k] A_i[n-k] + z[n] = o_i[n] + z[n]
$$

Where $o_i[n]$ is a known constant and $z[n]$ is a White Gaussian noise (zero mean) with variance
$σ_z^2$. This makes $q[n]$ a Gaussian random variable, with mean $o_i[n]$ and variance $σ_z^2$.

Recall that, for a vector of independent random variables, the probability density function is the
product of the individual probability density functions.

$$
f_{\bar{X}} (\bar{x}) = f_{X_1, X_2, …, X_n} (x_1, x_2, …, x_n) = ∏_{i=1}^n f_{X_i} (x_i)
$$

If $z[n]$ is white, then the value at each $n$ is independent of the value at any other $n$. This
translates to the same property for $q[n]$, for which each $q[n]$ is independent of any other.
Therefore,

$$
\begin{aligned}
    f_{\bar{q}|\bar{A}} \left(\Big[q[0], q[1], …, q[L_q-1]\Big] \middle| \bar{A} = \bar{A}_i\right)
    &= ∏_{n=0}^{L_q-1} f_{q[n] | \bar{A}} \left(q \middle| \bar{A} = \bar{A}_i\right) \\
    &= \frac{1}{(2πσ_z^2)^{\frac{L_q}{2}}}
        e^{\displaystyle -\frac{∑_{n=0}^{L_q-1} |q[n]-o_i[n]|^2}{2σ_z^2}}\\
    &= \frac{1}{(2πσ_z^2)^{\frac{L_q}{2}}}
        e^{\displaystyle -\frac{d^2(\bar{q}, \bar{o}_i)}{2σ_z^2}}
\end{aligned}
$$

> **Example**
>
> Let's consider an M-PAM system with $A[n] = ±1$, $L=4$, and $p[n] = δ[n] + 0.5δ[n-1]$
>
> Decide the maximum ML transmitted sequence when the observations are:
>
> $$
> q[0] = +0.5 \quad q[1] = -0.4 \quad q[2] = +0.1 \quad q[3] = -1.7 \quad q[4] = +0.3
> $$
>
> We'd build all possible sent sequence and their corresponding output sequences before noise,
> followed by the likelihood metric calculated for each sequence. Then, we'd choose the sequence with
> the lowest metric.
>
> | $A[0]$ | $A[1]$ | $A[2]$ | $A[3]$ | $o[0]$ | $o[1]$ | $o[2]$ | $o[3]$ | $o[4]$ | Likelihood metric |
> | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :---------------: |
> |   +1   |   +1   |   +1   |   +1   |  +1.5  |  +1.5  |  +1.5  |  +1.5  |  +1.5  |       18.25       |
> |   -1   |   +1   |   +1   |   +1   |  -0.5  |  +0.5  |  +1.5  |  +1.5  |  +1.5  |     $\vdots$      |

In order to make this large number of calculations more manageable, we can use the following
approach, looking at the sequence of symbols as a shifting register, where the output is a
convolution.

```mermaid
%%{init: {'forceLegacyMathML':'true'} }%%
graph TD
a0["$$A[n]$$"]
a1["$$A[n-1]$$"]
a_["$$…$$"]
aL["$$A[n-L_p]$$"]

p0(["$$p[0]$$"])
p1(["$$p[1]$$"])
p_(["$$…$$"])
pL(["$$p[L_p]$$"])

times0(("$$\times$$"))
times1(("$$\times$$"))
times_(("$$\times$$"))
timesL(("$$\times$$"))

sum(("$$+$$"))

a0 ---> times0 --> sum
a1 ---> times1 --> sum
a_ ---> times_ --> sum
aL ---> timesL --> sum

p0 --> times0
p1 --> times1
p_ --> times_
pL --> timesL

sum --> dec["$$o[n]$$"]
```

This can be thought of as a state machine, where the state at symbol $n$ is given by

$$
ψ[n]= \Big[A[n-L_p], A[n-L_p+1], …, A[n-1]\Big]
$$

> **Example**

Let us take the previous example and represent the input, output and state using a state diagram,
where each state is the sequence of past symbols relevant to the output, $A[n-1] … A[-L_p]$ and each
transition has the form `A[n] / o[n]`

```mermaid
stateDiagram-v2
    s1 : +1
    s2 : -1
    s1 --> s1 : +1 / +1.5
    s1 --> s2 : -1 / -0.5
    s2 --> s2 : -1 / -1.5
    s2 --> s1 : +1 / +0.5
```

We can represent the sequence as a Trellis diagram, where each node is a state, each branch is a
symbol applied to the state, each column is a sample, and each path is a possible sequence.

```mermaid
%%{init: {'forceLegacyMathML':'true'} }%%
graph LR

```
