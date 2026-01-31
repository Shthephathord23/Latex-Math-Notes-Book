# Integral Notation Rules

## Raw LaTeX Prohibition
- **NEVER use `\int`** - always use the semantic macros from `Preamble/Commands/MeasureTheory.tex`
- Available macros:
  - `\Integral` for standard integrals
  - `\LowerInt`, `\UpperInt` for Darboux integrals
  - `\LowerSigmaInt`, `\UpperSigmaInt` for sigma-integrals
  - `\LowerRiemann`, `\UpperRiemann` for Riemann integrals

## Bounds Notation
- **Use interval notation** for definite integrals: `\Integral_{[a,b]}` not `\Integral_a^b`
- Examples:
  - `\Integral_{[0,1]} f \, d\lambda` instead of `\Integral_0^1 f \, dx`
  - `\Integral_{[0,\infty)} f \, d\mu` instead of `\Integral_0^\infty f \, d\mu`
  - For general measure spaces without specific bounds: `\Integral_X f \, d\mu`

## Measure Notation
- **NEVER use plain `dx`, `dy`, `dt`, etc.**
- Always use proper measure differential notation:
  - `d\lambda(x)` or `d\lambda` for Lebesgue measure
  - `d\mu(x)` or `d\mu` for general measure μ
  - `d\nu` for measure ν
  - `d(\mu \otimes \nu)` for product measure

## Examples
```latex
% WRONG
\int_0^1 f(x) \, dx
\Integral_0^\infty g(t) \, dt

% CORRECT
\Integral_{[0,1]} f \, d\lambda
\Integral_{[0,\infty)} g \, d\lambda(t)
\Integral_X f \, d\mu
```

## Exception
In compendiums and informal remarks, abbreviated notation like `\Integral_0^\infty` may be acceptable for readability, but prefer the full notation when space permits.
