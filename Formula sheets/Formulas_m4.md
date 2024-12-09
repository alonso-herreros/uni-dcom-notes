---
title: Digital Communications - Midterm 4 formulas
---

<style>
body {
    column-count: 2;
    column-gap: 2em;
    column-rule: 1px solid gray;
}
@media print {
    body {
        font-size: 9pt;
    }
}
h1, h2, h3, h4, h5, h6 {
    break-after: avoid;
}
h1, h2, h3 {
    font-variant: small-caps;
    column-span: all;
}
h1, h2 { text-align: center; }

h1 {
    margin-bottom: 0;
}
h2 {
    margin-top: 0.4em;
    margin-bottom: 0.2em;
}
h3 {
    margin-top: 0.4em;
    margin-bottom: 0.2em;
    border-bottom: 0.96px dashed rgba(128, 128, 128, 0.5);
}
h4 {
    margin-top: 0.1em;
    margin-bottom: -0.3em;
}
h5 {
    margin-top: 0.5em;
    margin-bottom: -0.4em;
    font-style: italic;
}

p {
    margin-top: 0.1em;
    margin-bottom: 0.1em;
    break-before: avoid;
    break-inside: avoid;
}
ul {
    margin-top: 0.6em;
}
li {
    break-before: avoid;
    break-inside: avoid
}
table {
    margin-left: auto;
    margin-right: auto;
    break-before: avoid;
    break-inside: avoid
}
.katex .katex-html>.newline {
    height: 0.2rem;
}
.pagebreak {
    column-span: all;
    border: none;
    break-after: page;
}

/* This allows me to put a code span or equation spanning all columns */
blockquote:has(> * > * > .katex),
blockquote:has(> pre):not(:has(> *:not(pre))) {
    column-span: all;
    border: none;
    background: transparent;
}
</style>

# Digital Communications - Midterm 4 formulas

## General

$$
P_e ≈ κ Q\left(\frac{d_{min}}{2σ_n}\right)
$$

$$
W = B_{\text{rad}} = 2πB
$$

#### Tx rates

$$
R_s = \frac{1}{T_s} \\
R_b = m R_s \\
m = \log_2 M \\
$$

## Encoding

* **Input word**: $\bar{b}_i$ ($k$ bits)
* **Codeword**: $\bar{c}_i$: ($n$ bits, $2^k$ combinations)
* **Code**: $\mathcal C = \{\bar{c}_i\}_{i=1}^{2^k}$ ($2^k$ codewords)

#### Code rate

* **Code rate** = $\frac{k}{n}$
* Bitrate with code $\mathcal{C}$:

$$
R_b^{\mathcal{C}} = \frac{k}{n} R_b
$$

#### Weight

$$
wt(\bar{c}_i) = \text{\# of 1's in } \bar{c}_i
$$

#### (Hamming) distance

$$
\begin{aligned}
    d_H(\bar{c}_i, \bar{c}_j) &= \text{\# of bits different} \\
    &= wt(\bar{c}_i \oplus \bar{c}_j)
\end{aligned}
$$

$$
d_{min} = \min_{i \neq j} d_H(\bar{c}_i, \bar{c}_j)
$$

## Decoding

### Hard decoder

#### HD input: received word $\bar{r}$

$$
\bar{r} = \bar{c} + \bar{e}
$$

* $\bar{e}$: Error pattern, bits that were flipped (length $n$)

#### HD output: decoded word $\hat{\bar{b}}$

$$
\hat{\bar{b}} = \argmin_{\bar{c}_i} d_H(\bar{c}_i, \bar{r})
$$

* Codeword with the shortest Hamming distance to the received word

### Soft decoder

> ```mermaid
> %%{init: {'forceLegacyMathML':'true'} }%%
> flowchart LR
> ib(["$$\bar{b}_i$$"])
> ib --> enc[Encoder]
> enc --"$$\bar{c}_i$$"--> senc[Symbol <br/> Encoder]
> senc --"$$\bar{a}_i$$"---> sum(("$$+$$"))
> n(["$$\bar{z}$$"]) --> sum
> sum --"$$\bar{q}$$"--> dec[Decoder]
> dec --> est(["$$\hat{\bar{b}}_i$$"])
> ```

#### SD input: observation $\bar{q}$

$$
\bar{q} = \bar{a} + \bar{z}
$$

#### SD output: estimated word $\hat{\bar{b}}$

* Minimze (euclidean) distance between received symbol $\bar{q}$ and all
  possible symbols $\bar{a}$

## Error detection and correction

#### Error detection capability

$$
d_{min}-1
$$

#### Error correction capability

$$
\left⌊\frac{d_{min}-1}{2}\right⌋
$$

<hr class='pagebreak'/>

## Linear block codes

#### Linearity

$$
\bar{c}_i + \bar{c}_j ∈ \mathcal{C} \;∀\; i, j
$$

#### Properties

* $\bar{0} ∈ \mathcal{C}$
* $\bar{c}_i + \bar{0} = \bar{c}_i$
* $\displaystyle d_{min} = \min_{\bar{c}_i≠\bar{0}} wt(\bar{c}_i)$
* Easy to encode and decode

#### Generating matrix

$$
\overline{\overline{G}}_{k×n} = \begin{bmatrix}
    \bar{g}_1 \\
    \bar{g}_2 \\
    \vdots \\
    \bar{g}_k
\end{bmatrix}
$$

* $\{\bar{g}_i\}_{i=1}^{k}$ are the **generating codewords** (linearly
  independent)

$$
\bar{c}_i = \bar{b}_i ⋅ \overline{\overline{G}}
$$

## Systematic code

* The **input word** is always at the left or right end of the **codeword**

#### Systematic linear block code

Block code is systematic **iff**:

$$
\overline{\overline{G}} = \begin{bmatrix}
    \begin{array}{c:c}
    \overline{\overline{I}}_k & \overline{\overline{P}} \\
    \end{array}
\end{bmatrix} or
\begin{bmatrix}
    \begin{array}{c:c}
    \overline{\overline{P}} & \overline{\overline{I}}_k \\
    \end{array}
\end{bmatrix}
$$

Where $\overline{\overline{P}}_{k×(n-k)}$ is the **Parity matrix**

#### Parity-check matrix $\overline{\overline{H}}_{(n-k)×n}$

$$
\overline{\overline{G}} = \begin{bmatrix}
    \begin{array}{c:c}
    \overline{\overline{I}}_{k} & \overline{\overline{P}} \\
    \end{array}
\end{bmatrix} ⟹ \overline{\overline{H}} = \begin{bmatrix}
    \begin{array}{c:c}
    \overline{\overline{P}}^T & \overline{\overline{I}}_{n-k} \\
    \end{array}
\end{bmatrix} \\[0.5em]

\overline{\overline{G}} = \begin{bmatrix}
    \begin{array}{c:c}
    \overline{\overline{P}} & \overline{\overline{I}}_{k} \\
    \end{array}
\end{bmatrix} ⟹ \overline{\overline{H}} = \begin{bmatrix}
    \begin{array}{c:c}
    \overline{\overline{I}}_{n-k} & \overline{\overline{P}}^T \\
    \end{array}
\end{bmatrix}
$$

### Syndrome

$$
\bar{s} = \bar{r} ⋅ \overline{\overline{H}}^T
$$

#### Syndrome table

|           $\bar{e}$           | $\bar{s} = \bar{e}⋅\overline{\overline{H}}^T$ |
| :---------------------------: | :-------------------------------------------: |
| [$\bar{e}$ with least errors] |           [All possible syndromes]            |

#### Decoding with syndrome

1. Find $\bar{s} = \bar{r} ⋅ \overline{\overline{H}}^T$
2. Find corresponding $\bar{e}$ in table: $\hat{\bar{e}}$
3. $\hat{\bar{c}} = \bar{r} + \hat{\bar{e}}$

## Convolutional Coding

#### Convolutional encoder

* Encodes $k$ information bits in $n$ coded bits, with memory $m$

* Defined by $\left\{\bar{g}_i^j\right\}_{i,j=1}^{n,k}$ ($m$ bits each)

#### Convolutional decoder

* Viterbi algorithm

#### Convolutional Code distance

* $D_{min}$ computed via Viterbi minimum distance algorithm

#### Convolutional Code error probability

$$
P_e^{cc} ≈ κ ∑_{\mathclap{e=\left⌊\frac{D_{min}-1}{2}\right⌋ +1}}^{nz}
    \begin{pmatrix} nz \\ e \end{pmatrix} ϵ^e(1-ϵ)^{nz-e}
$$

* $z$: Length of shortest path at distance $D_{min}$
* $ϵ = P_e^{BSC}$
