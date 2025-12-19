# Mathematical Notation System

This document uses custom commands that should be used instead of just typing plain latex.

For example:
- use ```\Composition{S}{R}``` instead of ```S \circ R```
- use ```\Powerset{X}``` instead of ```\mathcal{P}(X)```
- use ```\tand``` instead of ```\text{ and }```
- use ```\tor``` instead of ```\text{ or }```

## Custom Quantifier Commands
The project uses sophisticated quantifier notation via `\MakeQuantifier`. For detailed implementation look in ```./Preamble/Commands/Logic.tex ```:

For example ```\Forall v:p{variable}{predicate}``` will compile to ```\forall ({variable}):({predicate})```. Omitting, `v` will remove the parentheses around the variable, omitting `:` will omit the colon and omiting `p` should omit the parentheses around the predicates.
The same with ```\Exists v:p{variable}{quantifier}```.

Use cases:
- Only add `v` and `p` arguments when without them it would be confusing since overusing them reduces readability
- For "forall x Q(x)" write like this ```\Forall vp{x}{Q(x)}``` or ```\Forall p{x}{Q(x)}``` when the text is simple and parentheses would reduce readability.
- For nested quantifiers, "forall x exists y Q(x, y)" write ```\Forall v{x}{\Exists vp{y}{Q(x, y)}}``` or ```\Forall {x}{\Exists p{y}{Q(x, y)}}``` when parentheses would reduce readability.
- Try to avoid unnecessary nested parentheses, as they reduce readability.
- Sometimes you may want to use predicates in the variable slot for the quantified variable for readability. For example in the epsilon delta definition of the limit "forall epsilon > 0 exists delta > 0 such that for all x in D 0 < |x - a| < delta implies |f(x) - L| < epsilon" you could write: ```\Forall :{\epsilon > 0}{\Exists :{\delta > 0}{\Forall :{x \in D}{0 < |x - a| < \delta \implies |f(x) - L| < \epsilon}}}```. Notice how we did not use parentheses arguments (`v` and `p`) since the expresion is readable as it is with the colon separator.

### Common Notation Patterns
- Set membership: `\Forall :{x \in A}{P(x)}`
- Existence: `\Exists :{y \in B}{Q(y)}`

### Common Errors and AI Hallucinations
- Using ```\forall``` and not the custom command
- Using ```\Forall *:{x \in X}{P(x)}``` with `*` or `+` or other character instead of `v` and `p`.
- Using ```\Forall v{x \in X}:p{P(x)}``` not in their place, like here in between the mandatory arguments. It should always be like immediatly after the command name with a space.

### Always do
- Use the `:` for set membership in the variable. Always use ```\Forall :{x \in A}{P(x)}``` and ```\Exists :{y \in B}{Q(y)}``` if the variable is just `<var_name> \in <set_name>` or ```\Forall :p``` or ```\Exists :p``` if the predicate requires it. Only add `v` and `p` arguments when without them it would be confusing since overusing them reduces readability.
- Use `p` if in the first mandatory argument there is just the variable itself. For example ```\Forall p{x}{P(x)}``` and ```\Exists p{x}{P(x)}```. If there are nested quantifiers like ```\Forall v{x}{\Exists vp{y}{Q(x, y)}}```, you should always use `vp` for the last nested quantifier, and not for the other ones, unless absolutely necessarry to reduce confusion.

## General Math Commands
- `\bb{R}`, `\bb{Z}`, `\bb{C}` - Blackboard bold via `\bb{}`
- `\defeq`, `\eqdef` - Definition equality symbols
- Custom inequality: `\le` (redefined as `<`), `\ge` (redefined as `>`)

## Binary Relations Commands
- `\Domain{R}`, `\Range{R}`, `\Codomain{R}` - Domain/range notation
- `\ConverseRel{R}` or `\ConverseRel*{R}` - Converse relation (with/without parentheses)
- `\ComplementRel{R}` - Complement relation
- `\Composition{S}{R}` - Relation composition (S ∘ R)
- `\Restriction{R}[A][B]` - Restriction notation with optional domain/codomain
- `\DirectImage{R}{A}`, `\InverseImage{R}{B}` - Image operations
- `\UniversalRel{A}[B]` - Universal relation notation
