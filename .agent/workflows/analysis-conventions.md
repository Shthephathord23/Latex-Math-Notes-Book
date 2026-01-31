---
description: Analysis notation conventions for mathematical expressions
---

# Analysis Notation Conventions

## Quantified Variables in Set Operations

When writing set operations (unions, intersections), **always use explicitly quantified indices**:

### Unions and Intersections - MUST be quantified
```latex
% CORRECT
\bigcup_{n \in \bb{N}} A_n
\bigcap_{\alpha < \omega_1} \mathcal{F}_\alpha
\bigcup_{i \in I} B_i

% INCORRECT - unquantified
\bigcup_n A_n
\bigcap_\alpha \mathcal{F}_\alpha
```

### Sums and Products - Subscript notation is acceptable
```latex
% Both are acceptable for sums/products:
\sum_{n=0}^\infty a_n       % OK - standard analysis notation
\sum_{n \in \bb{N}} a_n     % Also OK
\prod_{k=1}^n x_k           % OK
```

## Rationale

- **Set operations** like unions and intersections are fundamentally set-theoretic and should use set membership notation for clarity.
- **Sums and products** are analysis constructs where the traditional `n=0` to `\infty` notation is standard and expected.

## When to Use Each Style

| Operation | Preferred Notation | Also Acceptable |
|-----------|-------------------|-----------------|
| `\bigcup` | `_{n \in \bb{N}}` | Never use unquantified `_n` |
| `\bigcap` | `_{\alpha < \lambda}` | Never use unquantified `_\alpha` |
| `\sum` | `_{n=0}^\infty` | `_{n \in \bb{N}}` |
| `\prod` | `_{k=1}^n` | `_{k \in \{1,\ldots,n\}}` |
| `\lim` | `_{n \to \infty}` | Standard |

## Ordinal-Indexed Operations

For transfinite operations, always use explicit ordinal bounds:
```latex
\bigcup_{\alpha < \omega_1} \mathcal{F}_\alpha   % Correct
\bigcup_{\beta < \alpha} A_\beta                 % Correct
```
