# Addition is all you need

An exploration of the paper [Addition is All You Need for Energy-efficient Language Models](https://arxiv.org/abs/2410.00907).

## Overview of the paper

The paper proposes the $\mathcal{L}$-Mul floating point (FP) multiplication algorithm
with $O(n)$ complexity, where $n$ is the number of set bits.

By contrast, the traditional FP multiplication algorithm has
$O(m^2)$ complexity, where $m$ is the number of bits
used to represent the mantissas of the operands.

### $\mathcal{L}$-Mul

From the paper:
>Consider two floating point numbers $x$ and $y$,
>whose exponents and fractions are $x_e$, $y_e$ and $x_m$, $y_m$ respectively.
>The vanilla FP Mul result is
>$$
>\begin{aligned}
>Mul(x, y) &= (1 + x_m) * 2^{x_e} * (1 + y_m) * 2^{y_e} \\
>          &= (1 + x_m + y_m + x_m * y_m) * 2^{x_e + y_e}
>\end{aligned}
>$$
>plus an `xor` to decide the sign of the result.
>Assume $x_m$ and $y_m$ are mantissas of $m$ bits.
>The $O(m^2)$ mantissa multiplication operation is the 
>complexity bottleneck of this calculation.
>We remove this operation and introduce a new multiplication 
>algorithm that processes mantissas with a computational 
>complexity of $O(m)$:
>$$
>\begin{aligned}
>L-Mul(x, y) &= (1 + x_m + y_m + 2^{-l(m)}) * 2^{x_e + y_e}, \\
>l(m) &= \begin{cases}
>    m & m \leq 3 \\
>    3 & m = 4 \\
>    4 & m > 4.
>\end{cases}
>\end{aligned}
>$$

The key difference is the replacement of the $x_m \cdot y_m$ term
in the traditional FP multiplication algorithm
with $2^{-l(m)}$ in $\mathcal{L}$-Mul.

