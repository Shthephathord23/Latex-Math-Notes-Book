# Linear Algebra Implementation Prompt

> A comprehensive, rigorous treatment of the theory of vector spaces in the style of the existing Measure Theory content. This document serves as an AI implementation guide.

---

## Prerequisites (CRITICAL)

Before implementing this document, ensure the following packages are loaded in `Preamble/Packets/Packets.tex`:

```latex
% TikZ for diagrams
\usepackage{tikz}
\usepackage{tikz-cd}

% Caption for figures without float environment (for \captionof)
\usepackage{caption}

% Enhanced lists with [resume] capability
\usepackage{enumitem}
```

**Why these are required:**
- **tikz** and **tikz-cd**: For commutative diagrams (universal properties, isomorphism theorems)
- **caption**: For `\captionof{figure}{...}` used with TikZ figures outside float environments
- **enumitem**: For `[resume]` option in enumerate (continuing numbering across separate lists)

**Without these packages:**
- Commutative diagrams will render as raw LaTeX code (e.g., `U [r, "\pi"]...`)
- `\captionof` will be undefined
- `[resume]` will appear literally in the output

---

## Style and Writing Guidelines

### Match Measure Theory Style
- Study `Document/UniBuc FMI/MeasureTheory/Chapters/Measures.tex` for reference
- Use **intuition blocks** at the start of major topics to explain the "why"
- Use **compendium blocks** at the end of chapters for quick reference summaries
- Use **remark blocks** for conceptual connections and clarifications
- Include concrete **examples** after abstract definitions

### Rigorous and Verbose Proofs
- Every proof must be **complete and self-contained**
- Justify each step explicitly—no "it is easy to see" or "clearly"
- Use proper logical structure: claim, proof, conclusion
- Label proof directions clearly: `($\Rightarrow$)` and `($\Leftarrow$)` on separate lines
- For equivalence proofs (TFAE), explicitly prove each implication chain

### Theorem Ordering (CRITICAL)
- **Never use results before proving them**
- Example: Do NOT use rank-nullity to prove isomorphism theorems
- Isomorphism theorems must be proved via the universal property of quotient spaces
- Rank-nullity theorem comes AFTER the first isomorphism theorem (as a corollary or separate proof)
- Injectivity/Surjectivity characterizations order:
  1. Kernel = {0} (for injectivity) / Image = codomain (for surjectivity)
  2. Monic / Epic
  3. Split monomorphism / Split epimorphism
  4. Basis characterizations

### Universal Property of Quotient (Must Include)
- Full statement with all conditions:
  - $\tilde{T}$ exists iff $W \subseteq \Ker{T}$
  - Uniqueness when it exists
  - $\tilde{T}$ epic iff $T$ epic
  - $\tilde{T}$ monic iff $\Ker{T} = W$
  - $\tilde{T}$ isomorphism iff $T$ surjective and $\Ker{T} = W$

### Infinite-Dimension-Friendly Formulations (CRITICAL)
- **Avoid subtraction**—use additive cardinal equations that work for all cardinalities:
  - **Grassmann**: $\Dim[K]{U + W} + \Dim[K]{U \Intersect W} = \Dim[K]{U} + \Dim[K]{W}$ (NOT dim = dim - dim)
  - **Quotient**: $\Dim[K]{W} + \Dim[K]{\QuotientSpace{V}{W}} = \Dim[K]{V}$ (NOT dim = dim - dim)
  - **Rank-Nullity**: $\Rank{T} + \Nullity{T} = \Dim[K]{U}$ (additive form)
- **Subtraction is ONLY allowed when**:
  - The hypothesis **explicitly states** finite-dimensional (e.g., "Let $V$ be a finite-dimensional $K$-vector space")
  - Even then, prefer additive form and mention subtraction as a corollary
- **Rank-nullity proofs**:
  1. First proof via **universal property of vector spaces** (basis extension)
  2. Rediscover via **first isomorphism theorem** as a corollary
- **Cardinal philosophy**: Dimension is a cardinal; for finite dims, additive equations trivially imply subtraction formulas

### Macro Usage
- AI is **free to add new macros** to `LinearAlgebra.tex` if useful
- Use `tp` token parameters (not `s` star) for optional flags (e.g., `\QuotientSpace p{V}{W}`)
- Use `\Composition{S}{T}` for composition (from BinaryRelations.tex)
- Use `\DirectImage{f}{A}` and `\InverseImage{f}{B}` for images (from BinaryRelations.tex)
- **Prefer `\Rng{T}` over `\DirectImage{T}{U}`** when speaking of range of a linear map
- Use `\Identity{V}` for identity map (from BinaryRelations.tex)
- Use `\Set{...}` for set notation (from SetTheory.tex)
- Use `\Forall` and `\Exists` with `:`, `v`, `p` modifiers per notation guide

### Common Errors to Avoid
- Do NOT copy theorem environments at the start of sections without being asked
- Do NOT use `\forall` or `\exists` directly—use `\Forall` and `\Exists`
- Do NOT use `\lor` or `\land`—use `\LogicOr` and `\LogicAnd`
- Do NOT use `*` for starred macros—use `p` token (e.g., `\Transpose p{A}` not `\Transpose*{A}`)
- Do NOT use undefined macros—add them first if needed
- Do NOT use subtraction on dimensions in proofs—use additive form ALWAYS unless explicitly stated finite-dimensional
- Do NOT use `\sum_{n=0}^{\infty}`—use `\sum_{n \in \bb{N}}` instead (cardinal/set notation)
- Do NOT use `< \infty` for finite—use "finite-dimensional" phrasing instead
- **0-Based Indexing Convention**: This document uses **0-based indexing** throughout. Finite ordered bases of dimension $n$ are written $(b_0, b_1, \ldots, b_{n-1})$; the canonical basis of $K^n$ is $(e_0, e_1, \ldots, e_{n-1})$; $K^n$ is identified with $K^{\{0,1,\ldots,n-1\}} = K^n$ (as the von Neumann ordinal). All index ranges start at 0: $i \in \{0, \ldots, n-1\}$, $\sum_{i=0}^{n-1}$, etc. This matches the set-theoretic convention $n = \{0, 1, \ldots, n-1\}$.
- Do NOT leave broken references like `??`—ensure all `\ref{}` labels are defined before referencing
- **Do NOT write `\hfill QED` at the end of proofs**—the `proof` environment automatically adds the QED symbol. Manual QED causes double symbols in the PDF.
- **Do NOT use disjoint union symbol `\bigsqcup` or `\sqcup`**—use regular union (`\Union`, `\BigUnion`, `\Intersect`, `\BigIntersect`, `\Subset`, `\SubsetEq`, `\Supset`, `\SupsetEq`) and note "(disjoint)" or "where the union is disjoint" in text when relevant.
- **BigUnion/BigIntersect Rule**: Always use `[index]` form except when referring to the abstract operation itself (e.g., "the union of a family"). Write `\BigUnion[i \in I]` NOT `\bigcup_i`.
- **Union/Intersect Macros**: Use `\Union` and `\Intersect` from SetTheory.tex for the abstract symbols, `\BigUnion[i \in I]` and `\BigIntersect[i \in I]` for indexed operations.
- **Indexed Sums Rule (CRITICAL)**: ALL sums must be indexed. Do NOT write `\sum a_i` without specifying the index set. Use `\sum_{i=1}^{n}` (finite), `\sum_{i \in I}` (arbitrary index set), or `\sum_{n \in \mathbb{N}}` (over naturals). **Prefer `\sum_{n \in \mathbb{N}}` over `\sum_{n=0}^{\infty}`** for infinite sums over naturals.
- **Do NOT start theorem/example content with a list**—always include introductory text before `\begin{enumerate}` or `\begin{itemize}`. This is a LaTeX limitation where lists absorb line breaks from theorem styles. Example:
  ```latex
  % BAD - list starts immediately
  \begin{example}[Title]
      \begin{enumerate}
          \item First item...
  
  % GOOD - introductory text first
  \begin{example}[Title]
      Consider the following cases:
      \begin{enumerate}
          \item First item...
  ```

### Diagrams and Visual Intuition (REQUIRED)
**Packages**: `tikz-cd`, `TikZ`, `nicematrix`, `PGFPlots`

**tikz-cd SYNTAX** (CRITICAL—raw code must NOT appear in PDF):
```latex
\\begin{center}
\\begin{tikzcd}
V \\arrow[r, "\\pi"] \\arrow[dr, "T"'] & \\QuotientSpace{V}{W} \\arrow[d, dashed, "\\tilde{T}"] \\\\
 & X
\\end{tikzcd}
\\end{center}
```

**Commutative diagrams (tikz-cd)** MUST be drawn for:
- **Universal Property of Quotient Spaces** (the canonical triangle)
- **All Isomorphism Theorems** (First, Second, Third, Fourth)
- **Universal Property of Direct Product/Coproduct**
- **Change of Basis** (commutative square with two bases)
- **Dual maps** (contravariant diagram)
- **Any factorization** (e.g., $T = \tilde{T} \circ \pi$)

**Geometric intuition (TikZ/PGFPlots)** where reasonable:
- Subspaces as planes/lines in $\mathbb{R}^3$
- Direct sums as orthogonal complements
- Quotient spaces as "collapsing" a subspace
- Projections as "shadows"
- Eigenspaces as invariant directions

**3D TikZ Projections (CRITICAL for proper visualization)**:
When drawing 3D figures (planes, lines in $\mathbb{R}^3$), define proper unit vectors for oblique projection:

```latex
\begin{tikzpicture}[scale=1.2,
    x={(1cm,0cm)},        % x-axis points right
    y={(0.4cm,0.3cm)},    % y-axis points diagonally (gives depth)
    z={(0cm,1cm)}]        % z-axis points up
    
    % xy-plane (horizontal at z=0)
    \fill[blue!20, opacity=0.6] (-1.5,-1.5,0) -- (1.5,-1.5,0) -- (1.5,1.5,0) -- (-1.5,1.5,0) -- cycle;
    
    % Coordinate axes
    \draw[->, thick] (0,0,0) -- (2,0,0) node[right] {$x$};
    \draw[->, thick] (0,0,0) -- (0,2,0) node[above right] {$y$};
    \draw[->, thick] (0,0,0) -- (0,0,2) node[above] {$z$};
    
    % Origin
    \fill (0,0,0) circle (1.5pt) node[below left] {$0$};
\end{tikzpicture}
```

**Without proper 3D coordinates**, planes will appear as vertical rectangles behind axes instead of horizontal surfaces.

### Document Structure (CRITICAL)
- **Use \\chapter{} and \\section{}** for proper organization
- **Linear maps MUST come BEFORE quotient spaces** in the document  
- **Labels**: Define labels BEFORE referencing them (avoid `??`)

### Formatting Guidelines
- **Do NOT start `\begin{example}` with a list** (`\begin{enumerate}`)—it creates bad spacing. Always have introductory text before any list.
- **Examples should include TikZ figures** when geometric intuition is helpful (lines, planes, translations, etc.)

---

**COMPENDIUM STYLE**: Each chapter ends with a formula-dense compendium that summarizes ALL definitions, theorems, and formulas in compact notation. This serves as a quick reference sheet.

> **SECTION FORMATTING**: Every `\section{}` MUST be preceded by `\newpage` to ensure each section starts on a new page. The compendium is a separate section titled "Chapter Compendium".

---

## Phase 0: Macro Reference

**Macros in `Preamble/Commands/LinearAlgebra.tex`:**

| Category | Macros |
|----------|--------|
| **Vector Spaces** | `\VecSpace[K]{V}`, `\Span[K]{S}`, `\Subspace[K]` |
| **Sums** | `\SubspaceSum{index}{body}` (2-arg) or `\SubspaceSum{i=1}{n}{U_i}` (3-arg with superscript), same for `\DirectSum`, `\ExtDirectSum`, `\DirectProd` |
| **Quotient** | `\QuotientSpace{V}{W}`, `\QuotientSpace p{V}{W}` (parenthesized), `\Coset{v}{W}` |
| **Fundamental** | `\Ker{T}`, `\Rng{T}`, `\Rank{T}`, `\Nullity{T}` |
| **Dimension** | `\Dim[K]{V}` |
| **Subspace Lattice** | `\Sub{V}` → $\operatorname{Sub}(V)$ (set of all subspaces of $V$, ordered by inclusion) |
| **Matrix Spaces** | `\MatSpace{X}{Y}[K]` → $\mathrm{Mat}_{X,Y}(K)$ (all functions $X \times Y \to K$, no finiteness); `\CFMat{X}{Y}[K]` → $\mathrm{Mat}_{X,(Y)}(K)$ (column-finite: for each $j \in Y$, $A(-,j) \in K^{(X)}$) |
| **Matrices** | `\Transpose{A}`, `\Adjoint{A}`, `\Hermitian{A}`, `\TensorProduct[K]{U}{V}`, `\Hadamard{A}{B}`, `\KroneckerProduct[K]{A}{B}` |
| **Matrix Rep** | `\MatrixRep{T}` → $[T]$; `\MatrixRep{T}[\mathcal{B}]` → $[T]_{\mathcal{B}}$; `\MatrixRep{T}[\mathcal{B}_U][\mathcal{B}_V]` → $[T]_{\mathcal{B}_U}^{\mathcal{B}_V}$ |
| **Spectral** | `\Det{A}`, `\Trace{A}`, `\Adjugate{A}`, `\Spectrum{T}`, `\Eigenspace{T}{\lambda}`, `\GenEigenspace{T}{\lambda}` |
| **Polynomials** | `\MinPoly{T}`, `\CharPoly{T}` |
| **Hom/End** | `\Hom[K]{U}{V}`, `\End[K]{V}`, `\Aut[K]{V}`, `\GL{n}{K}`, `\Dual{V}` |
| **Dual** | `\DualBasis{B}`, `\DualMap{T}` |
| **Multilinear** | `\MultiLin[K]{...}{V}`, `\MultiForm[K]{n}{V}`, `\SymForm{n}{V}`, `\AltForm{n}{V}`, `\Wedge{a}{b}` |
| **Norms/IP** | `\Norm[p]{v}`, `\InnerProd{u}{v}`, `\OrthComp{U}`, `\Proj{U}`, `\Sym{U}` |
| **Column Vectors** | `\ColVec{a_1 \\ a_2 \\ \vdots \\ a_n}` (no base), `\ColVec{a_1 \\ \vdots \\ a_n}[\mathcal{B}]` (with base subscript) |
| **Misc** | `\LinIndep`, `\LinDep`, `\MinkowskiSum{A}{B}`, `\Hermitian{A}` |

**Sum Macro Syntax (CRITICAL)**:
- **2-arg (subscript only)**: `\DirectSum{i \in I}{U_i}` → $\bigoplus_{i \in I} U_i$
- **3-arg (sub + superscript)**: `\DirectSum{i=1}{n}{U_i}` → $\bigoplus_{i=1}^{n} U_i$
- **Prefer 3-arg for numbered ranges** like $i=1$ to $n$; use 2-arg for set membership like $i \in I$
- Same applies to `\SubspaceSum`, `\DirectProd`, `\ExtDirectSum`

**ALWAYS use `[K]` subscript** for: `\Span[K]`, `\Subspace[K]`, `\Dim[K]`, `\Hom[K]`, `\End[K]`, `\Aut[K]`, `\TensorProduct[K]`, `\KroneckerProduct[K]`, `\MatSpace[K]`, `\CFMat[K]`

**Use from `Preamble/Commands/BinaryRelations.tex`:**
- `\Composition{S}{T}` for $S \circ T$
- `\DirectImage{f}{A}` for $f[A]$
- `\InverseImage{f}{B}` for $f^{\leftarrow}[B]$
- `\Identity{V}` for $\mathrm{Id}_V$
- `\Restriction{f}[A]` for $f|_A$

**Use from `Preamble/Commands/SetTheory.tex`:**
- `\Set{...}` for $\{...\}$
- `\Powerset{X}` for $\mathcal{P}(X)$
- `\EquivClass[R]{x}` for $[x]_R$
- `\BigUnion[i \in I]` for $\bigcup_{i \in I}$, `\BigUnion` for abstract union operation
- `\BigIntersect[i \in I]` for $\bigcap_{i \in I}$, `\BigIntersect` for abstract intersection
- `\Card{X}` for $|X|$ (cardinality)

**BigUnion/BigIntersect Rule**: Always use `[index]` form except when referring to the abstract operation itself (e.g., "the union of a family").

### Notation Conventions
- **L** = Linearly independent set (not F)
- **G** = Generating system (not S)
- **B** = Basis
- **Dimension notation**: NEVER write `dim = n < ∞` or `dim < ∞`. Always use:
  - `$\Dim[K]{V} = n \in \bb{N}$` for finite-dimensional
  - `$\Dim[K]{V} < \aleph_0$` when comparing to cardinals
  - `$\Dim[K]{V}$ is finite` in prose



---

# CHAPTER 1: Foundations of Vector Spaces

**File: `Document/LinearAlgebra/VectorSpaces/Foundations/`**

### Required 3D Examples in Chapter 1
Each of the following concepts MUST have examples and non-examples in $\mathbb{R}^3$ with TikZ figures:
- **Span** (Phase 3b): Line (1 vector), plane (2 non-collinear vectors), whole space (3 independent vectors), non-example (redundant vectors)
- **Linear Independence** (Phase 4): Independent (3 axes), dependent (coplanar vectors), dependent (4+ vectors)
- **Bases** (Phase 6): Standard basis, non-standard basis, not-a-basis (spanning but dependent), not-a-basis (independent but not spanning)

## Phase 1: Vector Spaces

### Categorical Framing
- **Linear Algebra studies the category $K$-Vect**
- **Objects**: $K$-vector spaces (defined in this phase)
- **Morphisms**: $K$-linear transformations (defined in Phase 9)
- This perspective unifies the treatment and motivates constructions

### File: `Document/LinearAlgebra/VectorSpaces/Foundations/Definition.tex`

1. **Intuition Block**: Category K-Vect perspective; vector spaces as the fundamental objects
2. **Definition** with 8 axioms (abelian group + ring action)
3. **Geometric vectors** in 2D/3D (FIRST EXAMPLE):
   - Identify points with position vectors (arrows from origin)
   - **Addition** via tip-to-toe rule (equivalently parallelogram rule)
   - **Scalar multiplication** as scaling (stretch/shrink, reverse if negative)
   - 🎨 **FIGURE**: TikZ illustration of parallelogram rule and 3D vector
4. **Coordinate spaces $K^n$**:
   - $K^n = K^{\{0, 1, \ldots, n-1\}}$—view $n$-tuples as functions (0-based: index set is the von Neumann ordinal $n = \{0, \ldots, n-1\}$)
   - Pointwise operations
5. **Geometric Interpretation of $K^n$** (REMARK):
   - For $n = 1, 2, 3$ over $K = \bb{R}$, identify points in $\bb{R}^n$ with **position vectors** (arrows from origin $0$ to the point)
   - This identification is bijective: $(x_0, \ldots, x_{n-1}) \leftrightarrow$ arrow from $0$ to $(x_0, \ldots, x_{n-1})$
   - **Geometric proof for addition**: Decompose $u = u_0 e_0 + u_1 e_1$, $v = v_0 e_0 + v_1 e_1$. By axioms: $u + v = (u_0+v_0)e_0 + (u_1+v_1)e_1$, which is the coordinatewise sum. Geometrically: fourth vertex of the parallelogram.
   - **Geometric proof for scaling**: $\alpha u = \alpha(u_0 e_0 + u_1 e_1) = (\alpha u_0)e_0 + (\alpha u_1)e_1$, the coordinatewise product. Geometrically: scaling the arrow by $|\alpha|$, reversing if $\alpha < 0$.
   - 🎨 **FIGURE 1**: Parallelogram rule — large TikZ diagram (scale 1.3) with grid, axes, u+v label at tip
   - 🎨 **FIGURE 2**: Scalar multiplication — three side-by-side diagrams: $\alpha=2$ (stretch), $\alpha=\tfrac{1}{2}$ (squish), $\alpha=-1$ (negate)
   - This geometric intuition guides linear algebra even in higher dimensions and abstract fields
6. **Function spaces $K^I$**:
   - Define pointwise operations for arbitrary index set $I$
   - Define **indicator functions** $e_i(j) = \delta_{ij}$ (do NOT call them "basis" yet—bases not introduced)
7. **Functions with Finite Support $K^{(I)}$**:
   - Definition as subspace of $K^I$
   - Every $f \in K^{(I)}$ can be written as finite sum $\sum f(i) e_i$
   - Do NOT claim $(e_i)$ is a basis here—defer to Phase 7
   - Key distinction: $K^{(I)} = K^I$ iff $I$ finite
   - **Column vector notation for $K^n$** (REMARK): For $n \in \bb{N}$, elements of $K^n$ (functions from $\{0,\ldots,n-1\}$ to $K$) are denoted as column vectors $\ColVec{a_0 \\ \vdots \\ a_{n-1}}$. Show indicator functions $e_0, \ldots, e_{n-1}$ in column form (0-based).
8. **Polynomial spaces $K[X]$**:
   - $K[X]_{\leq n} = \Set{p \in K[X] \mid \deg p \leq n}$ (degree at most $n$)
   - $K[X]_{\le n} = \Set{p \in K[X] \mid \deg p \le n}$ (degree less than $n$)
9. **Matrix spaces $\MatSpace{m}{n}[K]$ and $\CFMat{m}{n}[K]$**:
   - $\MatSpace{m}{n}[K] = K^{m \times n}$: all functions $\{0,\ldots,m-1\} \times \{0,\ldots,n-1\} \to K$; pointwise operations
   - $\CFMat{m}{n}[K]$: column-finite subspace (for finite $m, n$, coincides with $\MatSpace{m}{n}[K]$; distinction matters for infinite index sets)
   - Matrix multiplication defined on $\CFMat{m}{n}[K]$ (fully developed in Phase 16)
10. **Field Extension Examples**: $\mathbb{C}/\mathbb{R}$, $\mathbb{R}/\mathbb{Q}$
11. **Properties from axioms** with proofs:
    - $0v = 0$, $a0 = 0$, $(-1)v = -v$
    - $av = 0 \implies a = 0 \LogicOr v = 0$
    - **Cancellation laws**: $av = bv$ with $v \neq 0 \implies a = b$; $av = aw$ with $a \neq 0 \implies v = w$
12. **Remark on Notation Abuse**: $0$ is polymorphic (scalar or vector from context)

---

## Phase 2: Minkowski Structure on $\mathcal{P}(V)$

1. **Minkowski sum**: $\MinkowskiSum{A}{B} = \Set{a+b \mid a \in A, b \in B}$
   - **Examples**:
     - $\MinkowskiSum{\{1,2\}}{\{10,20\}} = \{11, 21, 12, 22\}$ in $\mathbb{R}$
     - $\MinkowskiSum{[0,1]}{[0,1]} = [0,2]$ (interval sum in $\mathbb{R}$)
     - $\MinkowskiSum{\text{line through origin}}{\{p\}}$ = translated line through $p$ (in $\mathbb{R}^2$)
   - 🎨 **FIGURE**: Line through origin + point $p$ = parallel line through $p$
   - **Singleton notation**: $\MinkowskiSum{u}{U} \defeq \MinkowskiSum{\{u\}}{U}$ (translation of $U$ by $u$)

2. **Minkowski scalar-set product**: $\MinkowskiProd{L}{A} = \Set{\lambda a \mid \lambda \in L, a \in A}$ for $L \subseteq K$, $A \subseteq V$
   - **Examples**:
     - $\MinkowskiProd{\{1,2\}}{\{3\}} = \{3, 6\}$ in $\mathbb{R}$
     - $\MinkowskiProd{[0,1]}{[0,1]} = [0,1]$ (in $\mathbb{R}$)
     - $\MinkowskiProd{\mathbb{R}}{\{v\}} = \Set{\lambda v \mid \lambda \in \mathbb{R}}$ (line through origin, for $v \neq 0$)
   - 🎨 **FIGURE**: Scaling a set by $\mathbb{R}$ gives line through origin
   - **Singleton notation**: $\MinkowskiProd{\lambda}{A} \defeq \MinkowskiProd{\{\lambda\}}{A}$

3. **Monoid structure of $(\mathcal{P}(V), +)$** (merged with absorbing):
   - Associativity, commutativity
   - Identity: $\{0\}$
   - Absorbing element: $\emptyset + A = \emptyset$

4. **Monoid action of $(\mathcal{P}(K), \cdot)$ on $\mathcal{P}(V)$**:
   - Identity: $\{1_K\} \cdot A = A$
   - Compatibility: $(L \cdot M) \cdot A = L \cdot (M \cdot A)$
   - Absorbing: $\emptyset \cdot A = \emptyset$ and $L \cdot \emptyset = \emptyset$
   - Zero properties: $L \cdot \{0\} = \{0\}$ if $L \neq \emptyset$; $\{0\} \cdot A = \{0\}$ if $A \neq \emptyset$

5. **Monotonicity** (Proposition):
   - **Statement**: Minkowski operations are monotone:
     - (1) If $A \subseteq B$, then $A + C \subseteq B + C$
     - (2) If $A \subseteq B$, then $LA \subseteq LB$
     - (3) If $L \subseteq M$, then $LA \subseteq MA$
   - **Proof**: Each follows by noting elements of LHS are in RHS since A⊆B or L⊆M.

6. **Partial Distributivity** (Proposition):
   - **Statement**: $L(A+B) \subseteq LA+LB$ and $(L+M)A \subseteq LA+MA$ for all $L, M \subseteq K$, $A, B \subseteq V$
   - **Proof of (1)**: Let $x \in L(A+B)$. Then $x = \lambda(a+b)$ for $\lambda \in L$, $a \in A$, $b \in B$. By distributivity: $x = \lambda a + \lambda b \in LA + LB$.
   - **Proof of (2)**: Let $x \in (L+M)A$. Then $x = (\lambda+\mu)a$ for $\lambda \in L$, $\mu \in M$, $a \in A$. By distributivity: $x = \lambda a + \mu a \in LA + MA$.

7. **Failure of Reverse Inclusions** (Example):
   - **(1) $LA+LB \not\subseteq L(A+B)$**: Take $L=\{1,2\}$, $A=B=\{1\}$. Then $L(A+B) = \{1,2\}\cdot\{2\} = \{2,4\}$ but $LA+LB = \{1,2\}+\{1,2\} = \{2,3,4\}$. Here $3 \in LA+LB$ but $3 \notin L(A+B)$.
   - **(2) $LA+MA \not\subseteq (L+M)A$**: Take $L=M=\{1\}$, $A=\{0,1\}$. Then $(L+M)A = \{2\}\cdot\{0,1\} = \{0,2\}$ but $LA+MA = \{0,1\}+\{0,1\} = \{0,1,2\}$. Here $1 \in LA+MA$ but $1 \notin (L+M)A$.

8. **Full Distributivity Conditions** (Proposition):
   - **Statement**: The reverse inclusions hold under these conditions:
     - (1) If $L = \{\lambda\}$ is a singleton, then $\lambda A + \lambda B = \lambda(A+B)$
     - (2) If $A = \{a\}$ is a singleton, then $La + Ma = (L+M)a$
     - (3) If $KB = B$ and $0 \in A$ (or $KA = A$ and $0 \in B$), then $LA + LB = L(A+B)$
     - (4) If $KA = A$, $A + A = A$, and $L + M \neq \{0\}$, then $LA + MA = (L+M)A$
   - **Proof of (1)**: Let $x \in \lambda A + \lambda B$, so $x = \lambda a + \lambda b$. Then $x = \lambda(a+b) \in \lambda(A+B)$.
   - **Proof of (2)**: Let $x \in La + Ma$, so $x = \lambda a + \mu a$. Then $x = (\lambda+\mu)a \in (L+M)a$.
   - **Proof of (3)**: Assume $KB = B$ and $0 \in A$. Let $x = \lambda a + \mu b \in LA + LB$.
     - Case $\lambda \neq 0$: Since $KB = B$, $(\mu/\lambda)b \in B$. Thus $x = \lambda(a + (\mu/\lambda)b) \in L(A+B)$.
     - Case $\lambda = 0$: Then $x = \mu b = \mu(0 + b) \in L(A+B)$ since $0 \in A$.
   - **Proof of (4)**: Assume $KA = A$, $A + A = A$, $L + M \neq \{0\}$. Let $x = \lambda a + \mu a' \in LA + MA$.
     - Case $\lambda + \mu \neq 0$: Since $KA = A$ and $A+A=A$, $\frac{\lambda}{\lambda+\mu}a + \frac{\mu}{\lambda+\mu}a' \in A$. Thus $x = (\lambda+\mu) \cdot (\frac{\lambda}{\lambda+\mu}a + \frac{\mu}{\lambda+\mu}a') \in (L+M)A$.
     - Case $\lambda + \mu = 0$: Then $\mu = -\lambda$ and $x = \lambda(a-a')$. Since $L+M \neq \{0\}$, $\exists \lambda' \in L, \mu' \in M$ with $\lambda'+\mu' \neq 0$. Since $KA=A$ and $A+A=A$, $\frac{\lambda}{\lambda'+\mu'}(a-a') \in A$. Thus $x = (\lambda'+\mu') \cdot \frac{\lambda}{\lambda'+\mu'}(a-a') \in (L+M)A$.

9. **Distributivity over Unions** (Proposition):
   - $A + \bigcup_i B_i = \bigcup_i (A + B_i)$
   - $L \cdot \bigcup_i A_i = \bigcup_i LA_i$
   - $(\bigcup_i L_i) \cdot A = \bigcup_i L_i A$
   - **Proof**: Iff-chain: $x = a + b$ with $b \in B_j$ iff $x \in A + B_j$

10. **Sub-Distributivity over Intersections** (Proposition):
    - $A + \bigcap_i B_i \subseteq \bigcap_i (A + B_i)$
    - $L \cdot \bigcap_i A_i \subseteq \bigcap_i LA_i$
    - $(\bigcap_i L_i) \cdot A \subseteq \bigcap_i L_i A$
    - **Proof**: If $b \in \bigcap B_i$, then $a + b \in A + B_i$ for all $i$

11. **Counterexamples for Reverse Intersection Inclusions**:
    - $(A+B) \Intersect (A+C) \not\subseteq A + (B \Intersect C)$: $A=\{0,1\}$, $B=\{0\}$, $C=\{1\}$ → RHS empty, LHS = $\{1\}$
    - $LA \Intersect LB \not\subseteq L(A \Intersect B)$: $L=\{1,2\}$, $A=\{1\}$, $B=\{2\}$ → RHS empty, LHS = $\{2\}$

12. **Singleton Equality for Intersections** (Proposition):
    - $\{a\} + \bigcap_i B_i = \bigcap_i (\{a\} + B_i)$
    - $\{\lambda\} \cdot \bigcap_i A_i = \bigcap_i \{\lambda\} A_i$
    - **Proof**: $x \in \bigcap_i (\{a\} + B_i)$ ⟹ $x = a + b_i$ but $b_i = x - a$ is same for all $i$

## Phase 3: Vector Subspaces

1. **Definition**
   - **Examples**:
     - Lines through origin in $\mathbb{R}^2$
     - Planes through origin in $\mathbb{R}^3$
     - $K[X]_{\leq n} \Subspace[K] K[X]$ (polynomials of degree $\leq n$)
   - 🎨 **FIGURE**: Line and plane through origin in $\mathbb{R}^3$
   - **Non-examples**:
     - Line NOT through origin (fails $0 \in U$)
     - First quadrant in $\mathbb{R}^2$ (fails closure under scalar mult)
2. **Subspace Criterion**: $U \neq \emptyset$ and $\Forall :{a,b \in K}{\Forall :{u,v \in U}{au + bv \in U}}$
3. **Minkowski Characterization of Subspaces** (prove all equivalences):
   - (i) $U$ is a subspace
   - (ii) $U \neq \emptyset$, $U + U = U$, and $KU = U$
   - (iii) $U \neq \emptyset$ and $KU + KU = U$
   - where $KU = \Set{au \mid a \in K, u \in U}$
   - **Proof outline**:
     - (1)⇒(2): $U \neq \emptyset$ since $0 \in U$. $U + U \subseteq U$ by closure, $U \subseteq U + U$ via $u = u + 0$; similarly for $KU$
     - (2)⇒(3): $U \neq \emptyset$ by assumption. $KU + KU = U + U = U$
     - (3)⇒(1): $0 = \lambda u + (-\lambda)u \in KU + KU = U$ for any $u \in U$ (exists since nonempty); closure via $au + bv \in KU + KU$
4. **Linear Combination** (CRITICAL DEFINITION):
   - A **linear combination** of vectors in $S$ is a **FINITE** sum $\sum_{i=1}^{n} a_i v_i$ with $a_i \in K$, $v_i \in S$
   - **Examples**:
     - $3v_1 + 2v_2$ is a linear combination of $\{v_1, v_2\}$
     - The empty sum (= $0$) is a linear combination
     - In $\mathbb{R}^3$: $(1,2,3) = 1 \cdot e_1 + 2 \cdot e_2 + 3 \cdot e_3$ is lin. comb. of standard basis
   - 🎨 **FIGURE**: Span of 2 vectors in $\mathbb{R}^3$ = plane through origin
   - **Non-examples**:
     - Infinite series like $\sum_{i=1}^{\infty} a_i v_i$ are NOT linear combinations
   - **Remark (finiteness is essential)**: Infinite sums require topological structure (limits). As counterexample: constant sequence $(1,1,1,\ldots)$ is NOT a lin. comb. of $e_i$'s in $K^{\bb{N}}$, even though intuitively it equals $\sum_{i=1}^{\infty} e_i$.
   - **Proposition (lin. combs. of indicator functions)**: The set of all lin. combs. of $(e_i)_{i \in I}$ in $K^I$ is precisely $K^{(I)}$. In particular if $I$ is infinite, elements of $K^I \setminus K^{(I)}$ exist. Full proof with both inclusions using Kronecker delta calculations. Do NOT use span here—span is introduced later.
4. **Vector subspace iff closed under all linear combinations**: $U \Subspace[K] V$ iff $\Forall :{F \subseteq U, F \text{ finite}}{\Forall :{(a_v)_{v \in F} \in K^F}{\sum_{v \in F} a_v v \in U}}$
5. **Trivial subspaces**: $\{0\}$ and $V$
   - **Non trivial examples of vector spaces**
   - 🎨 **FIGURE**: Lines and planes through origin in $\mathbb{R}^3$
5b. **Remark (Subspace Lattice $\Sub{V}$)**:
   - Write $\Sub{V}$ for the **set of all $K$-subspaces of $V$**, ordered by inclusion.
   - It is a **complete lattice**: meet $= \bigcap$ (intersection), join $= \sum$ (sum).
   - Bottom element: $\{0\}$; top element: $V$.
   - Forward reference: this lattice is the home of **flags** (Phase 7b).
6. **Intersection**: Arbitrary intersection of subspaces is subspace
7. **Union**: $U \Union W$ is subspace iff $U \subseteq W \LogicOr W \subseteq U$
8. **Minkowski sum of subspaces is a subspace** (prove)
9. **Extend the definition of subspace sum to arbitrary sums**
   - $\SubspaceSum{i \in I}{U_i} = \Set{ \sum_{i \in F} u_i \mid F \subseteq I, F \text{ finite}, \Forall :{i \in F}{u_i \in U_i} }$
10. **Prove that $\SubspaceSum{i \in I}{U_i}$ is a subspace**
11. **Show that for finite sets I, it is the same as Minkowski sum**

**NOTE**: Hyperplanes defined AFTER direct sums and linear maps (see Phase 10).

---

## Phase 3b: Span (Generated Subspaces)

1. **Linear Combination** (CRITICAL DEFINITION):
   - A **linear combination** of vectors in $S$ is a **FINITE** sum $\sum_{i=1}^{n} a_i v_i$ with $a_i \in K$, $v_i \in S$
   - **Examples**:
     - $3v_1 + 2v_2$ is a linear combination of $\{v_1, v_2\}$
     - The empty sum (= $0$) is a linear combination
     - In $\mathbb{R}^3$: $(1,2,3) = 1 \cdot e_1 + 2 \cdot e_2 + 3 \cdot e_3$ is lin. comb. of standard basis
   - 🎨 **FIGURE**: Span of 2 vectors in $\mathbb{R}^3$ = plane through origin
   - **Non-examples**:
     - Infinite series like $\sum_{i=1}^{\infty} a_i v_i$ are NOT linear combinations
   - **Counterexample**: In $K^{\bb{N}}$, the constant-$1$ sequence $f(i)=1$ is NOT a lin. comb. of $e_i$ (finite support argument). The set of all lin. combs. of $(e_i)$ equals $K^{(I)}$, which is strictly smaller than $K^I$ for infinite $I$.

2. **Definition (intersection)**: $\Span[K]{S} = \BigIntersect \Set{ U \Subspace[K] V \mid S \subseteq U }$
   - **Examples**:
     - $\Span[\mathbb{R}]{\{(1,0)\}} = $ x-axis in $\mathbb{R}^2$
     - $\Span[\mathbb{R}]{\{(1,0), (0,1)\}} = \mathbb{R}^2$
     - $\Span[K]{\{1, X, X^2\}} = K[X]_{\leq 2}$ (polynomials of degree $\leq 2$)
   - 🎨 **FIGURE**: Span of one vector = line through origin; span of two non-collinear vectors = plane
3. **Constructive characterization**: $\Span[K]{S}$ = set of all FINITE $K$-linear combinations of elements in $S$
   - Prove equivalence between these definitions
4. **Properties**:
   - use earlier proofs to prove the latter
   - $\Span[K]{S}$ is subspace
   - $S \subseteq \Span[K]{S}$
   - If $S \subseteq U$ where $U \Subspace[K] V$, then $\Span[K]{S} \subseteq U$
   - **Monotonicity**: $S \subseteq T \implies \Span[K]{S} \subseteq \Span[K]{T}$
   - **Idempotence**: $\Span[K]{\Span[K]{S}} = \Span[K]{S}$
   - **Subspace criterion**: $U$ subspace iff $\Span[K]{U} = U$
   - **Union**: $\Span[K]{S \Union T} = \Span[K]{\Span[K]{S} \Union \Span[K]{T}}$
   - **Span of empty set**: $\Span[K]{\emptyset} = \{0\}$ (empty sum = 0)
   - **Span of zero**: $\Span[K]{\{0\}} = \{0\}$ (all linear combinations of 0 equal 0)
5. **Sum of subspaces**: $\SubspaceSum{U}{W} = \Span[K]{U \Union W}$ (with proof)
6. **Arbitrary sums**: $\SubspaceSum{U_i}_{i \in I} = \Span[K]{\BigUnion[i \in I] U_i}$
   - **Proof**: ($\subseteq$) Each $\sum_{j \in F} u_j$ is a linear combination of elements in the union. ($\supseteq$) Each $u \in U_j$ is a singleton sum, and $S$ is a subspace containing the union.
7. **Sum of Spans Proposition**: $\SubspaceSum{i \in I}{\Span[K]{S_i}} = \Span[K]{\BigUnion[i \in I] S_i}$ (with proof)
8. **Generating systems (G)**:
   - $G \subseteq V$ is generating iff $\Span[K]{G} = V$
   - **$V$ always generates $V$**: $\Span[K]{V} = V$ (since $V$ is a subspace containing itself)
   - **Superset of generating is generating**
9. **$\Span[K]{\{e_i\}_{i \in I}} = K^{(I)}$** (Proposition, follows from the linear combination example in Phase 3):
   - Reformulation using span: the set of lin. combs. of $e_i$ proven to be $K^{(I)}$ equals $\Span[K]{\{e_i\}_{i \in I}}$ by constructive characterization of span
   - **Corollary**: For infinite $I$, $\Span[K]{\{e_i\}_{i \in I}} = K^{(I)} \subsetneq K^I$ — indicator functions do NOT generate $K^I$

---

## Phase 4: Linear Independence

1. **Intuition**: Not all vectors contribute equally to span
   - **Examples of lin. indep. sets**:
     - $\{(1,0), (0,1)\}$ in $\mathbb{R}^2$
     - $\{1, X, X^2\}$ in $K[X]$
     - $\{e_0, e_1, e_2\}$ standard basis in $\mathbb{R}^3$ (0-indexed)
   - 🎨 **FIGURE**: Two non-collinear vectors in $\mathbb{R}^2$ (lin. indep.) vs two collinear vectors (lin. dep.)
   - **Examples of lin. dep. sets**:
     - $\{(1,0), (2,0)\}$ in $\mathbb{R}^2$ (one is scalar multiple of other)
     - $\{(1,0,0), (0,1,0), (1,1,0)\}$ in $\mathbb{R}^3$ (third is sum of first two)
2. **Intuition**: $v \in S$ is **linearly dependent on** $S \setminus \{v\}$ iff $v \in \Span[K]{S \setminus \{v\}}$ (formal definition of linear dependence is "not linearly independent", given below)
   - **Examples**:
     - $(1,2,3)$ is linearly dependent on $(1,0,0), (0,1,0), (0,0,1)$ in $\mathbb{R}^3$, because...
     - $(1,2,3)$ is linearly independent on $(1,0,0), (0,1,0), (0,0,1)$ in $\mathbb{R}^3$, because...
     - $(1,2,3)$ is linearly dependent on $(1,0,0), (0,1,0), (0,0,1)$ in $\mathbb{R}^4$, because...
3. **Unique representation (DEFINE FIRST)**:
   - A vector $v$ has **unique representation** in terms of $L$ iff there exists a **unique finite subset** $F \subseteq L$ and **unique nonzero scalars** $(a_\ell)_{\ell \in F}$ such that $v = \sum_{\ell \in F} a_\ell \ell$
   - Nonzero scalars required for uniqueness (else could add $0 \cdot w$ for any $w$)
4. **Equivalent characterizations of linear independence** (prove all equivalences):
   - (i) $\Forall :{v \in L}{\Span[K]{\{v\}} \Intersect \Span[K]{L \setminus \{v\}} = \{0\}}$ (equivalent to (i))
   - (i') $\Forall :{v \in L}{\Span[K]{L \setminus \{v\}} \subsetneq \Span[K]{L}}$
   - (ii) Every $v \in \Span[K]{L}$ has unique representation
   - (iii) $\Exists :{v \in \Span[K]{L}}{\text{unique representation}}$
   - (iv) $0$ has unique representation (only trivial combination gives $0$)
   
   State that (i)/(i') implies the rest needs field axioms (invertible nonzero elements), while the rest that imply (i) do not need them in the proof.
5. **Define linear independence as any of the above characterizations**
6. We always have a linearly independent set, namely the empty set.
7. **Subset of linearly independent is linearly independent**

---

## Phase 5: Internal Direct Sums

**Definition of unique representation first, then direct sums:**

**Duality with Linear Independence**: The characterizations of internal direct sums are **similar** to those of linear independence:
- Linear independence: unique representation of vectors as linear combinations of **vectors** in $L$
- Direct sum: unique representation of vectors as sums of **subspace elements** from $(U_i)_{i \in I}$

1. **Unique representation in a family of subspaces**:
   - Given $(U_i)_{i \in I}$ subspaces, a vector $v$ has **unique representation** iff there exists a **unique finite subset** $F \subseteq I$ and **unique nonzero vectors** $(u_i)_{i \in F}$ with $u_i \in U_i$ such that $v = \sum_{i \in F} u_i$
   - Nonzero components required for uniqueness (else could add $0 \in U_j$ for any $j$)

2. **Equivalent characterizations of internal direct sums** (prove all equivalences):
   - (i) $(U_i)$ is direct sum family iff $\Forall :{j \in I}{U_j \Intersect \SubspaceSum{i \neq j}{U_i} = \{0\}}$
   - (ii) $(U_i)$ is direct sum family iff $\Forall :{v \in \SubspaceSum{i \in I}{U_i}}{\text{unique representation}}$
   - (iii) $(U_i)$ is direct sum family iff $\Exists :{v \in \SubspaceSum{i \in I}{U_i}}{\text{unique representation}}$
   - (iv) $(U_i)$ is direct sum family iff $0$ has unique representation (i.e., if $0 = \sum_{i \in F} u_i$ with $u_i \in U_i$, then all $u_i = 0$)
   
   State that (i) implies the rest needs field axioms (invertible nonzero elements), while the rest that imply (i) do not need them in the proof.

3. **Two-subspace criterion**:
   - For two subspaces the first characterization simplifies: $U \DirectSum W = U + W$ with the condition $U \Intersect W = \{0\}$
   - **Proof**: Specialize the general characterization to $I = \{1, 2\}$
   - **Decomposition**: $V = U \DirectSum W$ iff $V = U + W$ and $U \Intersect W = \{0\}$
   - **Examples**:
     - $\mathbb{R}^3 = $ (xy-plane) $\DirectSum$ (z-axis)
     - $K^n = \DirectSum{i=0}{n-1}{K \cdot e_i}$ (direct sum of coordinate axes, 0-indexed)
     - $K[X] = K[X]_{\text{even}} \DirectSum K[X]_{\text{odd}}$ (even/odd degree polynomials)
   - 🎨 **FIGURE**: $\mathbb{R}^3$ as direct sum of xy-plane and z-axis
   - **Non-example**: Two distinct lines through origin in $\mathbb{R}^3$ do NOT give $\mathbb{R}^3$ (only a plane)

4. **Definition (arbitrary family)**: $(U_i)_{i \in I}$ is an **internal direct sum family** (notation: $\DirectSum{i \in I}{U_i}$) iff it satisfies any of the above characterizations.

5. **Linear independence ↔ Direct sum family (Proposition)**:
   - Let $F \subseteq V$ and $(A_i)_{i \in I}$ partition $F$. Then $F$ is lin. indep. iff $(\Span[K]{A_i})_{i \in I}$ is a direct sum family
   - **Proof**: (⟹) If $0 = \sum u_i$ with $u_i \in \Span[K]{A_i}$ nonzero, expand each $u_i$ as linear combination in $A_i$; by partition, all vectors distinct; by lin. indep., all coefficients zero, contradiction. (⟸) If $0 = \sum a_v v$, group by partition index; direct sum gives each group sum zero; thus $G = \emptyset$.
   - **Corollary (Singleton Partition)**: $F$ is lin. indep. iff $(\Span[K]{\{v\}})_{v \in F}$ is a direct sum family with sum $\Span[K]{F}$

6. **Direct sum ↔ unions of lin. indep. (Proposition)**:
   - $(U_i)_{i \in I}$ is direct sum family iff for any lin. indep. sets $F_i \subseteq U_i$, $\bigcup_{i \in I} F_i$ is lin. indep.
   - **Proof**: (⟹) If $0 = \sum a_v v$ with $a_v \neq 0$, group by index, apply direct sum to get each inner sum $= 0$, then lin. indep. of $F_i$ gives all $a_v = 0$, so only the empty sum represents $0$. (⟸) If $0 = \sum u_i$ with $u_i \neq 0$, then each $\{u_i\} \subseteq U_i$ is lin. indep. (nonzero), so $\bigcup \{u_i\}$ is lin. indep. by assumption, but $0 = \sum 1 \cdot u_i$ is nontrivial, contradiction.

7. **$V$ decomposes as direct sum**: $V = \DirectSum{i \in I}{U_i}$ iff $(U_i)$ is direct sum family AND $V = \SubspaceSum{i \in I}{U_i}$
   - Equivalently: every $v \in V$ has unique representation in the family


---

## Phase 6: Bases and Dimension

1. **Definition**: $B$ is a **basis** iff $B$ is both linearly independent AND generating
   - **Examples**:
     - $\{(1,0), (0,1)\}$ is basis for $\mathbb{R}^2$ (standard basis)
     - $\{1, X, X^2, \ldots\}$ is basis for $K[X]$
     - $\{(1,1), (1,-1)\}$ is basis for $\mathbb{R}^2$ (non-standard)
   - 🎨 **FIGURE**: Standard basis vs rotated basis in $\mathbb{R}^2$
   - **Terminology**: A basis of infinite cardinality is sometimes called a **Hamel basis** (to distinguish from topological bases in functional analysis, called Schauder basis, these notions match if the basis is finite)
   - equivalent: **Basis decomposition**: $B$ basis iff $V = \DirectSum{v \in B}{\Span[K]{\{v\}}}$ (prove it)
2. **Observations (from above)**:
   - $\emptyset$ is always linearly independent
   - $V$ is always a generating system
3. **Maximal/Minimal characterizations** (with proofs):
   - Maximal lin. indep. $\Rightarrow$ generating (else could add a vector)
   - Minimal generating $\Rightarrow$ lin. indep. (else could remove a vector)
4. **Basis Extension Theorem** (Zorn's Lemma):
   - For $L$ lin. indep. and $G$ generating with $L \subseteq G$, there exists basis $B$ with $L \subseteq B \subseteq G$
   - **Proof structure** (simplified):
     1. Apply Zorn to get maximal $B$ in $\mathcal{F} = \{M : L \subseteq M \subseteq G, M \text{ lin. indep.}\}$
     2. $B$ is lin. indep. and maximal by construction. By Proposition, maximal lin. indep. $\Rightarrow$ basis.
5. **Corollaries of Basis Extension Theorem**:
   - Every vector space has a basis (take $L = \emptyset$, $G = V$)
   - **Extension to Basis**: Any lin. indep. set can be extended to a basis (take $G = V$)
   - **Extraction from Generating Set**: Any generating set contains a basis (take $L = \emptyset$)

6. **Complementary Subspaces**:
   - **Definition**: $W$ is a **complement** of $U$ in $V$ iff $V = U \DirectSum W$ (i.e., $U + W = V$ and $U \Intersect W = \{0\}$)
   - **Existence (Proposition)**: Every subspace $U \Subspace[K] V$ has a complement in $V$
     - **Proof**: Extend basis $B_U$ of $U$ to basis $B = B_U \Union C$ of $V$. Then $W = \Span[K]{C}$ is a complement.
   - **Non-Uniqueness (Corollary)**: Complements are generally **not unique**—different basis extensions yield different complements
   - **Example**: In $\mathbb{R}^2$, the $x$-axis has infinitely many complements: any non-horizontal line through origin (e.g., $y$-axis, line $y = x$, etc.)

7. **Steinitz Exchange Lemma** (Zorn's Lemma proof):
   - Let $L$ be linearly independent, $S$ spanning
   - **Statement**: There exists injection $f: L \to S$ such that $(S \setminus \DirectImage{f}{L}) \Union L$ spans $V$
   - **Proof structure**:
     1. **Define poset**: $\mathcal{P} = \{(L_0, S_0) : L_0 \subseteq L, S_0 \subseteq S, \exists$ bijection $g: L_0 \to S_0$, $(S \setminus S_0) \Union L_0$ spans$\}$
     2. **Apply Zorn**: Chains have upper bounds (union of $L_\alpha$, union of $S_\alpha$, combined bijection); spanning preserved since any $v$ is finite linear combination
     3. **Prove $\bar{L} = L$**: If $u \in L \setminus \bar{L}$, write $u = \sum a_i \ell_i + \sum b_j s_j$; some $b_j \neq 0$ (else $u \in \Span[K]{\bar{L}}$, contradicting lin. indep.); exchange $u \leftrightarrow s_j$ to extend, contradicting maximality

8. **Corollary**: $\Card{L} \leq \Card{S}$ (injection $f: L \to S$)

9. **Remark: Finite-Dimensional Case and Choice**:
   - If $V$ has a **finite generating set**, both Basis Extension and Steinitz hold **without** Axiom of Choice
   - Zorn's Lemma for finite posets reduces to exhaustive search—provable in ZF

10. **Dimension invariance**: Any two bases have the same cardinality (follows from Steinitz)
11. **Definition of dimension**: $\Dim[K]{V}$ = common cardinality of all bases
    - For infinite-dim: use actual cardinal ($\aleph_0$, $\mathfrak{c}$), NOT "$\infty$"
    - **Corollary (Dimension Zero)**: $\Dim{V} = 0$ iff $V = \{0\}$
    - **Corollary (Subspace Equal Dim)**: If $U \Subspace V$ finite-dim and $\Dim{U} = \Dim{V}$, then $U = V$
      - Proof: Basis of $U$ is lin indep of maximal size in $V$ ⟹ basis of $V$ ⟹ $U = V$
12. **Examples**:
    - **Finite-dim**: $K^n$ (dim $n$), polynomials of deg $\leq n$ (dim $n+1$), $\MatSpace{m}{n}[K]$ (dim $mn$)
    - **Infinite-dim**: $K[X]$ (all polynomials, dim $\aleph_0$), $K^{\mathbb{N}}$ (sequences, dim $\mathfrak{c}$), $C([0,1], \mathbb{R})$
13. **Cardinality comparisons**: $\Card{L} \leq \Dim[K]{V} \leq \Card{G}$
14. **Corollary**: Any lin indep system has at most $\Dim[K]{V}$ elements, any generating has at least.

15. **Grassmann formula**: $\Dim[K]{U + W} + \Dim[K]{U \Intersect W} = \Dim[K]{U} + \Dim[K]{W}$
    - **Proof**: Extend basis of $U \Intersect W$ to bases of $U$ and $W$ separately, show union is basis of $U + W$
    - **Direct sum corollary**: $\Dim[K]{U \DirectSum W} = \Dim[K]{U} + \Dim[K]{W}$ (since $U \Intersect W = \{0\}$)
    - **Basis union lemma**: If $(U_i)_{i \in I}$ is a direct sum family and $B_i$ is a basis for $U_i$, then $\bigcup_{i \in I} B_i$ is a basis for $\DirectSum{i \in I}{U_i}$
      - **Proof**: Generating clear (each $u_i \in \Span[K]{B_i}$). Lin. indep. by Phase 5 item 5: direct sum family ⟹ union of lin. indep. sets is lin. indep.
    - **Arbitrary direct sum**: $\Dim[K]{\DirectSum{i \in I}{U_i}} = \sum_{i \in I} \Dim[K]{U_i}$ (follows from basis union lemma)
    - **Inclusion-exclusion (finite)**: For finite $I$, $\Dim[K]{\sum_{i \in I} U_i} = \sum_{\emptyset \neq J \subseteq I} (-1)^{|J|+1} \Dim[K]{\bigcap_{j \in J} U_j}$
      - **Proof**: Induction on $|I|$, using Grassmann for $|I|=2$ as base case
    - 🎨 **FIGURE**: Two planes intersecting in a line in $\mathbb{R}^3$

16. **Cardinal Properties of Vector Spaces** (Theorem with detailed proof, requires $V \neq \{0\}$):
    - **(1)** Lower bound: $\max(\Card{K}, \Card{B}) \leq \Card{V}$
    - **(2)** Bijection with disjoint union: $V \sim \BigUnion[n \in \mathbb{N}] (K \setminus \{0\})^n \times \binom{B}{n}$
      - **PROVE EXPLICITLY**: Define map $\phi$, prove injectivity, surjectivity
      - Disjoint ⟹ can sum cardinals: $\Card{V} = \sum_{n \in \mathbb{N}} (\Card{K}-1)^n \cdot \binom{\Card{B}}{n}$
    - **(3)** Finite case (both the base and the field are finite): **State** $\binom{m}{n} = 0$ for $n > m$ ⟹ finite sum ⟹ **Newton's binomial**: $\Card{V} = \Card{K}^{\Card{B}}$
    - **(4)** Infinite case: If $\max(\Card{K}, \Card{B}) = \kappa \geq \aleph_0$, then $\Card{V} = \kappa$
    - Use `\sum_{n \in \mathbb{N}}` not `\sum_{n=0}^{\infty}`, `\Card{X}` macro

17. **Infinite-Dimensional Examples** (MUST come AFTER Cardinal Properties):
    - **$\Dim[K]{K[X]} = \aleph_0$** — countable basis $\{1, X, X^2, \ldots\}$
    - **$\Dim[\mathbb{Q}]{\mathbb{R}} = \mathfrak{c}$** — proof uses Cardinal Properties item (4)
    - **$\Dim[\mathbb{R}]{C([0,1], \mathbb{R})} = \mathfrak{c}$** — upper bound via evaluation, lower bound via $\{e^{ax}\}$
    - **ORDERING RULE**: These examples MUST appear AFTER the Cardinal Properties theorem since proofs use it

---

## Phase 7: Ordered Bases and Coordinates

1. **Motivation**: Writing is order-preserving; plain sets are not
2. **Definition: Ordered Basis**: A basis $B$ with a **well-ordering** $<$ on it
3. **Coordinates** (works for ANY dimension, including infinite):
   - For ordered basis $(b_i)_{i \in I}$ and $v = \sum_{i \in I} a_i b_i$ (finite support), coordinate vector has finitely many nonzero entries
   - **Infinite case**: Column vector with finitely many nonzero components
   - CRITICAL: Coordinates depend on the **ordering**
   - Examples showing ordering matters
4. **Coordinate Isomorphism**: $\phi_{\mathcal{B}}: V \to K^{(I)}$ is $K$-linear bijection
5. **Canonical Basis of $K^{(I)}$** (NOW prove it's a basis):
   - **Definition**: $(e_i)_{i \in I}$ where $e_i(j) = \delta_{ij}$
   - **Proposition**: $(e_i)_{i \in I}$ is a basis for $K^{(I)}$
   - **Proof**: Generating (expand any $f$ as $\sum f(i)e_i$), Lin. indep. (evaluate at $j$)
   - **Corollary**: $\Dim[K]{K^{(I)}} = \Card{I}$
   - **Remark (Canonicity is special to $K^{(I)}$)**: The basis $(e_i)_{i \in I}$ is called **canonical** because it is *defined by the structure of $K^{(I)}$ itself* — no arbitrary choice is involved. An abstract $K$-vector space $V$ has no canonical basis: every basis requires a choice (even $\mathbb{R}^n$ has many bases; the standard one is only preferred because we remember that $\mathbb{R}^n = K^{(n)}$ where $n = \{0,1,\ldots,n-1\}$). The canonical basis exists **only** for spaces of the form $K^{(I)}$.
6. **Standard Canonical Bases** (0-indexed):
   - $K^n = K^{(n)}$ where $n = \{0, 1, \ldots, n-1\}$: canonical basis $(e_0, e_1, \ldots, e_{n-1})$
   - $K^{(\kappa)}$: $(e_\alpha)_{\alpha < \kappa}$
   <!-- - $K[X]$: $(1, X, X^2, \ldots)$ -->
7. **Cardinal Properties via $K^{(I)}$** (corollary of coordinate isomorphism):
   - Since $V \cong K^{(I)}$, study cardinal properties of $K^{(I)}$:
     - (1) Lower bound: embeddings $k \mapsto k \cdot e_{i_0}$, $i \mapsto e_i$
     - (2) Bijection: disjoint union over support sizes
     - (3) Finite case: $K^{(I)} = K^I$ when both $K$ and $I$ are finite, so $\Card{K^{(I)}} = \Card{K}^{\Card{I}}$
     - (4) Infinite: $\sum_{n \in \mathbb{N}} \kappa = \aleph_0 \cdot \kappa = \kappa$
---

8. **Add macro**: `\CoordVec{v}[\mathcal{B}]` for coordinate vector

---

## Phase 7b: Flags and Chains of Subspaces

**File: `Document/LinearAlgebra/VectorSpaces/Foundations/Flags.tex`**

1. **Remark (Motivation)**: Flags are to subspaces what ordered bases are to vectors — they impose a linear order to capture geometry and enable normal forms (triangular matrices, Jordan form, Gaussian elimination).

2. **Remark (Total order vs.\ well-order for ordered bases)**:
   - **For coordinates only**: A total order on the basis suffices.
   - **For matrices**: A well-order is required to index by ordinals; without it there is no canonical first column for infinite bases.
   - In **finite dimensions** the distinction vanishes.

3. **Definition (Flag)**: A **flag** in $V$ is a chain of subspaces under strict inclusion. A finite flag of **length $r+1$** is $V_0 \subsetneq V_1 \subsetneq \cdots \subsetneq V_r$ ($r+1$ subspaces; no requirement on $V_0 = \{0\}$ or $V_r = V$). An infinite flag is indexed by a totally ordered set.
   - **Example in $\mathbb{R}^2$**: $\{0\} \subsetneq \mathbb{R}^2$ (length 2); $\{0\} \subsetneq \Span{e_1} \subsetneq \mathbb{R}^2$ (length 3); $\Span{e_2} \subsetneq \mathbb{R}^2$ (length 2, not starting at $\{0\}$). Infinitely many length-3 flags, one per line.
   - **Example in $K[X]_{\leqslant 2}$**: length 2 ($\{0\} \subsetneq K[X]_{\leqslant 2}$), length 3 ($\{0\} \subsetneq \Span{1} \subsetneq K[X]_{\leqslant 2}$, dims $0,1,3$), length 4 ($\{0\} \subsetneq \Span{1} \subsetneq K[X]_{\leqslant 1} \subsetneq K[X]_{\leqslant 2}$, dims $0,1,2,3$).

4. **Definition (Complete flag)**: A flag is **complete** if it is a maximal chain in the poset of subspaces — nothing can be inserted.
   - **Examples/counterexamples in $\mathbb{R}^3$**:
     - ❌ $\{0\} \subsetneq \mathbb{R}^3$ (length 2): can insert any line or plane.
     - ❌ $\{0\} \subsetneq \Span{e_1} \subsetneq \mathbb{R}^3$ (length 3, dims $0,1,3$): can insert $\Span{e_1,e_2}$ between the last two.
     - ✅ $\{0\} \subsetneq \Span{e_1} \subsetneq \Span{e_1,e_2} \subsetneq \mathbb{R}^3$ (length 4, dims $0,1,2,3$): complete.
     - ✅ $\{0\} \subsetneq \Span{e_1+e_2} \subsetneq \Span{e_1+e_2, e_3} \subsetneq \mathbb{R}^3$: another complete flag.

5. **Proposition (Complete flags, finite dims)**: For $\Dim[K]{V} = n$, a flag $V_0 \subsetneq \cdots \subsetneq V_r$ with $V_0 = \{0\}$, $V_r = V$ is complete iff $r = n$ and $\Dim[K]{V_i} = i$ for all $i$.
   - **Proof** ($\Rightarrow$): A gap of $\geq 2$ allows inserting an intermediate subspace by basis extension — contradicts maximality. ($\Leftarrow$): Any $W$ with $V_i \subsetneq W \Subspace[K] V_{i+1}$ has $\Dim[K]{W} = i+1 = \Dim[K]{V_{i+1}}$, so $W = V_{i+1}$ by the equal-dimension corollary.
   - **Example in $\mathbb{R}^4$**: $\mathcal{F}_1$ (dims $0,1,2,3,4$, length 5) complete; $\mathcal{F}_2$ (dims $0,1,3,4$, length 4) not complete — $\Span{e_1,e_2}$ fits in the gap.

6. **Proposition (Every ordered basis induces a complete flag — including infinite dims)**:
   Let $\mathcal{B} = (b_i)_{i \in I}$ be an ordered basis with $I$ well-ordered. Set $V_\alpha \defeq \Span[K]{\{b_i \mid i \leqslant \alpha\}}$. Then $(V_\alpha)_{\alpha \in I}$ (with $V_{-\infty} = \{0\}$ before the first index) is a complete flag.
   - **Proof**:
     - *Strict inclusion*: $b_{\alpha^+} \notin V_\alpha$ by linear independence, so $V_\alpha \subsetneq V_{\alpha^+}$.
     - *Covers $V$*: Every $v \in V$ is a finite combination $\sum_{j \in F} c_j b_j$; taking $\alpha = \max F$, $v \in V_\alpha$.
     - *Completeness*: If $V_\alpha \subsetneq W \Subspace[K] V_{\alpha^+}$, pick $w \in W \setminus V_\alpha$; write $w$ in the basis — the $b_{\alpha^+}$ coefficient is nonzero, so $b_{\alpha^+} \in W$, giving $V_{\alpha^+} \Subspace[K] W$, hence $W = V_{\alpha^+}$.
   - **Examples**: Standard basis of $\mathbb{R}^3$ gives the standard complete flag; canonical basis $(e_n)_{n \in \mathbb{N}}$ of $K^{(\mathbb{N})}$ gives $\{0\} \subsetneq \Span{e_0} \subsetneq \Span{e_0,e_1} \subsetneq \cdots$ (infinite complete flag).

7. **Remark (Converse, finite dims)**: Every complete flag of a finite-dimensional space admits an adapted ordered basis $(b_1,\ldots,b_n)$ with $V_i = \Span[K]{b_1,\ldots,b_i}$, built by choosing $b_i \in V_i \setminus V_{i-1}$ at each step.

---


## Chapter 1 Compendium

> **SECTION FORMATTING**: Every `\section{}` MUST be preceded by `\newpage` to ensure each section starts on a new page. The compendium is a separate section titled "Chapter Compendium".

> **COMPENDIUM STYLE**: Use `\section{Chapter Compendium}` followed by `\begin{compendium}[title]` environment. Formula-dense summary of ALL definitions, theorems, and formulas in compact notation.

---

### Minkowski Operations

**Definitions**: $\MinkowskiSum{A}{B} = \Set{a + b \mid a \in A, b \in B}$, $\MinkowskiProd{L}{A} = \Set{\lambda a \mid \lambda \in L, a \in A}$

**Monoid Structure**: $(A + B) + C = A + (B + C)$, $A + B = B + A$, $\{0\} + A = A$, $\emptyset + A = \emptyset$ (absorbing)

**Scalar Action**: $\{1_K\} \cdot A = A$, $(LM)A = L(MA)$, $\emptyset \cdot A = L \cdot \emptyset = \emptyset$, $\{0\} \cdot A = L \cdot \{0\} = \{0\}$

**Monotonicity**: $A \subseteq B \Rightarrow A + C \subseteq B + C$, $LA \subseteq LB$; $L \subseteq M \Rightarrow LA \subseteq MA$

**Distributivity over Unions**: $A + \BigUnion[i] B_i = \BigUnion[i](A + B_i)$, $L \cdot \BigUnion[i] A_i = \BigUnion[i] LA_i$

**Sub-Distributivity**: $L(A+B) \subseteq LA + LB$, $(L+M)A \subseteq LA + MA$

**Full Distributivity (singletons)**: $\lambda(A+B) = \lambda A + \lambda B$, $(L+M)\{a\} = L\{a\} + M\{a\}$

**Sub-Distributivity over Intersections**: $A + \BigIntersect[i] B_i \subseteq \BigIntersect[i](A + B_i)$

**Singleton Equality**: $\{a\} + \BigIntersect[i] B_i = \BigIntersect[i](\{a\} + B_i)$

---

### Subspaces

**Subspace Criterion**: $U \Subspace[K] V \Leftrightarrow U \neq \emptyset \land \Forall :{a,b \in K, u,v \in U}{au + bv \in U}$

**Minkowski Characterization**: $U$ subspace $\Leftrightarrow U \neq \emptyset \land U + U = U \land KU = U \Leftrightarrow U \neq \emptyset \land KU + KU = U$

**Operations**: $\BigIntersect[i \in I] U_i$ is subspace; $U \Union W$ subspace $\Leftrightarrow U \subseteq W \LogicOr W \subseteq U$

---

### Span

**Equivalences**:
$$\Span[K]{S} = \BigIntersect \Set{U \Subspace[K] V \mid S \subseteq U} = \Set{\sum_{i=1}^n a_i v_i \mid n \in \mathbb{N}, a_i \in K, v_i \in S}$$

**Properties**:
- $\Span[K]{\emptyset} = \{0\}$, $S \subseteq \Span[K]{S}$, $\Span[K]{\Span[K]{S}} = \Span[K]{S}$
- Monotonicity: $S \subseteq T \Rightarrow \Span[K]{S} \subseteq \Span[K]{T}$
- Subspace iff $\Span[K]{U} = U$
- $\Span[K]{S \Union T} = \Span[K]{\Span[K]{S} \Union \Span[K]{T}}$
- **Sum of spans = span of union**: $\SubspaceSum{i}{\Span[K]{S_i}} = \Span[K]{\BigUnion[i] S_i}$

---

### Linear Independence

**Equivalent characterizations** (all must be stated):
- (i) $\Forall :{v \in L}{\Span[K]{L \setminus \{v\}} \subsetneq \Span[K]{L}}$
- (i') $\Forall :{v \in L}{\Span[K]{\{v\}} \Intersect \Span[K]{L \setminus \{v\}} = \{0\}}$
- (ii) Every $v \in \Span[K]{L}$ has **unique representation**
- (iii) Some $v \in \Span[K]{L}$ has unique representation
- (iv) $0$ has unique representation (only trivial: all $a_i = 0$)

---

### Direct Sums

**Equivalent characterizations** (all must be stated):
- (i) $\Forall :{j \in I}{U_j \Intersect \SubspaceSum{i \neq j}{U_i} = \{0\}}$
- (ii) Every $v \in \SubspaceSum{i \in I}{U_i}$ has unique representation
- (iii) Some $v \neq 0$ has unique representation
- (iv) $0$ has unique representation

---

### Bases

**Basis**: $B$ lin. indep. AND $\Span[K]{B} = V$ $\Leftrightarrow$ maximal lin. indep. $\Leftrightarrow$ minimal generating

**Basis Extension** (Zorn): $L$ lin. indep. $\subseteq G$ generating $\Rightarrow \exists B$ basis with $L \subseteq B \subseteq G$

**Steinitz Exchange**: $L$ lin. indep., $S$ spanning $\Rightarrow \Card{L} \leq \Card{S}$

**Basis Union Lemma**: $(U_i)$ direct sum, $B_i$ basis of $U_i$ $\Rightarrow \BigUnion[i] B_i$ basis of $\DirectSum{i}{U_i}$

---

### Dimension

**Definition**: $\Dim[K]{V} = \Card{B}$ for any basis $B$; $\Card{L} \leq \Dim[K]{V} \leq \Card{G}$

**Grassmann Formula**: $\Dim[K]{U + W} + \Dim[K]{U \Intersect W} = \Dim[K]{U} + \Dim[K]{W}$

**Direct Sum Dimension**: $\Dim[K]{\DirectSum{i \in I}{U_i}} = \sum_{i \in I} \Dim[K]{U_i}$

**Cardinal Properties**: $\Card{V} = \sum_{n \in \mathbb{N}} (\Card{K}-1)^n \cdot \binom{\Card{B}}{n}$
- Finite: $K$ and $B$ both finite $\Rightarrow \Card{V} = \Card{K}^{\Card{B}}$ (Newton binomial, $\binom{m}{n} = 0$ for $n > m$)
- Infinite: $\max(\Card{K}, \Card{B}) = \kappa \geq \aleph_0 \Rightarrow \Card{V} = \kappa$

---

### Coordinates

**Coordinate Isomorphism**: $\phi_{\mathcal{B}}: V \xrightarrow{\sim} K^{(I)}$, $v \mapsto \CoordVec{v}[\mathcal{B}]$; $\Dim[K]{K^{(I)}} = \Card{I}$

**Key Dimensions**: $K^n$ ($n$), $K[X]_{\leq n}$ ($n+1$), $\MatSpace{m}{n}[K]$ ($mn$), $K[X]$ ($\aleph_0$), $\bb{R}/\bb{Q}$ ($\mathfrak{c}$), $C([0,1])$ ($\mathfrak{c}$)

---

# CHAPTER 2: Linear Maps

**File: `Document/LinearAlgebra/VectorSpaces/LinearMaps/`**

## Phase 9: Linear Transformations

### File: `Document/LinearAlgebra/LinearMaps/Definition.tex`

1. **Intuition Block (Category $K$-Vect)**:
   - Linear maps are morphisms in the category $K$-Vect
   - $\Hom[K]{U}{V} = \Set{T : U \to V \mid T \text{ is $K$-linear}}$

2. **Definition**: Preserves vector space structure $T(u + v) = T(u) + T(v)$ and $T(au) = aT(u)$ or equivalently $T(au + bv) = aT(u) + bT(v)$

3. **Composition of Linear Maps** (for category theory, right after definition):
   - $T, S$ linear $\Rightarrow$ $\Composition{S}{T}$ linear
   - Associativity, identity morphisms

4. **Basic Properties** (after composition):
   - $T(0) = 0$
   - $T(-v) = -T(v)$
   - $T(\text{linear combination}) = \text{linear combination of } T()$

5. **Examples**:
   - $T(v) = av$ for any $a \in K$
   - Identity transformation
   - Zero transformation
   - Inclusion
   - Derivative on $K[X]$
   - Evaluation at a point
   - **Coordinate projection** (general case): $\pi_i: K^I \to K$ and $\pi_i: K^{(I)} \to K$ with $\pi_i(f) = f(i)$
   
   📊 **FIGURE** (TikZ): Additivity - vectors $u, v$ and $u+v$ in domain map to $T(u), T(v), T(u+v)$ preserving parallelogram law.

6. **Universal Property of Vector Spaces (Free Objects)**:
   - For basis $B$ of $V$, any $f: B \to W$ extends uniquely to linear $T: V \to W$
   - 📊 **DIAGRAM** (tikz-cd): Triangle $B \xrightarrow{\iota} V \xrightarrow{\exists! T} W$ with $f: B \to W$
   - 📊 **FIGURE** (TikZ): Unit square (basis $\{e_0, e_1\}$) → parallelogram ($\{f(e_0), f(e_1)\}$) showing $T$ determined by basis images
   - 📊 **DIAGRAM**: Triangle showing $B \to V \to W$ with unique extension
   - **Equivalent characterizations** (without isomorphism):
     - $V$ has a basis
     - $V$ satisfies the universal property (free object)
     - $V = \bigoplus_{i \in I} \Span{v_i}$ for nonzero $(v_i)$ (internal direct sum)
   - **Proof (2)⟹(1)**: Elaborate - linear independence via contradiction with two extensions, generation via identity map uniqueness
   - **Corollary**: Every vector space is free (has a basis by Zorn's lemma)
   - **Corollary**: Linear maps are determined by their values on a basis
   - (Further isomorphism characterizations at Bijectivity section)

7. **Images and Preimages of Subspaces**:
   - $\DirectImage{T}{W}$ is subspace when $W$ is subspace
   - $\InverseImage{T}{W}$ is subspace when $W$ is subspace

8. **Fundamental Subspaces**:
   - $\Ker{T} = \InverseImage{T}{\{0\}}$
   - $\Rng{T} = \DirectImage{T}{U}$
   - Both are subspaces (prove using previous)

9. **Rank and Nullity**:
   - $\Rank{T} = \Dim[K]{\Rng{T}}$
   - $\Nullity{T} = \Dim[K]{\Ker{T}}$
   - **Corollaries** (after definition):
     - $\Rank{T} \leq \Dim{V}$ (since $\Rng{T} \Subspace V$)
     - $\Nullity{T} \leq \Dim{U}$ (since $\Ker{T} \Subspace U$)
     - $\Ker{T} = \{0\}$ iff $\Nullity{T} = 0$ (by dim=0 iff trivial)
     - $\Rng{T} = V$ iff $\Rank{T} = \Dim{V}$ (by subspace equal dim, requires $V$ finite-dim)

10. **Kernel and Range under Composition** (simplified):
    - $\Ker{T} \subseteq \Ker{\Composition{S}{T}}$
    - $\Rng{\Composition{S}{T}} \subseteq \Rng{S}$

---


## Phase 10: Injectivity, Surjectivity, Bijectivity

### File: `Document/LinearAlgebra/LinearMaps/Properties.tex`

**Order carefully: prove each implication, do not use characterizations not yet proved.**

### Injectivity

```latex
\begin{theorem}[Characterizations of Injectivity]
    For $T: U \to V$ linear, TFAE:
    \begin{enumerate}
        \item $T$ is injective
        \item $\Ker{T} = \{0\}$
        \item $T$ sends every linearly independent set to linearly independent set
        \item $T$ sends every basis to linearly independent set
        \item $T$ sends some basis to linearly independent set
        \item $T$ is a split monomorphism (has left inverse, when $U \neq \{0\}$)
        \item $T$ is monic (left-cancellable)
    \end{enumerate}
\end{theorem}
```

**Proof order**: (1)⟺(2)⟹(3)⟹(4)⟹(5)⟹(6)⟹(7)⟹(2).

**Proof elaborations**:
- **(1)⟺(2)**: $T(u_1)=T(u_2) \Rightarrow T(u_1-u_2)=0 \Rightarrow u_1-u_2 \in \Ker{T}$
- **(2)⟹(3)**: $\sum a_i T(\ell_i) = 0 \Rightarrow T(\sum a_i \ell_i) = 0 \Rightarrow \sum a_i \ell_i \in \Ker{T} = \{0\}$
- **(3)⟹(4)⟹(5)**: Every basis is lin indep → trivial implications
- **(5)⟹(6)**: Some basis $B$ sent to lin indep set $T[B]$. Extend $T[B]$ to basis of $V$, define $S(T(b))=b$, $S(c)=0$ on extension. By universal property $S \circ T = \mathrm{id}_U$
- **(6)⟹(7)**: If $S \circ T = \mathrm{id}$, then $T \circ S_1 = T \circ S_2 \Rightarrow S_1 = S \circ T \circ S_1 = S \circ T \circ S_2 = S_2$
- **(7)⟹(2)**: For $u \in \Ker{T}$, map $S_u: K \to U$ by $S_u(a)=au$. Then $T \circ S_u = T \circ 0$, so $S_u = 0$, so $u=0$

### Surjectivity

```latex
\begin{theorem}[Characterizations of Surjectivity]
    For $T: U \to V$ linear, TFAE:
    \begin{enumerate}
        \item $T$ is surjective
        \item $\DirectImage{T}{U} = V$
        \item $T$ sends every generating system to generating system
        \item $T$ sends every basis to generating system
        \item $T$ sends some basis to generating system
        \item $T$ is a split epimorphism (has right inverse, when $V \neq \{0\}$)
        \item $T$ is epic (right-cancellable)
    \end{enumerate}
\end{theorem}
```

**Proof order**: (1)⟺(2)⟹(3)⟹(4)⟹(5)⟹(6)⟹(7)⟹(2).

**Proof elaborations**:
- **(1)⟺(2)**: Definition of surjectivity
- **(2)⟹(3)**: By Span-Image commute: $\Rng{T} = T(\Span{G}) = \Span{T(G)}$
- **(3)⟹(4)⟹(5)**: Every basis is a generating set → trivial implications
- **(5)⟹(6)**: Some basis $B$ sent to gen set $T[B]$. Pick any $V$-basis, express each in $\Span{T[B]}$, define $S$ by preimage, extend linearly. Then $T \circ S = \mathrm{id}_V$
- **(6)⟹(7)**: If $T \circ S = \mathrm{id}$, then $S_1 \circ T = S_2 \circ T \Rightarrow S_1 = S_1 \circ T \circ S = S_2 \circ T \circ S = S_2$
- **(7)⟹(2)**: Consider $\pi: V \to V/\Rng{T}$ and $0: V \to V/\Rng{T}$. Since $\pi \circ T = 0 \circ T$, epicness gives $\pi = 0$, so $V = \Rng{T}$

### Bijectivity

```latex
\begin{theorem}[Characterizations of Bijectivity]
    For $T: U \to V$ linear, TFAE:
    \begin{enumerate}
        \item $T$ is bijective
        \item $T$ is injective and surjective
        \item $T$ sends every basis to a basis
        \item $T$ sends some basis to a basis
        \item $T$ is an isomorphism (has two-sided linear inverse)
        \item $T$ is monic and epic
    \end{enumerate}
\end{theorem}
```

**Proof order**: (1)⟺(2)⟹(3)⟹(4)⟹(5)⟹(6)⟹(2).

**Proof elaborations**:
- **(1)⟺(2)**: Definition of bijectivity
- **(2)⟹(3)**: Use injectivity char (3): $T[B]$ lin indep. Use surjectivity char (4): $T[B]$ generates. So $T[B]$ is basis
- **(3)⟹(4)**: Every basis is some basis
- **(4)⟹(5)**: $T[B]$ is basis ⟹ lin indep (so injective by char (5)⟹(2)) and generates (so surjective by char (5)⟹(2)). Thus bijective. Show $T^{-1}$ is linear: $T(au_1+bu_2) = av_1+bv_2 \Rightarrow T^{-1}(av_1+bv_2) = au_1+bu_2$
- **(5)⟹(6)**: Has left inverse ⟹ monic (by char (6)⟹(7)). Has right inverse ⟹ epic (by char (6)⟹(7))
- **(6)⟹(2)**: Monic ⟹ injective (by char (7)⟹(2)). Epic ⟹ surjective (by char (7)⟹(2))

**Definition: Inverse Linear Map**: For a bijective linear map $T: U \to V$, its inverse $T^{-1}: V \to U$ is linear.

**After bijectivity, add:**

1. **Two vector spaces are isomorphic iff they have the same dimension**

2. **Coordinate Isomorphism** (moved from Ordered Bases where it was just a bijection):
   - Extends the coordinate bijection to show it is a $K$-linear isomorphism
   - $\phi_{\mathcal{B}}: V \to K^{(I)}$ is a linear isomorphism
   - **Remark**: Different ordered bases give different isomorphisms. Example: $(e_0, e_1)$ vs $(e_1, e_0)$ give $\phi(3e_0+2e_1) = (3,2)$ vs $(2,3)$. Permuting basis = composing with permutation isomorphism on $K^{(I)}$.

3. **Corollary: $V \cong K^{(\Dim{V})}$**:
   - Every vector space with basis of cardinality $|I|$ is isomorphic to $K^{(I)}$
   - Proof: immediate from coordinate isomorphism

### Equal Dimension Equivalence

```latex
\begin{theorem}[Equal Dimension Equivalence]
    Let $U$ and $V$ be finite-dimensional $K$-vector spaces with $\Dim[K]{U} = \Dim[K]{V}$,
    and let $T: U \to V$ be a linear map. Then TFAE:
    \begin{enumerate}
        \item $T$ is injective
        \item $T$ is surjective
        \item $T$ is bijective
    \end{enumerate}
\end{theorem}
```

**Proof (WITHOUT rank-nullity, using basis characterizations)**:
- (1)⟹(3): $T$ sends basis $B$ of $U$ to lin indep set in $V$ of size $n = \Dim{V}$, hence a maximal lin indep ⟹ basis of $V$ ⟹ surjective
- (2)⟹(3): $T$ sends basis $B$ of $U$ to generating set of $V$; must have $n$ elements ⟹ minimal gen ⟹ basis ⟹ lin indep ⟹ injective

### Finite dimensional endomorphisms (Corollary)

```latex
\begin{corollary}[Endomorphism Equivalence]
    For $T: V \to V$ endomorphism on finite-dimensional $V$, TFAE:
    \begin{enumerate}
        \item $T$ is injective
        \item $T$ is surjective
        \item $T$ is bijective
    \end{enumerate}
\end{corollary}
```

**Proof**: Apply Equal Dimension Equivalence with $U = V$.

-Counterexamples if not endomorphism or if not finite dimensional (shift operators, inclusion/projection).

---

## Phase 9b: Hom Spaces as Vector Spaces

1. **$\Hom[K]{U}{V}$ is a $K$-vector space** (ALL 10 AXIOMS explicitly verified):
   - Closure under addition and scalar multiplication
   - Additive identity (zero map), additive inverse
   - Commutativity and associativity of addition
   - Compatibility of scalar and field multiplication
   - Identity element of scalar multiplication
   - Both distributivity laws
2. **$\End[K]{V}$ definition**: State it equals $\Hom{V}{V}$ AND that it has composition as multiplication: $S \cdot T = \Composition{S}{T}$
3. **$\End[K]{V}$ is a ring** with composition (use bracket protection in title: `[{$\End[K]{V}$ is a Ring}]`)
4. **$K$-algebra definition**: State compatibility $a(xy) = (ax)y = x(ay)$ WITH the definition, not separate
5. **$\End[K]{V}$ is a $K$-algebra**: unital, associative
6. **Non-Commutativity** (EXPLICIT EXAMPLE):
   - For $\Dim[K]{V} \geq 2$, pick linearly independent $v_0, v_1$, extend to basis
   - Define $S(v_0) = v_1$, $S(v_1) = 0$, $S(b) = 0$ for other basis elements
   - Define $T(v_0) = 0$, $T(v_1) = v_0$, $T(b) = 0$ for other basis elements
   - Compute $(\Composition{S}{T})(v_0) = 0 \neq v_0 = (\Composition{T}{S})(v_0)$
7. **Dimension of $\Hom[K]{U}{V}$ (finite-dimensional)**: $\Dim[K]{\Hom[K]{U}{V}} = m \cdot n$ where $m = \Dim[K]{U}$, $n = \Dim[K]{V}$
   - Proof: Construct explicit basis $\{T_{ij}\}$ where $T_{ij}(u_k) = \delta_{ik} v_j$ (all indices $0$-based: $i,k \in \{0,\ldots,m-1\}$, $j \in \{0,\ldots,n-1\}$). Show spanning (any $T$ is $\sum a_{ij} T_{ij}$ by expanding $T(u_i)$ in basis of $V$) and lin. indep. (evaluate at $u_k$, use lin. indep. of $v_j$'s). No matrices.
   - **Corollary**: $\Dim[K]{\End[K]{V}} = n^2$

---

<!-- ## Phase 10b: Rank-Nullity Theorem (via basis extension)

**Prove BEFORE quotient spaces, using basis extension directly.**

1. **Rank-Nullity**: $\Dim[K]{U} = \Rank{T} + \Nullity{T}$
   - Proof: Extend basis of $\Ker{T}$ to basis of $U$; show remaining basis vectors map to basis of $\Rng{T}$
2. **Corollaries** (finite-dimensional):
   - Injective + same dimension ⟹ surjective
   - Surjective + same dimension ⟹ injective -->

---

> **CHAPTER 2 COMPENDIUM** (at end of chapter, `Compendium.tex`):
> 
> 1. **Linear Transformations**: Definition, Category $K$-Vect, Composition (associative), Basic properties ($T(0)=0$)
> 2. **Universal Property (Free Objects)**: Basis extension, Equivalent characterizations (no isomorphism), Every VS is free, Maps determined by basis values
> 3. **Fundamental Subspaces**: $\Ker{T}$, $\Rng{T}$, Span-Image commute, Rank/Nullity definitions
> 4. **Injectivity Characterizations**: TFAE 7 items (injective, kernel=0, split mono, monic, every lin indep, every basis, some basis)
> 5. **Surjectivity Characterizations**: TFAE 7 items (surjective, image=V, split epi, epic, every gen set, every basis, some basis)
> 6. **Bijectivity and Isomorphism**: TFAE, $U \cong V$ iff same dim, Coordinate isomorphism, Equal Dimension Equivalence (inj⟺surj⟺bij when dim U = dim V), Endomorphism corollary
> 7. **Rank-Nullity Theorem**: Additive form, Proof idea (basis extension)
> 8. **Hom Spaces and Endomorphisms**: $\Hom[K]{U}{V}$ is VS, $\End[K]{V}$ is ring and $K$-algebra, Non-commutativity (dim ≥ 2)

---


---

# CHAPTER 3: Matrices & Subspace Representations

## Phase 16: Matrices

### Motivation (open the chapter with this remark — before any definition)

**Remark (Why Matrices?)**

We have built a rich theory of linear maps. But working with abstract maps is cumbersome for computation: to describe $T \in \Hom[K]{U}{V}$ we would need to specify $T(v)$ for every $v \in U$. By the universal property, $T$ is completely determined by its values on a basis $\mathcal{B}_U = (b_j)_{j \in J}$, reducing the data to the family $(T(b_j))_{j \in J}$ in $V$. Expressing each $T(b_j)$ in a basis $\mathcal{B}_V = (c_i)_{i \in I}$ of $V$ further reduces the information to a family of scalars $(a_{ij})_{i \in I, j \in J}$ with $T(b_j) = \sum_i a_{ij} c_i$. A **matrix** is precisely this rectangular array of scalars. It stores the action of $T$ in a form that is:
- **Finite** (finitely many nonzero entries per column, since $T(b_j) \in V$ has finite support in $\mathcal{B}_V$),
- **Computable** (finding $T(v)$ for any $v$ reduces to arithmetic on scalars),
- **Composable** (the matrix of $\Composition{S}{T}$ is determined by the matrices of $S$ and $T$).

However, the array itself depends on the choice of ordered bases. To define things properly, we first define the **matrix of $T$ as a linear map** (a basis-dependent isomorphism $K^{(\kappa_1)} \to K^{(\kappa_2)}$), and then separately define **matrices as purely combinatorial objects** (arrays of scalars). The connection between these two notions is made precise by the fundamental isomorphism of **16c**.

---

**Proposition (Existence and uniqueness of the scalar array)**:
Let $T: U \to V$ be $K$-linear, with ordered bases $\mathcal{B}_U = (b_j)_{j \in J}$ and $\mathcal{B}_V = (c_i)_{i \in I}$. For each $j \in J$ there exist **unique** scalars $(A_{ij})_{i \in I}$ with $A_{ij} \in K$ such that
\[
    T(b_j) = \sum_{i \in I} A_{ij}\, c_i
\]
and for each fixed $j$, only finitely many $A_{ij}$ are nonzero.

**Proof**: Each $T(b_j) \in V$. Since $\mathcal{B}_V$ is a basis of $V$, every vector in $V$ has a unique expression as a finite linear combination of $(c_i)_{i \in I}$. Applying this to $T(b_j)$ gives the unique scalars $A_{ij} = \CoordVec{T(b_j)}[\mathcal{B}_V](i)$, with only finitely many nonzero (finite support by definition of a basis). $\checkmark$

**Remark**: The double index $(i,j)$ is the fundamental datum of the chapter: $i$ picks the row (the target basis vector $c_i$) and $j$ picks the column (the source basis vector $b_j$). The family $(A_{ij})_{i \in I,\, j \in J}$ is the **matrix of $T$** with respect to $\mathcal{B}_U$ and $\mathcal{B}_V$. Everything that follows — column formula, matrix-times-vector, matrix multiplication — is a consequence of this single fact together with linearity.

---

### 16a: Matrix of a Linear Map

**Setup**: Let $U$ and $V$ be $K$-vector spaces with ordered bases $\mathcal{B}_U = (b_j)_{j \in J}$ and $\mathcal{B}_V = (c_i)_{i \in I}$, where $|J| = \kappa_1$ and $|I| = \kappa_2$ are cardinals. Let $T: U \to V$ be a $K$-linear map. Recall the coordinate isomorphisms $\phi_{\mathcal{B}_U}: U \xrightarrow{\sim} K^{(\kappa_1)}$ and $\phi_{\mathcal{B}_V}: V \xrightarrow{\sim} K^{(\kappa_2)}$ from Phase 7.

**Why $K^{(I)}$?** As established in Phase 7, abstract vector spaces have no canonical basis — any basis requires an arbitrary choice. The spaces $K^{(I)}$ are the exception: they carry the canonical basis $(e_i)_{i \in I}$, intrinsically defined by the function-space structure. This is the key reason we factor $T$ through coordinate spaces: once we pass to $K^{(\kappa_1)} \to K^{(\kappa_2)}$, the canonical bases are automatically available. The resulting map $[T]_{\mathcal{B}_U}^{\mathcal{B}_V}$ can then be interrogated on each $e_j$ — yielding the column formula — and stored as a plain array of scalars. **Matrices exist to exploit the canonical structure of $K^{(I)}$.**

1. **Definition (Matrix of $T$ w.r.t. $\mathcal{B}_U$, $\mathcal{B}_V$)**:
   - The **matrix of $T$ with respect to $\mathcal{B}_U$ and $\mathcal{B}_V$**  is the $K$-linear map
     \[
         [T]_{\mathcal{B}_U}^{\mathcal{B}_V} \defeq \Composition{\phi_{\mathcal{B}_V}}{\Composition{T}{\phi_{\mathcal{B}_U}^{-1}}} : K^{(\kappa_1)} \to K^{(\kappa_2)}
     \]
     This is the map making the following square commute:
     ```
     📊 DIAGRAM: commutative square
       U  --T-->  V
       |          |
      φ_BU      φ_BV
       |          |
       ↓          ↓
     K^(κ₁) --> K^(κ₂)
              [T]
     ```
   - In other words, $[T]_{\mathcal{B}_U}^{\mathcal{B}_V}$ expresses $T$ in coordinate language: it takes the coordinates of $u$ in $\mathcal{B}_U$ and returns the coordinates of $T(u)$ in $\mathcal{B}_V$.
   - **Note**: This definition requires **ordered** bases (to have a specific coordinate isomorphism); unordered bases give only an isomorphism up to reordering.

2. **Theorem (Column Formula)**:
   - The $j$-th column of $[T]_{\mathcal{B}_U}^{\mathcal{B}_V}$ (i.e., its value on $e_j \in K^{(\kappa_1)}$) equals the coordinate vector $\phi_{\mathcal{B}_V}(T(b_j))$.
   - **Proof**: $[T]_{\mathcal{B}_U}^{\mathcal{B}_V}(e_j) = \phi_{\mathcal{B}_V}(T(\phi_{\mathcal{B}_U}^{-1}(e_j))) = \phi_{\mathcal{B}_V}(T(b_j))$.
   - **Practical consequence**: To compute $[T]_{\mathcal{B}_U}^{\mathcal{B}_V}$, apply $T$ to each basis vector $b_j \in \mathcal{B}_U$, express the result $T(b_j)$ in the basis $\mathcal{B}_V$, and place those coordinates as the $j$-th column.
   - **Explicit entry**: Denote the scalars by $a_{ij}$ where $T(b_j) = \sum_{i} a_{ij} c_i$. Then $(\phi_{\mathcal{B}_V}(T(b_j)))_i = a_{ij}$, so the $(i,j)$-entry of the matrix equals $a_{ij}$.

3. **Proposition (Matrix-times-vector formula)**:
   - **Motivation**: The column formula gives the action of $[T]_{\mathcal{B}_U}^{\mathcal{B}_V}$ on every canonical basis vector $e_j$. Any $x = \sum_j x_j e_j \in K^{(\kappa_1)}$ (a finite sum) is determined by its coordinates $(x_j)$. Linearity of $[T]_{\mathcal{B}_U}^{\mathcal{B}_V}$ then forces:
     \[
         [T]_{\mathcal{B}_U}^{\mathcal{B}_V}(x)
         = \sum_j x_j \cdot [T]_{\mathcal{B}_U}^{\mathcal{B}_V}(e_j)
         = \sum_j x_j \cdot \phi_{\mathcal{B}_V}(T(b_j))
     \]
     Taking the $i$-th coordinate of both sides and using $a_{ij}$ for the entries:
     \[
         \left([T]_{\mathcal{B}_U}^{\mathcal{B}_V}(x)\right)_i = \sum_j a_{ij} x_j
     \]
     This is exactly the $i$-th row of $A$ dotted with $x$: the standard row-by-column product $(Ax)_i = \sum_j A_{ij} x_j$. The formula $[T]x = Ax$ is therefore **not a definition** — it is a **theorem** that follows solely from linearity and the column formula.
   - **Statement**: For any $u \in U$,
     \[
         \CoordVec{T(u)}[\mathcal{B}_V] = [T]_{\mathcal{B}_U}^{\mathcal{B}_V} \cdot \CoordVec{u}[\mathcal{B}_U]
     \]
   - **Proof**: Let $x = \CoordVec{u}[\mathcal{B}_U]$, so $u = \sum_j x_j b_j$. Then $T(u) = \sum_j x_j T(b_j) = \sum_j x_j \sum_i a_{ij} c_i = \sum_i \left(\sum_j a_{ij} x_j\right) c_i$. Hence the $i$-th coordinate of $T(u)$ in $\mathcal{B}_V$ is $\sum_j a_{ij} x_j = (Ax)_i$. $\checkmark$

4. **Composition rule**:
   - If $S: V \to W$ has matrix $[S]_{\mathcal{B}_V}^{\mathcal{B}_W}$, then the matrix of $\Composition{S}{T}$ is $[S]_{\mathcal{B}_V}^{\mathcal{B}_W} \circ [T]_{\mathcal{B}_U}^{\mathcal{B}_V}$ (composition of the two maps $K^{(\kappa_1)} \to K^{(\kappa_2)} \to K^{(\kappa_3)}$).

---

### 16b: Matrix Spaces $\MatSpace{X}{Y}[K]$ and $\CFMat{X}{Y}[K]$

**Motivation**: We now define matrices as *purely set-theoretic objects* — functions on a product of index sets — independent of any vector spaces. Two levels of structure arise naturally:

- The **general matrix space** $\MatSpace{X}{Y}[K] = K^{X \times Y}$ consists of all functions $X \times Y \to K$, with pointwise addition and scalar multiplication inherited from the product vector space structure. No finiteness condition is imposed.
- The **column-finite matrix space** $\CFMat{X}{Y}[K]$ is the subspace of matrices with finite support in each column. Column-finiteness is not arbitrary: it is the condition that $T(b_j) \in V$ imposes when expressing a linear map in a basis (each image is a *finite* linear combination of basis vectors).

**Motivation for matrix operations (transport of structure)**:
Matrix addition on $\MatSpace{X}{Y}[K]$ needs no extra motivation — it is the pointwise addition of functions. Matrix multiplication has no natural a priori definition, and is *forced upon us* by requiring the isomorphism $\Phi: \Hom[K]{K^{(\kappa_1)}}{K^{(\kappa_2)}} \xrightarrow{\sim} \CFMat{\kappa_2}{\kappa_1}[K]$ of **16c** to be an algebra morphism: since $\End[K]{K^{(\kappa)}}$ is a $K$-algebra with composition as multiplication, requiring $\Phi(\Composition{S}{T}) = \Phi(S) \cdot \Phi(T)$ forces $(AB)_{ij} = \sum_k A_{ik} B_{kj}$. Since composition involves only column-finite maps (those landing in $K^{(\kappa_2)}$), multiplication is defined only for column-finite matrices.

In other words: **matrix addition is defined on all matrices (product space structure); matrix multiplication is defined only for column-finite matrices, and its formula is transported from composition of linear maps.**

1. **Definitions**:

   **General matrix space**: Let $X, Y$ be sets (row and column index sets). A **$(X \times Y)$-matrix with entries in $K$** is a function
     \[
         A: X \times Y \to K
     \]
     Write $A_{ij}$ for $A(i,j)$ (row index $i \in X$, column index $j \in Y$). The set of all such matrices is
     \[
         \MatSpace{X}{Y}[K] \defeq K^{X \times Y}
     \]
     with no finiteness condition. For cardinals $\kappa_1, \kappa_2$, write $\MatSpace{\kappa_2}{\kappa_1}[K]$ ($\kappa_2$ rows, $\kappa_1$ columns).

   **Column-finite matrix space**: A matrix $A \in \MatSpace{X}{Y}[K]$ is **column-finite** if for each $j \in Y$, the column $A(-,j): i \mapsto A_{ij}$ has **finite support**: $A_{ij} = 0$ for all but finitely many $i \in X$. Equivalently, $A(-,j) \in K^{(X)}$ for all $j$. The set of column-finite matrices is
     \[
         \CFMat{X}{Y}[K] \defeq \Set{A \in \MatSpace{X}{Y}[K] \mid \forall j \in Y,\ A(-,j) \in K^{(X)}}
     \]
     This is a **subspace** of $\MatSpace{X}{Y}[K]$: for $A, B \in \CFMat{X}{Y}[K]$ and $\lambda \in K$, one has $(A+B)(-,j) = A(-,j) + B(-,j) \in K^{(X)}$ and $(\lambda A)(-,j) = \lambda A(-,j) \in K^{(X)}$.

   **Currying**: A matrix $A \in \MatSpace{X}{Y}[K]$ is equivalently a function $j \mapsto A(-,j) \in K^X$ (column map). For column-finite matrices: $\CFMat{X}{Y}[K] \cong (K^{(X)})^Y$ — functions from $Y$ to finitely-supported column vectors. This is **not** $K^{(X \times Y)}$: the latter requires globally finite support (only finitely many nonzero entries total), strictly stronger than column-finiteness when $Y$ is infinite.

2. **Vector space structure — matrix addition and scalar multiplication**:

   Since $\MatSpace{X}{Y}[K] = K^{X \times Y}$ is a product of copies of $K$, it is a $K$-vector space with pointwise operations:
   \[
       (A + B)_{ij} \defeq A_{ij} + B_{ij}, \qquad (\lambda A)_{ij} \defeq \lambda A_{ij}, \qquad 0_{ij} \defeq 0
   \]
   These are defined for **all** $A, B \in \MatSpace{X}{Y}[K]$, with no finiteness condition required. Since $\CFMat{X}{Y}[K]$ is a subspace (item 1), it is itself a $K$-vector space under the same operations.

   **Compatibility with $\Phi$**: The isomorphism $\Phi: \Hom[K]{K^{(\kappa_1)}}{K^{(\kappa_2)}} \xrightarrow{\sim} \CFMat{\kappa_2}{\kappa_1}[K]$ of **16c** is $K$-linear with respect to these definitions, since $\Phi(S+T)_{ij} = ((S+T)(e_j))_i = (S(e_j))_i + (T(e_j))_i = \Phi(S)_{ij} + \Phi(T)_{ij}$ and similarly for scalar multiplication. Thus the entrywise formulas are the *unique* definitions making $\Phi$ a linear map.

3. **Standard matrix operations** (state and prove):
   - **Matrix multiplication** (column-finite only):

     **Setup**: Let $A \in \CFMat{X}{Z}[K]$ and $B \in \CFMat{Z}{Y}[K]$.

     **Motivation**: We want $A \cdot B$ to correspond to composing the associated linear maps — i.e., if $T_A = \Phi^{-1}(A)$ and $T_B = \Phi^{-1}(B)$, then $AB$ should equal $\Phi(\Composition{T_A}{T_B})$.

     **Derivation of entry formula**: Compute the $j$-th column of $\Phi(\Composition{T_A}{T_B})$ using the column formula:
     \[
         \Phi(\Composition{T_A}{T_B})(-,j) = (\Composition{T_A}{T_B})(e_j) = T_A(T_B(e_j)) = T_A(B(-,j))
     \]
     Now apply the matrix-times-vector formula (proved in **16a.3**) to $T_A$ acting on $B(-,j) = \sum_k B_{kj} e_k$ (finite sum since $B(-,j) \in K^{(Z)}$ by column-finiteness of $B$):
     \[
         T_A(B(-,j)) = \sum_k B_{kj} \cdot T_A(e_k) = \sum_k B_{kj} \cdot A(-,k)
     \]
     The $i$-th entry of this column is:
     \[
         (AB)_{ij} \defeq \sum_{k \in Z} A_{ik} B_{kj}
     \]
     **Proof of well-definedness**: For fixed $j$, let $S_j = \{k \in Z : B_{kj} \neq 0\}$; this is finite by column-finiteness of $B$. Then $(AB)_{ij} = \sum_{k \in S_j} A_{ik} B_{kj}$ is a finite sum in $K$, well-defined for all $i$. Moreover, $\{i : (AB)_{ij} \neq 0\} \subseteq \bigcup_{k \in S_j} \{i : A_{ik} \neq 0\}$ is a finite union of finite sets (column-finiteness of $A$), so $(AB)(-,j) \in K^{(X)}$. Thus $AB \in \CFMat{X}{Y}[K]$. $\checkmark$

     **Why column-finite only**: Without column-finiteness of $B$, the sum $\sum_k A_{ik} B_{kj}$ may be infinite. Without column-finiteness of $A$, the result $AB$ may fail to be column-finite (its columns may have infinite support). Column-finiteness of both factors is the minimal condition ensuring the product is well-defined and stays in $\CFMat{X}{Y}[K]$.
   - **Hadamard product**: $(\Hadamard{A}{B})_{ij} = A_{ij} B_{ij}$ (pointwise; same size matrices only; defined on all of $\MatSpace{X}{Y}[K]$, and column-finiteness is preserved)
   - **Kronecker product**: $\KroneckerProduct[K]{A}{B}$ (block matrix structure)
   - **Transpose**: $(\Transpose{A})_{ij} = A_{ji}$; the transpose of $A \in \MatSpace{X}{Y}[K]$ lies in $\MatSpace{Y}{X}[K]$. The transpose of a column-finite matrix $A \in \CFMat{X}{Y}[K]$ is **not** in general column-finite as an element of $\MatSpace{Y}{X}[K]$: transposing turns columns of $A$ into rows of $\Transpose{A}$, and each row of $\Transpose{A}$ may have infinite support even if each column of $A$ does not. For finite matrices (both $X$ and $Y$ finite), $\Transpose{A} \in \CFMat{Y}{X}[K]$ always.
   - **Hermitian conjugate (conjugate transpose)**: $\Hermitian{A} \defeq \overline{\Transpose{A}}$, i.e. $(\Hermitian{A})_{ij} = \overline{A_{ji}}$. This requires $K$ to carry a conjugation involution (e.g. $K = \mathbb{C}$). Same column-finiteness caveat as for transpose.
   - **Determinant** (forward reference — fully developed in Chapter 7, Multilinear Algebra):
     - **Leibniz formula** (for square $n \times n$ matrices over a commutative ring):
       \[
           \Det{A} = \sum_{\sigma \in S_n} \operatorname{sgn}(\sigma) \prod_{i=1}^{n} A_{i,\sigma(i)}
       \]
       This formula will be derived in Chapter 7 as the unique alternating multilinear form normalized by $\Det{I_n} = 1$.
     - **Rank-based characterization**: $\Det{A} \neq 0$ iff $A$ is invertible (equivalently, $\Rank{A} = n$). State this as a theorem to be proved in Chapter 7.
     - For now: use $\Det{A}$ as notation and rely on the Leibniz formula when needed for small matrices.

4. **Sizes**:
   - **General matrices**: $\MatSpace{X}{Y}[K] = K^{X \times Y}$, canonically isomorphic to the product $\prod_{j \in Y} K^X$ (one copy of $K^X$ per column).
   - **Column-finite matrices**: Via currying, $\CFMat{X}{Y}[K] \cong \left(K^{(X)}\right)^Y$ — functions from $Y$ to finitely-supported column vectors. Explicitly, $A \leftrightarrow (A(-,j))_{j \in Y}$ is a bijection between $\CFMat{X}{Y}[K]$ and functions $Y \to K^{(X)}$.
   - **Contrast with $K^{(X \times Y)}$**: The space $K^{(X \times Y)}$ requires globally finite support (only finitely many nonzero entries in total). This is strictly stronger than column-finiteness when $Y$ is infinite: a column-finite matrix may have infinitely many nonzero columns, each individually finite.

---

### 16c: The Fundamental Isomorphism $\Hom[K]{K^{(\kappa_1)}}{K^{(\kappa_2)}} \cong \CFMat{\kappa_2}{\kappa_1}[K]$

1. **Theorem**: There is a canonical $K$-linear isomorphism
   \[
       \Phi: \Hom[K]{K^{(\kappa_1)}}{K^{(\kappa_2)}} \xrightarrow{\sim} \CFMat{\kappa_2}{\kappa_1}[K],
       \qquad T \mapsto \MatrixRep{T}, \quad \MatrixRep{T}_{ij} \defeq (T(e_j))_i
   \]
   i.e. the $(i,j)$-entry of $\MatrixRep{T}$ is the $i$-th coordinate of $T(e_j)$ in $K^{(\kappa_2)}$. The codomain is $\CFMat{\kappa_2}{\kappa_1}[K]$ (not the larger $\MatSpace{\kappa_2}{\kappa_1}[K]$): column-finiteness is not imposed externally but is a *consequence* of $T$ mapping into $K^{(\kappa_2)}$.

   - **Well-definedness** (image lands in $\CFMat{\kappa_2}{\kappa_1}[K]$): Each $T(e_j) \in K^{(\kappa_2)}$ has finite support in $i$, so $\MatrixRep{T}(-,j) \in K^{(\kappa_2)}$, i.e. $\MatrixRep{T}$ is column-finite. $\checkmark$
   - **$K$-linearity of $\Phi$**: $(\MatrixRep{\alpha T + \beta S})_{ij} = ((\alpha T{+}\beta S)(e_j))_i = \alpha \MatrixRep{T}_{ij} + \beta \MatrixRep{S}_{ij}$. $\checkmark$
   - **Injectivity**: If $\MatrixRep{T} = 0$, then $T(e_j) = 0$ for all $j$, so $T = 0$ (linear maps determined by basis values). $\checkmark$
   - **Surjectivity + matrix-times-column formula**: Given $A \in \CFMat{\kappa_2}{\kappa_1}[K]$, define $T_A(e_j) \defeq A(-,j) \in K^{(\kappa_2)}$ (well-defined since $A$ is column-finite) and extend linearly. Then $\MatrixRep{T_A} = A$. Moreover, for any $x = \sum_j x_j e_j \in K^{(\kappa_1)}$ (a finite sum):
     \[
         T_A(x) = \sum_j x_j A(-,j), \qquad (T_A(x))_i = \sum_j A_{ij} x_j = (Ax)_i
     \]
     so $T_A(x) = Ax$, i.e. $\ColVec{T_A(x)} = A \cdot \ColVec{x}$. Every $A \in \CFMat{\kappa_2}{\kappa_1}[K]$ acts on $K^{(\kappa_1)} \to K^{(\kappa_2)}$ by left multiplication. $\checkmark$

   **Remark**: A general matrix $A \in \MatSpace{\kappa_2}{\kappa_1}[K]$ also acts on $K^{(\kappa_1)}$ by $x \mapsto Ax$ (the sum $\sum_j A_{ij} x_j$ is finite since $x$ has finite support), but the result lies in $K^{\kappa_2}$, not necessarily in $K^{(\kappa_2)}$. Column-finiteness of $A$ is precisely the condition guaranteeing $Ax \in K^{(\kappa_2)}$.

2. **Special cases (must verify explicitly)**:
   - **Zero matrix = zero map**: $\MatrixRep{0}_{ij} = (0 \cdot e_j)_i = 0$. $\checkmark$
   - **Identity matrix = identity map**: $\MatrixRep{\Identity{K^{(\kappa)}}}_{ij} = (e_j)_i = \delta_{ij} = (I_\kappa)_{ij}$. $\checkmark$
   - **Scalar matrix**: $\MatrixRep{\lambda \Identity{K^{(\kappa)}}} = \lambda I_\kappa$. $\checkmark$

3. **Matrix-times-column for general bases**:
   - For $U$, $V$ with ordered bases $\mathcal{B}_U$, $\mathcal{B}_V$ and $T: U \to V$, the formula from surjectivity above applies to $\MatrixRep{T}[\mathcal{B}_U][\mathcal{B}_V] = \phi_{\mathcal{B}_V} \circ T \circ \phi_{\mathcal{B}_U}^{-1}$. For any $v \in U$:
     \[
         \ColVec{T(v)}[\mathcal{B}_V] = \MatrixRep{T}[\mathcal{B}_U][\mathcal{B}_V] \cdot \ColVec{v}[\mathcal{B}_U]
     \]
     **Proof**: $\MatrixRep{T}[\mathcal{B}_U][\mathcal{B}_V] \cdot \phi_{\mathcal{B}_U}(v) = (\phi_{\mathcal{B}_V} \circ T \circ \phi_{\mathcal{B}_U}^{-1})(\phi_{\mathcal{B}_U}(v)) = \phi_{\mathcal{B}_V}(T(v)) = \ColVec{T(v)}[\mathcal{B}_V]$. $\checkmark$
   - **This is the operational content of the definition in 16a**: the matrix transforms coordinate columns.

4. **Composition = matrix multiplication**:
   - $\Phi(\Composition{S}{T}) = \Phi(S) \cdot \Phi(T)$, i.e. $\MatrixRep{\Composition{S}{T}} = \MatrixRep{S} \cdot \MatrixRep{T}$.
   - **Proof using the matrix-times-column formula**: For any $x \in K^{(\kappa_1)}$:
     \[
         \MatrixRep{\Composition{S}{T}} \cdot x = (\Composition{S}{T})(x) = S(T(x)) = S(\MatrixRep{T} \cdot x) = \MatrixRep{S} \cdot (\MatrixRep{T} \cdot x) = (\MatrixRep{S} \cdot \MatrixRep{T}) \cdot x
     \]
     Since this holds for all $x$, $\MatrixRep{\Composition{S}{T}} = \MatrixRep{S} \cdot \MatrixRep{T}$. $\checkmark$
   - **Corollary**: $\Phi$ is an isomorphism of $K$-algebras $\End[K]{K^{(\kappa)}} \xrightarrow{\sim} \CFMat{\kappa}{\kappa}[K]$:
     - Adding maps ↔ adding matrices
     - Composing maps ↔ multiplying matrices
     - Zero map ↔ zero matrix
     - Identity map ↔ identity matrix

5. **General Hom via bases**:
   - For $U$, $V$ with ordered bases $\mathcal{B}_U$, $\mathcal{B}_V$, the map $T \mapsto \MatrixRep{T}[\mathcal{B}_U][\mathcal{B}_V]$ is a $K$-linear isomorphism:
     \[
         \Hom[K]{U}{V} \xrightarrow{\;\sim\;} \CFMat{|\mathcal{B}_V|}{|\mathcal{B}_U|}[K]
     \]
     The isomorphism lands in the **column-finite** space: for any $T \in \Hom[K]{U}{V}$ and any $j \in \mathcal{B}_U$, the image $T(b_j) \in V$ has finite support in $\mathcal{B}_V$ (by definition of a basis), so the $j$-th column of $\MatrixRep{T}$ is finitely supported. Different choices of bases yield different but isomorphic identifications (related by change-of-basis matrices, Phase 17).

---

<!-- ### 16d: Block Matrices (with all the proofs)
1. **Block matrix**: Matrix with matrix entries that unravel to rectangles
2. **Compatibility**: Row blocks must have same height, column blocks same width
3. **Unraveling morphism property**: For operations $\circ \in \{+, \cdot, \otimes_K, \circ_{\text{Hadamard}}\}$:
   - $\text{unravel}(A \circ B) = \text{unravel}(A) \circ \text{unravel}(B)$
   - i.e., partition into blocks → apply operation as block matrices → unravel = operate as regular matrices -->

---

### 16e: Matrices as Linear Maps (Canonical Interpretation)
- Since $A \in \CFMat{\kappa_2}{\kappa_1}[K]$ corresponds canonically to a linear map $\Phi^{-1}(A): K^{(\kappa_1)} \to K^{(\kappa_2)}$ via the isomorphism of **16c**, we treat any column-finite matrix as a linear map without further comment. This is **not** an abuse of notation — it is a direct consequence of the definition.
- In particular, $\Ker{A}$ and $\Rng{A}$ are well-defined:
  \[
      \Ker{A} \defeq \Set{x \in K^{(\kappa_1)} \mid Ax = 0}, \qquad \Rng{A} \defeq \Set{Ax \mid x \in K^{(\kappa_1)}}
  \]
  where $Ax \defeq \Phi^{-1}(A)(x) = \sum_{j \in \kappa_1} x_j \cdot A(-,j) \in K^{(\kappa_2)}$ (finite sum since $x$ has finite support).
- **For $K^n$**: When working in $K^n$, the canonical bases are always in force, so $A \in \CFMat{m}{n}[K]$ acts on $K^n$ directly via $x \mapsto Ax$ (for finite $m, n$, $\CFMat{m}{n}[K] = \MatSpace{m}{n}[K]$).

---

### 16f: Linear Systems
1. **Homogeneous system**: $Ax = 0$
   - Solution space = $\Ker{T_A}$ where $T_A: K^{(\kappa_1)} \to K^{(\kappa_2)}$
   - Always a subspace
2. **Non-homogeneous system**: $Ax = b$
   - Solution set = $v_0 + \Ker{T_A}$ (affine subspace/coset)
   - Non-empty iff $b \in \Rng{A}$
3. **Parametric equations**: Express solution in terms of free variables
4. **Implicit equations**: System of equations defining a subspace

---

## Phase 17: Change of Basis and Gaussian Elimination

1. **Change of basis matrices**: Invertible; columns are coordinates of new basis in terms of old
   - 📊 **DIAGRAM**: Commutative square $V \xrightarrow{T} W$ with coordinate maps $K^n \to K^m$
2. **Similarity**: $A' = P^{-1}AP$ for endomorphisms
3. **Equivalence**: $A' = QAP^{-1}$ for general linear maps
4. **RREF, CREF, REF** as equivalence relations
5. **Kernel/Range invariance** under row/column operations

### RREF/CREF and Map Properties
6. **Injectivity**: RREF has no zero rows iff injective (full column rank)
7. **Surjectivity**: CREF has no zero columns iff surjective (full row rank)
8. **Bijectivity**: Square matrix in RREF = identity iff bijective

### Gauss-Jordan Elimination for Inverses
9. **Left inverse** (when injective): Augment $[A | I]$, row reduce to $[R | L]$, then $LA = R$
10. **Right inverse** (when surjective): Augment $\begin{pmatrix} A \\ I \end{pmatrix}$, column reduce
11. **Two-sided inverse**: $[A | I] \to [I | A^{-1}]$ via Gauss-Jordan

---

## Phase 18: Subspace Representations and Equations

### Core Forms
1. **Every subspace = kernel of some linear map**:
   - Given $U \Subspace[K] V$, construct $\pi: V \to \QuotientSpace{V}{U}$ with $\Ker{\pi} = U$
   - Every subspace is the kernel of an endomorphism
2. **Every subspace = image of some linear map**:
   - Given $U \Subspace[K] V$, take inclusion $\iota: U \hookrightarrow V$
   - Every subspace is the image of an endomorphism
3. **Parametric form** (image): $U = \Span[K]{\{v_1, \ldots, v_k\}}$
4. **Implicit form** (kernel): $U = \Set{v \mid L_1(v) = \cdots = L_r(v) = 0}$ for functionals $L_i$

- Examples

### Converting Between Forms
- **Parametric → Implicit**: Find functionals vanishing on generating set (solve homogeneous system)
- **Implicit → Parametric**: Solve the homogeneous system to get generating set

### Operations on Subspaces
| Operation | Easier with | Method |
|-----------|-------------|--------|
| $U + W$ | Parametric | Concatenate generating sets |
| $U \Intersect W$ | Implicit | Concatenate equation systems |
| $\DirectImage{T}{U}$ | Parametric | Apply $T$ to generators |
| $\InverseImage{T}{W}$ | Implicit | Compose equations with $T$: $\Composition{L_i}{T} = 0$ |

### Direct Image $\DirectImage{T}{U}$ (Image of a Subspace)
**Easier with Parametric representation of $U$:**
- If $U = \Span[K]{\{u_1, \ldots, u_k\}}$, then $\DirectImage{T}{U} = \Span[K]{\{T(u_1), \ldots, T(u_k)\}}$
- Simply apply $T$ to each generator
- **Why implicit is harder**: If $U = \Ker{L}$ (implicit), then $\DirectImage{T}{U} = \DirectImage{T}{\Ker{L}}$ requires finding generators of $\Ker{L}$ first, then applying $T$

### Inverse Image $\InverseImage{T}{W}$ (Preimage of a Subspace)
**Easier with Implicit representation of $W$:**
- If $W = \Set{v \mid L_1(v) = \cdots = L_r(v) = 0}$, then $\InverseImage{T}{W} = \Set{u \mid L_1(T(u)) = \cdots = L_r(T(u)) = 0}$
- Simply compose each defining functional with $T$: the equations become $(\Composition{L_i}{T})(u) = 0$
- **Why parametric is harder**: If $W = \Span[K]{\{w_1, \ldots, w_k\}}$, finding $\InverseImage{T}{W}$ requires solving $T(u) = \sum a_j w_j$, which is a parametric equation in $u$ and $(a_j)$—more complex

### Summary Table (Expanded)
| Operation | Representation | Formula / Method |
|-----------|----------------|------------------|
| $\DirectImage{T}{U}$ | $U$ parametric | $\DirectImage{T}{\Span[K]{G}} = \Span[K]{\DirectImage{T}{G}}$ |
| $\DirectImage{T}{U}$ | $U$ implicit | Convert to parametric first, then apply $T$ |
| $\InverseImage{T}{W}$ | $W$ implicit | $\InverseImage{T}{\Ker{L}} = \Ker{\Composition{L}{T}}$ |
| $\InverseImage{T}{W}$ | $W$ parametric | Solve $T(u) \in W$; convert $W$ to implicit first |

### Formulas
- **Sum (implicit)**: Requires elimination; $U + W = \pi_V(\Set{(u,w) \mid u-w=0})$
- **Intersection (parametric)**: Requires solving $\sum a_i u_i = \sum b_j w_j$

---

## Phase 19: Rank and Nullity Inequalities

1. **$\Ker{\Composition{S}{T}} \supseteq \Ker{T}$**
2. **$\DirectImage{S}{V} \supseteq \DirectImage{\Composition{S}{T}}{U}$**
3. **Rank bounds**: $|\Rank{T} - \Rank{S}| \leq \Rank{T \pm S} \leq \Rank{T} + \Rank{S}$
4. **$\Rank{\Composition{S}{T}} \leq \min\{\Rank{S}, \Rank{T}\}$**
5. **Nullity counterexample**: $\Nullity{\Composition{S}{T}} \geq \Nullity{S}$ is FALSE in general

---

## Phase 20: Sylvester and Frobenius Inequalities

1. **Full rank factorization**
2. **Sylvester**: $\Dim[K]{\Ker{\Composition{S}{T}}} = \Dim[K]{\Ker{T}} + \Dim[K]{\Ker{S} \Intersect \DirectImage{T}{U}}$ (give the rank equivalent)
3. **Frobenius**: $\Dim[K]{\Ker{\Composition{A}{\Composition{B}{C}}}} + \Dim[K]{\Ker{B}} \leq \Dim[K]{\Ker{\Composition{A}{B}}} + \Dim[K]{\Ker{\Composition{B}{C}}}$ (give the rank equivalent)
4. **Generalization to $n$ maps**

---

> **CHAPTER 3 COMPENDIUM**: Add compendium here summarizing matrix representations, change of basis.

---


# CHAPTER 4: Quotient Spaces & Isomorphism Theorems

**File: `Document/LinearAlgebra/VectorSpaces/QuotientSpaces/`**

## Phase 11: Quotient Spaces

### File: `Document/LinearAlgebra/VectorSpaces/QuotientSpaces/Construction.tex`

1. **Construction**: Equivalence $u \sim v$ iff $u - v \in W$
   - **Prove explicitly** that the equivalence class of $v$ equals the coset $v + W$
2. **Vector space structure** (verify well-definedness, axioms)
3. **Canonical projection**: $\pi: V \to \QuotientSpace{V}{W}$, $\pi(v) = \EquivClass{v}$
   - $\pi$ is surjective linear (epimorphism)
   - $\Ker{\pi} = W$

**Examples with Figures** (add after construction):

- **Example 1: $\bb{R}^2 / L$ where $L$ is a line through origin**
  - Each coset is a parallel line to $L$
  - 📊 **FIGURE** (TikZ): $\bb{R}^2$ with line $L$ through origin, showing parallel lines as cosets, arrow to 1D quotient space
  
- **Example 2: $\bb{R}^3 / P$ where $P$ is a plane through origin**
  - Each coset is a plane parallel to $P$
  - The quotient is 1-dimensional (normal direction)
  - 📊 **FIGURE** (TikZ): 3D view with plane $P$ and parallel planes, arrow showing projection to 1D line
  
- **Example 3: $K[X] / \Span{X^2}$ (polynomials mod $X^2$)**
  - $p(X) \sim q(X)$ iff $p - q$ is divisible by $X^2$
  - Equivalently: keep only constant and linear terms
  - $\Dim{K[X]/\Span{X^2}} = 2$ (basis: $\EquivClass{1}, \EquivClass{X}$)

4. **Dimension formula**: $\Dim[K]{W} + \Dim[K]{\QuotientSpace{V}{W}} = \Dim[K]{V}$ (additive form; prove via basis extension)
   - Only on finite dimensions can we say $\Dim[K]{\QuotientSpace{V}{W}} = \Dim[K]{V} - \Dim[K]{W}$

**Hyperplanes (move to AFTER Isomorphism Theorems in file order):**

<!-- ### File: `Document/LinearAlgebra/VectorSpaces/QuotientSpaces/Hyperplanes.tex` -->

9. **Universal Property** (CRITICAL - prove fully):

📊 **DIAGRAM**: Canonical triangle $V \xrightarrow{\pi} \QuotientSpace{V}{W} \xrightarrow{\tilde{T}} X$ with $T = \Composition{\tilde{T}}{\pi}$
```latex
\begin{theorem}[Universal Property of Quotient Spaces]
    Let $W \Subspace V$ and $T: V \to X$ linear. Then:
    \begin{enumerate}
        \item There exists $\tilde{T}: \Quotient{V}{W} \to X$ with 
            $T = \Composition{\tilde{T}}{\pi}$ if and only if $W \subseteq \Ker{T}$.
        \item If such $\tilde{T}$ exists, it is unique.
        \item $\tilde{T}$ is surjective iff $T$ is surjective.
        \item $\tilde{T}$ is injective iff $\Ker{T} = W$.
        \item $\tilde{T}$ is an isomorphism iff $T$ is surjective and $\Ker{T} = W$.
    \end{enumerate}
\end{theorem}
```

**Proof** (use epimorphism/monomorphism characterizations):
- (1) ($\Rightarrow$): If $\tilde{T}$ exists, $\Ker{T} = \Ker{\Composition{\tilde{T}}{\pi}} \supseteq \Ker{\pi} = W$.
- (1) ($\Leftarrow$): Define $\tilde{T}(\EquivClass{v}) = T(v)$. Well-defined since $\EquivClass{v} = \EquivClass{u}$ implies $v - u \in W \subseteq \Ker{T}$.
- (2): $\pi$ is an epimorphism (surjective), so $\Composition{\tilde{T}_1}{\pi} = \Composition{\tilde{T}_2}{\pi}$ implies $\tilde{T}_1 = \tilde{T}_2$ by cancellation.
- (3): $\Rng{T} = \DirectImage{\tilde{T}}{\Rng{\pi}} = \Rng{\tilde{T}}$ since $\pi$ is surjective. Hence $T$ surj $\Leftrightarrow$ $\tilde{T}$ surj.
- (4): $\tilde{T}$ monomorphism: $\tilde{T}(\pi(v)) = 0$ implies $\pi(v) = 0$ iff $v \in W$. So $\Ker{T} = W$ iff $\tilde{T}$ injective.
- (5): Combine (3) and (4).

---

## Phase 12: Isomorphism Theorems

### File: `Document/LinearAlgebra/LinearMaps/IsomorphismTheorems.tex`

**Prove using the universal property of quotient spaces, NOT rank-nullity.**

### First Isomorphism Theorem

📊 **DIAGRAM**: $U \xrightarrow{T} V$ factors through $\QuotientSpace{U}{\Ker{T}} \cong \Rng{T}$

```latex
\begin{theorem}[First Isomorphism Theorem]
    For $T: U \to V$ linear:
    \[
        \Quotient{U}{\Ker{T}} \cong \DirectImage{T}{U}
    \]
\end{theorem}
```

**Proof**: Apply universal property with $W = \Ker{T}$. Since $\Ker{T} \subseteq \Ker{T}$, we get $\tilde{T}: \Quotient{U}{\Ker{T}} \to V$. By condition (4), $\tilde{T}$ is injective since $\Ker{T} = \Ker{T}$. The image is $\DirectImage{T}{U}$.

**Corollary (Rank-Nullity)**: This provides an alternative derivation of rank-nullity via dimensions.

### Second Isomorphism Theorem

📊 **DIAGRAM**: Diamond with $U$, $W$, $U+W$, $U \Intersect W$ and quotient isomorphism

```latex
\begin{theorem}[Second Isomorphism Theorem]
    Let $U, W \Subspace V$. Then:
    \[
        \Quotient{(U + W)}{W} \cong \Quotient{U}{(U \Intersect W)}
    \]
\end{theorem}
```

**Proof**: Consider inclusion $\iota: U \hookrightarrow U + W$ composed with projection $\pi: U + W \to \Quotient{(U+W)}{W}$. Show $\Ker{\Composition{\pi}{\iota}} = U \Intersect W$. Apply first isomorphism theorem.

**Corollary (Grassmann via Isomorphism)**:
- From $\Quotient{(U + W)}{W} \cong \Quotient{U}{(U \Intersect W)}$, we get $\Dim[K]{\Quotient{(U + W)}{W}} = \Dim[K]{\Quotient{U}{(U \Intersect W)}}$
- Using additive dimension formulas: $\Dim[K]{W} + \Dim[K]{\Quotient{(U + W)}{W}} = \Dim[K]{U + W}$ and $\Dim[K]{U \Intersect W} + \Dim[K]{\Quotient{U}{(U \Intersect W)}} = \Dim[K]{U}$
- Combining: $\Dim[K]{U + W} + \Dim[K]{U \Intersect W} = \Dim[K]{U} + \Dim[K]{W}$ (ADDITIVE form)
- **Finite-dimensional only**: If $\Dim[K]{V}$ is finite, then $\Dim[K]{U + W} = \Dim[K]{U} + \Dim[K]{W} - \Dim[K]{U \Intersect W}$

### Third Isomorphism Theorem

📊 **DIAGRAM**: Nested quotients $V \to \QuotientSpace{V}{U} \to \QuotientSpace{(V/U)}{(W/U)} \cong \QuotientSpace{V}{W}$

```latex
\begin{theorem}[Third Isomorphism Theorem]
    Let $U \subseteq W \subseteq V$ be subspaces. Then:
    \[
        \Quotient{\Quotient*{V}{U}}{\Quotient*{W}{U}} \cong \Quotient{V}{W}
    \]
\end{theorem}
```

**Corollary (Dimension Formula)**: We cannot subtract dimensions of subspaces in general, but we can "cancel" them due to the third isomorphism theorem.

### Fourth (Lattice) Isomorphism Theorem

Correspondence between subspaces of $\Quotient{V}{W}$ and subspaces of $V$ containing $W$.

---

### CHAPTER 4 COMPENDIUM

**Quotient Space Construction**:
- Equivalence: $u \sim_W v \Leftrightarrow u - v \in W$
- Coset: $v + W = \Set{v + w \mid w \in W}$
- Operations: $(u+W) + (v+W) = (u+v)+W$, $a(v+W) = av+W$
- Canonical projection: $\pi: V \to \QuotientSpace{V}{W}$, $\Ker{\pi} = W$

**Dimension Formula**:
- Additive: $\Dim{W} + \Dim{\QuotientSpace{V}{W}} = \Dim{V}$ (all dimensions)
- Subtraction form only for finite-dimensional

**Universal Property**:
- $\tilde{T}$ exists with $T = \tilde{T} \circ \pi$ iff $W \subseteq \Ker{T}$
- $\tilde{T}$ surj iff $T$ surj; $\tilde{T}$ inj iff $\Ker{T} = W$; $\tilde{T}$ iso iff both

**Isomorphism Theorems**:
1. First: $\QuotientSpace{U}{\Ker{T}} \cong \Rng{T}$ → Rank-Nullity
2. Second: $\QuotientSpace{(U+W)}{W} \cong \QuotientSpace{U}{(U \cap W)}$ → Grassmann
3. Third: $\QuotientSpace{(\QuotientSpace{V}{U})}{(\QuotientSpace{W}{U})} \cong \QuotientSpace{V}{W}$
4. Fourth: Lattice correspondence subspaces containing $W \leftrightarrow$ subspaces of $\QuotientSpace{V}{W}$

**Critical Counterexample**: Cardinal subtraction fails for hyperplane check in infinite dims

---

# CHAPTER 5: Products, Hom Spaces, and Dual Spaces

**IMPLEMENTATION NOTES (CRITICAL)**:
1. **Categorical Approach**: Start with universal property definitions for products/coproducts, THEN show constructive definitions and prove equivalence
2. **Product Uniqueness**: State as categorical fact (remark), do NOT prove—just note it follows from universal property
3. **Dimension Calculations**: Add explicit dimension calculations for products/coproducts, and for $\Hom[K]{U}{V}$ via categorical isomorphisms
4. **Hom-Product Proofs (ELABORATE)**: Each isomorphism proof must show:
   - Definition of the isomorphism map
   - Linearity (verify $\Phi(aS + bT) = a\Phi(S) + b\Phi(T)$)
   - Injectivity (show $\Phi(T) = 0 \Rightarrow T = 0$)
   - Surjectivity (construct preimage using universal property)
5. **Hom as Vector Space (ALL AXIOMS)**: Verify all 10 vector space axioms explicitly
6. **K-algebra formatting**: Keep compatibility condition with the definition, not separate
7. **Non-Commutativity Example**: For $\Dim[K]{V} \geq 2$, give explicit operators $S, T$ on basis with $ST \neq TS$. Do NOT use matrices, do NOT assume finite dimensions.
8. **Hyperplanes**: Move hyperplane content from Quotient Spaces to Dual Spaces after Annihilators
9. **Dual Map Surjectivity**: Proof of `T* surjective iff T injective` must work for ALL dimensions (not just finite)
10. **LaTeX Bracket Protection**: When theorem titles contain `[K]` notation, wrap entire title in braces: `\begin{theorem}[{$\End[K]{V}$ is a Ring}]`
11. **Labels on Separate Lines**: Put `\label{...}` on separate line from `\begin{theorem}[...]` to ensure proper line break

## Phase 13: External Direct Sum and Product

1. **Categorical Product** (universal property FIRST):
   - Define via universal property: for any $(f_j: W \to V_j)$, unique $f: W \to P$ with $\pi_j \circ f = f_j$
   - 📊 **DIAGRAM**: Product cone with projections
   - **Uniqueness as remark**: state that uniqueness up to isomorphism is a categorical fact (do NOT prove)
   - **Construction via functions**: $\prod V_i = \Set{f: I \to \bigcup_i V_i \mid f(i) \in V_i}$ with pointwise operations
   - **Proof outline** (VERBOSE - do NOT say "pointwise" without expanding):
     - Vector space verification: contains zero ($0(i) = 0_{V_i}$), closure under + (verify $f(i)+g(i) \in V_i$), closure under scalar (verify $af(i) \in V_i$), additive inverse ($(-f)(i) = -f(i)$)
     - Projections linear: expand $\pi_j(af+bg) = (af+bg)(j) = af(j) + bg(j) = a\pi_j(f) + b\pi_j(g)$ via align
     - Projections surjective: for $v \in V_j$, construct $f$ with $f(j)=v$, $f(i)=0$ otherwise (with cases)
     - Universal property: define $\Phi(w)(j) = f_j(w)$, verify well-defined ($f_j(w) \in V_j$), linearity (expand via $f_j$ linear), factorization, uniqueness (if $\pi_j \circ \Psi = f_j$ then $\Psi(w)(j) = \Phi(w)(j)$)
2. **Categorical Coproduct** (universal property FIRST):
   - Define via universal property: for any $(g_j: V_j \to W)$, unique $g: C \to W$ with $g \circ \iota_j = g_j$
   - 📊 **DIAGRAM**: Coproduct cocone with injections
   - **Uniqueness as remark**: same categorical fact for coproducts
   - **Construction via finite-support functions**: $\bigoplus V_i = \Set{f: I \to \bigcup_i V_i \mid f(i) \in V_i \text{ and } f^{-1}(\bigcup(V_i \setminus \{0\})) \text{ finite}}$
   - **Proof outline** (VERBOSE - do NOT say "immediate"):
     - Subspace verification: contains zero (empty support), closure under + (support ⊆ F∪G), closure under scalar (support preserved)
     - Injections linear: expand $\iota_j(av + bw)(i)$ with cases, match $(a\iota_j(v) + b\iota_j(w))(i)$
     - Injections injective: $\iota_j(v) = 0 \Rightarrow v = \iota_j(v)(j) = 0(j) = 0$, so $\Ker{\iota_j} = \{0\}$
     - Universal property: $g(f) = \sum_{j \in F} g_j(f(j))$, well-defined (finite), linearity (expand sum), factorization, uniqueness
3. **Dimensions**:
   - $\Dim[K]{\ExtDirectSum{i \in I}{V_i}} = \sum_{i \in I} \Dim[K]{V_i}$
   - Product = coproduct dimension for finite $I$
   - For infinite $I$: product dimension can exceed sum
3c. **Dimension of Infinite Products**:
   - **Statement**: $\Dim{\prod V_i} = |\prod V_i| = \prod |V_i|$ for non-trivial $V_i$, infinite $I$
   - **Proof outline**:
     - Step 1: $|V_i| \geq |K| \geq 2$ (field has ≥2 elements), so $|P| \geq 2^{|I|}$ is infinite
     - $|P| = \max(|K|, d)$ gives $d \leq |P|$
     - Step 2 (Case A): If $|P| > |K|$, then $d = |P|$
     - Step 3 (Case B: $|P| = |K|$): Construct injection $\nu: \mathbb{N} \hookrightarrow I$
       - Define $u_\alpha(x) = \alpha^n$ if $x = \nu(n)$, else $0$
       - Evaluate at $\nu(0), \nu(1), \ldots$ → Vandermonde system
     - Step 4: Conclude $d = |P| = \prod |V_i|$
   - **Corollary 1**: $\Dim{V^I} = |V|^{|I|}$ for non-trivial $V$
   - **Proposition (Trivial Factors Don't Contribute)**: Let $J = \{i \in I \mid V_i \neq \{0\}\}$:
     - $\prod V_i \cong \prod_{j \in J} V_j$ via restriction $\phi(f) = f|_J$
     - $\bigoplus V_i \cong \bigoplus_{j \in J} V_j$ via restriction $\psi(f) = f|_J$
     - Proof: restriction is linear, injective (trivial factors force 0), surjective (extend by 0)
   - **Corollary 2** (Dimensions): Let $J = \{i \mid V_i \neq \{0\}\}$:
     - $J = \emptyset$: $\prod V_i = \bigoplus V_i = \{0\}$
     - $J$ finite: dim = $\sum_{j \in J} \dim V_j$ (for both)
     - $J$ infinite: $\dim \prod = \prod_{j \in J} |V_j|$, $\dim \bigoplus = \sum_{j \in J} \dim V_j$
3b. **Example: Constant Family $V_i = V$**:
   - **Statement**: $\prod_{i \in I} V = V^I$ (all functions) and $\bigoplus_{i \in I} V = V^{(I)}$ (finite support)
   - **Proof outline**:
     - Product: Since all $V_i = V$, the union $\bigcup V_i = V$, so $\prod V = \{f: I \to V\} = V^I$
     - Coproduct: By construction, finite support functions in $V^I$, which is $V^{(I)}$
   - **Dimension remark**: $\Dim{V^{(I)}} = d \cdot |I|$; for $V = K$: $K^{(I)}$ has basis $\{\delta_i\}$
4. **Finite case: Biproduct** (CATEGORICAL PROOF with DIAGRAM):
   - 📊 **DIAGRAM**: Show $V_j \xrightarrow{\iota_j} \bigoplus V_i \xrightleftharpoons[\psi]{\phi} \prod V_i \xrightarrow{\pi_k} V_k$ with $\pi_k \circ \phi \circ \iota_j = \delta_{jk}$
   - **Proof outline** (VERBOSE - justify each step):
     - Step 1: Define additive category (zero object, abelian hom-sets, distributive composition)
     - Step 2: Zero morphisms $0_{VW}: V \to 0 \to W$
     - Step 3: Define $\phi$ via universal properties:
       - 3a: By coproduct UP, specify $\phi \circ \iota_j: V_j \to \prod V_i$
       - 3b: By product UP, specify $\pi_i \circ \phi \circ \iota_j: V_j \to V_i$
       - 3c: Define $\pi_i \circ \phi \circ \iota_j = \delta_{ij}$ (Kronecker delta)
     - Step 4: Construct $\psi = \sum_k \iota_k \circ \pi_k$ (finite sum, requires $n < \infty$)
     - Step 5: Verify $\psi \circ \phi = \text{Id}$ using coproduct UP (check on each $\iota_j$)
       - Expand with distributivity, use $\iota_k \circ 0 = 0$, split sum
     - Step 6: Verify $\phi \circ \psi = \text{Id}$ using product UP (check on each $\pi_i$)
       - Expand with distributivity, use $0 \circ f = 0$, split sum
     - Conclusion: Finiteness required because $\psi$ is finite sum; for infinite $I$, image of $\psi$ has finite support
5. **Internal vs External** (VERBOSE PROOF):
   - **Theorem**: $\sum U_i$ is direct sum ⟺ $\phi: \bigoplus U_i \to \sum U_i$ is isomorphism
   - **Proof outline**:
     - Construction: Define $\phi$ via coproduct UP using inclusions $\iota_j^V: U_j \hookrightarrow V$
     - Explicit formula: $\phi(f) = \sum_{j \in F} f(j)$ for $f$ with support $F$
     - Well-defined: finite sum in $V$
     - Linearity: expand via coproduct or directly via align
     - Image: $\Rng{\phi} = \sum U_i$ (both inclusions)
     - (1)⇒(2): Direct sum ⇒ unique representation ⇒ $\Ker{\phi} = \{0\}$ ⇒ injective
     - (2)⇒(1): $\phi$ injective ⇒ if $\sum u_j = 0$ then $f = 0$ ⇒ all $u_j = 0$
6. **Hom-Product Isomorphisms** (VERBOSE PROOFS):
   - **Contravariance** $\Hom[K]{\bigoplus U_i}{V} \cong \prod \Hom[K]{U_i}{V}$:
     - Define $\Phi(T) = (T \circ \iota_i)_i$
     - Well-defined: $T \circ \iota_i: U_i \to V$ is linear
     - Linearity: $\Phi(aS+bT) = ((aS+bT)\circ\iota_i)_i$, expand with align
     - Injectivity: If $T \circ \iota_i = 0$ for all $i$, then $T(f) = T(\sum \iota_j(f(j))) = \sum (T\circ\iota_j)(f(j)) = 0$
     - Surjectivity: By coproduct UP, given $(f_i)$, unique $T$ with $T \circ \iota_i = f_i$
   - **Covariance** $\Hom[K]{U}{\prod V_i} \cong \prod \Hom[K]{U}{V_i}$:
     - Define $\Psi(T) = (\pi_i \circ T)_i$
     - Linearity: expand $\Psi(aS+bT)$ with align
     - Injectivity: If $\pi_i \circ T = 0$ for all $i$, then $T(u)(i) = 0$ for all $i$, so $T(u) = 0$
     - Surjectivity: By product UP, given $(g_i)$, unique $T$ with $\pi_i \circ T = g_i$; verify T is linear
   - **Finite-dim covariance** $\Hom[K]{U}{\bigoplus V_i} \cong \bigoplus \Hom[K]{U}{V_i}$ (requires $\dim U < \infty$):
     - Define $\Theta(T) = (\pi_i \circ T)_i$
     - Key: $\Theta(T)$ has finite support because $F = \bigcup_{k=1}^n F_k$ is finite union
     - Linearity: expand with align
     - Injectivity: If $\pi_i \circ T = 0$ for all $i$, then $T(u)(i) = 0$ for all $i$
     - Surjectivity: Define $T(u)(i) = f_i(u)$, verify $T(u) \in \bigoplus$ (support $\subseteq F$), verify T linear
     - Note: requires $\dim U < \infty$ because basis produces finite union of supports
7. **Free Vector Space Characterizations**: V has basis B iff universal property iff $V \cong K^{(B)}$

---

## Phase 14: Dimension of Hom Spaces

> [!NOTE]
> Basic Hom vector space structure and Endomorphism Algebra moved to Phase 9b (Chapter 2).

1. **Proposition: $\Hom{K}{V} \cong V$** (with PROOF - BEFORE dimension theorem):
   - **Isomorphism**: $\Phi(T) = T(1_K)$, inverse $\Phi^{-1}(v) = (a \mapsto av)$
   - **Proof**: verify well-defined, linear, inverse property
   - **Asymmetry remark**: $\Hom{V}{K}$ is NOT isomorphic to $V$ in general
1b. **Corollary: $\Hom{K}{K} \cong K$** (apply proposition with $V = K$)
2. **Isomorphism theorem**: $\Hom{U}{V} \cong \DirectProd{B_U}{K^{(\Dim[K]{V})}}$ (finite $\Dim[K]{V}$)
   - **Full chain**:
     - $\Hom{U}{V} \cong \Hom{K^{(B_U)}}{V}$ ($U \cong K^{(B_U)}$)
     - $\cong \DirectProd{B_U}{\Hom{K}{V}}$ (contravariance: coproduct → product)
     - $\cong \DirectProd{B_U}{\Hom{K}{K^{(B_V)}}}$ ($V \cong K^{(B_V)}$)
     - $\cong \DirectProd{B_U}{\ExtDirectSum{B_V}{\Hom{K}{K}}}$ (covariance, $\Dim{V}$ finite)
     - $\cong \DirectProd{B_U}{\ExtDirectSum{B_V}{K}}$ (Corollary: $\Hom{K}{K} \cong K$)
     - $\cong \DirectProd{B_U}{K^{(\Dim[K]{V})}}$
   - **Dimension**: $\Card{K^{(\Dim[K]{V})}}^{\Dim[K]{U}}$
3. **Finite case corollary**:
   - By main theorem: $\Hom{U}{V} \cong \DirectProd{i=1}{m}{K^{(n)}}$
   - Finite products/coproducts coincide (biproduct): $\DirectProd{i=1}{m}{K^{(n)}} \cong K^{m \times n}$
   - **Result**: $\Dim[K]{\Hom{U}{V}} = m \cdot n$
4. **Dimension of End**: $\Dim[K]{\End{V}} = n^2$ (apply corollary with $U = V$)


---

## Phase 15: Dual Spaces

1. **Dual space**: $\Dual{V} = \Hom[K]{V}{K}$ (linear functionals)
2. **Dimension** (verbose proof referencing Hom theorems):
   - By Theorem 8.7.3 with codomain $K$ ($\Dim{K} = 1$): $\Dual{V} = \Hom{V}{K} \cong \DirectProd{B}{K^{(1)}} = K^B$
   - Finite: apply finite-dim corollary → $\Dim{V^*} = n \cdot 1 = n$
   - Infinite: product over infinite index → $\Dim{V^*} = |K^B| = |K|^\kappa > \kappa$ (Cantor)
3. **Dual basis** (SEPARATE THEOREMS with proofs):
   - **Linear independence**: evaluate $\sum a_i b^i = 0$ at $b_j$ gives $a_j = 0$
   - **Basis iff finite-dim**: uses dimension theorem both directions
   - **Example (functional NOT in span)**: On $K[X]$, the dual basis is $\{\delta_n\}$ (coefficient extraction). But $\mathrm{ev}_1(p) = p(1)$ is NOT in $\Span{\{\delta_n\}}$ since it requires infinitely many coefficients. Proof: if $\mathrm{ev}_1 = \sum_{i=0}^N c_i \delta_i$, then $\mathrm{ev}_1(X^{N+1}) = 1 \neq 0$.
   - **Expansion formula** (finite-dim only): $\varphi = \sum \varphi(b_i) b^i$, verify on basis
4. **Non-canonical isomorphism** (RIGOROUS PROOF):
   - $\psi_\mathcal{B}: V \to V^*$, $b_i \mapsto b^i$
   - Well-defined: linear extension theorem
   - Injective: dual basis linear independence
   - Surjective: equal finite dimensions
   - Basis-dependence: different $\mathcal{B}' \Rightarrow$ different $\psi$
5. **Annihilators** (SEPARATE PROPOSITIONS, verbose proofs):
   - **Subspace**: non-empty, closed under +, closed under scalar ×
   - **Antitonicity**: $S \subseteq T \Rightarrow \Ann{T} \subseteq \Ann{S}$
   - **Span invariance**: $\Ann{S} = \Ann{\Span{S}}$ (both inclusions with detailed linearity argument)
   - **Ann({0}) = V***: every functional vanishes on 0
   - **Ann(V) = {0}**: only zero functional vanishes everywhere
   - **Dimension**: $\dim \Ann{U} + \dim U = \dim V$, explicit basis construction

6. **Hyperplanes** (Definition):
   - $H \Subspace[K] V$ is a **hyperplane** iff $\Dim[K]{\QuotientSpace{V}{H}} = 1$
   - **Equivalent characterizations (all dimensions)**:
     - (i) $\Dim[K]{\QuotientSpace{V}{H}} = 1$
     - (ii) $\Exists :{\text{1-dim } L}{H \DirectSum L = V}$
     - (iii) $H = \Ker{f}$ for some non-zero $f \in \Dual{V}$
     - (iv) $\Dim[K]{\Ann{H}} = 1$
   - **Equivalent (FINITE-DIM ONLY)**: $\Dim[K]{H} = \Dim[K]{V} - 1$
   - **Proof outline**: 
     - (i)⟺(ii) via complement of hyperplane
     - (i)⟺(iii) via projection and 1st isomorphism theorem
     - (iii)⟺(iv): If $H = \Ker{f}$, any $g \in \Ann{H}$ satisfies $g = g(v_0)f$ for some $v_0$ with $f(v_0) = 1$
7. **Hyperplane Examples**:
   - **Finite-dimensional**: In $K^n$, the hyperplane $\Set{(x_1, \ldots, x_n) \mid x_1 + x_2 + \cdots + x_n = 0} = \Ker{f}$ where $f(x) = \sum x_i$
   - **Infinite-dimensional**: In $K[X]$ (polynomials), $\Set{p \in K[X] \mid p(0) = 0}$ is a hyperplane (kernel of evaluation at 0)
   - **Infinite-dimensional**: In $K^{(\mathbb{N})}$, the set $\Set{(a_n) \mid \sum_{n \in \mathbb{N}} a_n = 0}$ (finite sums!) is a hyperplane
   - 🎨 **FIGURE**: Hyperplane (plane through origin) in $\mathbb{R}^3$
8. **CRITICAL Counterexample (Cardinal vs Dimension)**:
   - Let $V = K[X]$ with $\Dim[K]{V} = \aleph_0$ (basis: $1, X, X^2, \ldots$)
   - Let $W = \Span[K]{\Set{X^n \mid n \geq 2}}$ with $\Dim[K]{W} = \aleph_0$
   - Cardinal arithmetic: $\aleph_0 + 1 = \aleph_0 = \Dim[K]{V}$
   - **But $W$ is NOT a hyperplane**: $\QuotientSpace{V}{W} \cong \Span[K]{\Set{1, X}}$ has $\Dim[K]{\QuotientSpace{V}{W}} = 2 \neq 1$
   - **Lesson**: In infinite dimensions, $\Dim[K]{W} + 1 = \Dim[K]{V}$ does NOT imply $W$ is hyperplane
   - **Correct criterion**: Always use $\Dim[K]{\QuotientSpace{V}{W}} = 1$, not cardinal subtraction
9. **Every subspace is intersection of hyperplanes** (all dimensions):
   - **Statement**: For any $U \Subspace[K] V$, $U = \BigIntersect[f \in \Ann{U}] \Ker{f}$
   - Convention: empty intersection = whole space $V$
   - For infinite-dim, the intersection may be over infinitely many hyperplanes
   - **Key for implicit equations** (finite case): $U = \Ker{f_1} \Intersect \cdots \Intersect \Ker{f_r}$ where $\{f_1, \ldots, f_r\}$ is a basis of $\Ann{U}$

10. **Dual map**: $T^*: V^* \to U^*$, contravariant
11. **Dual map properties**: inj ⟺ surj, surj ⟺ inj (all dimensions), bij ⟺ bij (all dimensions)
12. **Bidual**: $\eta_V: V \hookrightarrow V^{**}$ canonical, iso iff finite-dim
13. **Matrix**: $[T^*] = [T]^\top$

---

### CHAPTER 5 COMPENDIUM

**Products and Coproducts**:
- Product: $\DirectProd{i \in I}{V_i}$ with projections $\pi_j$
- Coproduct: $\ExtDirectSum{i \in I}{V_i}$ = finite support, with injections $\iota_j$
- Finite $I$: product = coproduct (biproduct)
- Dimensions: $\Dim{\ExtDirectSum{}{V_i}} = \sum \Dim{V_i}$

**Hom-Product Isomorphisms**:
- $\Hom[K]{\ExtDirectSum{}{U_i}}{V} \cong \DirectProd{}{\Hom[K]{U_i}{V}}$ (contravariant)
- $\Hom[K]{U}{\DirectProd{}{V_i}} \cong \DirectProd{}{\Hom[K]{U}{V_i}}$ (covariant)
- $\Hom[K]{U}{\ExtDirectSum{}{V_j}} \cong \ExtDirectSum{}{\Hom[K]{U}{V_j}}$ (finite-dim $U$)

**Hom Spaces and K-Algebras**:
- $\Hom[K]{U}{V}$ is a $K$-vector space; $\End[K]{V}$ is unital associative $K$-algebra
- Non-commutative when $\Dim{V} \geq 2$
- $\Hom{K}{V} \cong V$; $\Hom{K}{K} \cong K$

**Hom Dimension (Finite $\Dim{V}$)**:
$$\Hom{U}{V} \cong \DirectProd{B_U}{\Hom{K}{K^{(B_V)}}} \cong \DirectProd{B_U}{\ExtDirectSum{B_V}{K}} \cong \DirectProd{B_U}{K^{(\Dim{V})}}$$
- Dimension: $\Card{K^{(\Dim{V})}}^{\Dim{U}}$; finite case: $\Dim{\Hom{U}{V}} = m \cdot n$

**Dual Space**:
- $\Dual{V} = \Hom[K]{V}{K} \cong \DirectProd{B}{K^{(1)}} = K^B$
- $\Dim{\Dual{V}} = \Dim{V}$ (finite); $\Dim{\Dual{V}} = |K|^\kappa > \kappa$ (infinite)
- Dual basis: $b^i(b_j) = \delta_{ij}$; spans $\Dual{V}$ iff finite-dim
- **Not in span example**: $\mathrm{ev}_1 \notin \Span{\{\delta_n\}}$ on $K[X]$
- $V \cong \Dual{V}$ non-canonical (basis-dependent)

**Annihilators**:
- $\Ann{S} = \Set{\varphi \in \Dual{V} \mid \varphi|_S = 0}$
- $\Ann{S} = \Ann{\Span{S}}$; $\Ann{\{0\}} = \Dual{V}$; $\Ann{V} = \{0\}$
- $\Dim{\Ann{U}} + \Dim{U} = \Dim{V}$ (finite-dim)
- $\Ann{U} \cong \Dual{\QuotientSpace{V}{U}}$ (finite-dim)

**Hyperplanes** (equivalent characterizations):
- (i) $\Dim{\QuotientSpace{V}{H}} = 1$
- (ii) $\Exists{\text{1-dim } L}{H \DirectSum L = V}$
- (iii) $H = \Ker{f}$ for nonzero $f \in \Dual{V}$
- (iv) $\Dim{\Ann{H}} = 1$
- Finite-dim only: $\Dim{H} = \Dim{V} - 1$

**Dual Map and Bidual**:
- $\DualMap{T}: \Dual{V} \to \Dual{U}$, $\DualMap{T}(\varphi) = \varphi \circ T$ (contravariant)
- $\DualMap{T}$ inj $\Leftrightarrow$ $T$ surj; $\DualMap{T}$ surj $\Leftrightarrow$ $T$ inj
- $\eta_V: V \hookrightarrow \Dual{\Dual{V}}$ canonical; iso iff $\Dim{V} \in \bb{N}$

# CHAPTER 6: Endomorphisms & Spectral Theory

## Phase 22: Endomorphisms - Basics

1. **Definition**: $T \in \End[K]{V}$ (linear map $V \to V$)
2. **Ring structure**: $(\End[K]{V}, +, \Composition{}{})$ is a ring (non-commutative if $\Dim[K]{V} > 1$)
3. **$K$-algebra structure**: $(\End[K]{V}, +, \Composition{}{}, \cdot)$ is a $K$-algebra (non-commutative if $\Dim[K]{V} > 1$)
4. **Projections**:
   - Define projection onto a subspace along another subspace (they have to be in direct sum for well definedness)
   - $P^2 = P$ (idempotent)
   - $V = \Ker{P} \DirectSum \DirectImage{P}{V}$
   - 🎨 **FIGURE**: Projection as "shadow" onto a plane in $\mathbb{R}^3$
5. **Symmetries**:
   - Define symmetry relative to a subspace along another subspace (they have to be in direct sum for well definedness)
   - $S^2 = \Identity{V}$ (involution)
   - $S = 2P - \Identity{V}$ for some projection $P$
   - 🎨 **FIGURE**: Reflection across a plane in $\mathbb{R}^3$
6. **Nilpotent**: $T^n = 0$ for some $n$

---

## Phase 23: Invariant Subspaces, Diagonal, and Triangular Operators

### 22a: T-Invariant Subspaces
1. **$T$-invariant subspace**: $U \Subspace[K] V$ with $\DirectImage{T}{U} \subseteq U$
2. **Restriction**: $\Restriction{T}[U] \in \End[K]{U}$
3. **Invariant iff upper triangular block**: $U$ is $T$-invariant iff there exists ordered basis such that:
   $$\MatrixRep{T} = \begin{pmatrix} A & B \\ 0 & D \end{pmatrix}$$ 
   where first block corresponds to $U$
4. **Invariant iff lower triangular block**: Equivalently with lower triangular block form
5. **Invariant + direct sum complement iff block diagonal**: 
   $V = U \DirectSum W$ with both $U, W$ are $T$-invariant iff:
   $$\MatrixRep{T} = \begin{pmatrix} A & 0 \\ 0 & D \end{pmatrix}$$
6. **Similarity**: $T \sim S$ iff $\Exists :{P \\in \\Aut[K]{V}}{P^{-1}TP = S}$ (same operator up to change of basis)

### 22b: Diagonal Operators
1. **Diagonal matrix**: $A_{ij} = 0$ for $i \neq j$
2. **Diagonal operator relative to ordered basis**: $T$ is diagonal relative to ordered basis $B$ iff $\Forall :{v \in B}{T(v) = \lambda_v v}$ iff matrix representation is diagonal
3. **Diagonalizable operator**:
   - $V$ can be written as a direct sum of $T$-invariant subspaces of dimension $1$
   - Equivalently, there EXISTS an ordered basis such that $T$ is diagonal relative to it
4. **Diagonalizable matrix**: $A = PDP^{-1}$ for some invertible $P$ and diagonal $D$

### 22c: Triangular Operators
1. **Upper triangular matrix**: $A_{ij} = 0$ for $i > j$
2. **Lower triangular matrix**: $A_{ij} = 0$ for $i < j$
3. **Upper/lower triangular relative to ordered basis**: Can choose either convention since reversing basis order swaps upper↔lower (prove this) (i.e. $T$ is upper triangular relative to $(b_0, \ldots, b_{n-1})$ iff lower triangular relative to $(b_{n-1}, \ldots, b_0)$)
4. **Triangularizable operator**:
   - Triangularizability Characterization: $T$ triangularizable iff there exists chain of $T$-invariant subspaces:
   $$\{0\} = U_0 \subsetneq U_1 \subsetneq \cdots \subsetneq U_n = V$$
   with $\Dim[K]{U_{i+1}} = \Dim[K]{U_i} + 1$
   - Equivalently, there EXISTS an ordered basis such that $T$ is triangular relative to it

5. **Triangularizable matrix**: $A = PUP^{-1}$ for some invertible $P$ and upper triangular $U$

---

## Phase 24: Eigentheory

1. **Eigenvalue**: $\lambda \in K$ with $\Ker{T - \lambda \Identity{V}} \neq \{0\}$
2. **Eigenspace**: $E_\lambda(T) = \Ker{T - \lambda \Identity{V}}$
   - 🎨 **FIGURE**: Eigenspaces as invariant directions under scaling in $\mathbb{R}^2$
3. **Eigenvectors for distinct eigenvalues are linearly independent**
4. **Generalized eigenspace**: $G_\lambda(T) = \BigUnion[k \geq 1] \Ker{(T - \lambda I)^k}$
5. **Algebraic vs geometric multiplicity**
6. **Diagonalizable iff $V = \DirectSum{\lambda}{E_\lambda(T)}$** (sum over eigenspaces)

---

## Phase 25: Jordan Normal Form and Commuting Operators

### Jordan Form
1. **Jordan block** $J_n(\lambda)$
2. **Jordan normal form**: Exists over algebraically closed fields

### Minimal and Characteristic Polynomials
3. **Characteristic polynomial**: $\CharPoly{T}(\lambda) = \Det{T - \lambda I}$
4. **Minimal polynomial**: $\MinPoly{T}$ = monic polynomial of smallest degree with $\MinPoly{T}(T) = 0$
5. **Cayley-Hamilton theorem**: $\CharPoly{T}(T) = 0$
6. **Same linear factors**: $\MinPoly{T}$ and $\CharPoly{T}$ have the same irreducible factors (over $K$)
7. **Diagonalizability criterion**: $T$ diagonalizable iff $\MinPoly{T}$ has distinct linear factors
   - i.e., $\MinPoly{T} = \prod_{i}(\lambda - \lambda_i)$ with $\lambda_i$ distinct

### Commuting Operators
8. **Commuting operators share a common eigenvector**
9. **Commuting diagonalizable operators are simultaneously diagonalizable**

---

> **CHAPTER 6 COMPENDIUM**: Add compendium here summarizing eigenvalues, characteristic polynomials, diagonalization.

---

# CHAPTER 7: Multilinear Algebra & Determinants

## Phase 26: Multilinear Algebra

1. **Multilinear map**: $f: U_1 \times \cdots \times U_n \to V$ linear in each argument
2. **Vector space structure**: $\MultiLin[K]{U_1, \ldots, U_n}{V}$ is $K$-vector space
3. **Dimension**: $\Dim[K]{\MultiLin[K]{U_1, \ldots, U_n}{V}} = \prod_{i=1}^{n} \Dim[K]{U_i} \cdot \Dim[K]{V}$
4. **Forms**: Multilinear map to $K$ (codomain = field)
5. **For $U_1 = \cdots = U_n = V$**:
   - **Symmetric**: $f(v_{\sigma(1)}, \ldots, v_{\sigma(n)}) = f(v_1, \ldots, v_n)$ for all $\sigma \in S_n$
   - **Antisymmetric**: Swapping two arguments negates value
   - **Alternating**: $f(\ldots, v, \ldots, v, \ldots) = 0$ when two args equal
6. **Relationships**:
   - Alternating $\Rightarrow$ antisymmetric
   - $\operatorname{char}(K) = 2$: antisymmetric $\iff$ symmetric
   - $\operatorname{char}(K) \neq 2$: antisymmetric $\iff$ alternating
7. **Dimensions**: $\Dim{\SymForm{n}{V}} = \binom{\Dim{V}+n-1}{n}$, $\Dim{\AltForm{n}{V}} = \binom{\Dim{V}}{n}$

---

## Phase 27: Determinant

1. **Determinant as alternating multilinear form** on columns of $K^n$
2. **Uniqueness**: Unique alternating $n$-linear form with $\Det{I} = 1$
3. **Determinant as functional on $\End[K]{V}$**
4. **Properties**: $\Det{AB} = \Det{A}\Det{B}$, $\Det{\Transpose{A}} = \Det{A}$, $A$ invertible iff $\Det{A} \neq 0$
5. **Prove equivalence of determinants (the alternating multilinear form and the functional on $\End[K]{V}$)**

---

> **CHAPTER 7 COMPENDIUM**: Add compendium here summarizing multilinear maps, determinants, exterior algebra.

---

# CHAPTER 8: Normed & Inner Product Spaces

## Phase 28: Normed Vector Spaces

1. **Norm**: $\Norm{\cdot}: V \to [0, \infty)$ satisfying positive definiteness, homogeneity, triangle inequality
2. **Normed set** (unit sphere): $\Set{v \mid \Norm{v} = 1}$
3. **Isometry** (norm-preserving): $\Norm{T(u)} = \Norm{u}$
   - Isometries are injective

---

## Phase 29: Inner Product Spaces

1. **Inner product**: $\InnerProd{\cdot}{\cdot}: V \times V \to K$ with conjugate symmetry, linearity, positive definiteness
2. **Induced norm**: $\Norm{v} = \sqrt{\InnerProd{v}{v}}$
3. **Orthogonal set**: $\InnerProd{u}{v} = 0$ for all pairs
   - Orthogonal set not containing $0$ is linearly independent (prove using expansion)
4. **Orthonormal set/basis**
5. **Gram-Schmidt process**

---

## Phase 30: Orthogonal Projections and Symmetries

1. **Orthogonal complement**: $\OrthComp{U}$
2. **Orthogonal projection**: $\Proj{U}$ with $V = U \DirectSum \OrthComp{U}$
3. **Orthogonal symmetry**: $\Sym{U} = 2\Proj{U} - \Identity{V}$

---

## Phase 31: Adjoint Operators

1. **Inner product preserving maps**: Equivalent to norm-preserving
### Riesz-Fréchet Theorem (CRITICAL - prove fully)
2. **For each $u \in V$**: Define $f_u: V \to K$ by $f_u(v) = \InnerProd{u}{v}$
   - $f_u$ is a linear functional (prove linearity in second argument)
3. **The map $\Phi: V \to \Dual{V}$** defined by $\Phi(u) = f_u$ is:
   - **Conjugate-linear** (or linear for real case)
   - **Injective**: $f_u = 0 \implies \InnerProd{u}{u} = 0 \implies u = 0$ (positive definiteness)
   - **Surjective for finite-dim**: Every $\varphi \in \Dual{V}$ has the form $\varphi = f_u$ for unique $u$
     - **Proof**: If $\varphi = 0$, take $u = 0$. Otherwise, $\Ker{\varphi}$ is hyperplane, so $V = \Ker{\varphi} \DirectSum \Span[K]{\{w\}}$ for some $w \notin \Ker{\varphi}$. Set $u = \frac{\overline{\varphi(w)}}{\Norm{w}^2} w$. Verify $f_u = \varphi$ by checking on basis.
4. **Isometric**: $\Norm{f_u}_{\Dual{V}} = \Norm{u}_V$ (Cauchy-Schwarz)

### Adjoint Operators
5. **Adjoint existence**: Using Riesz-Fréchet, for each $v$, $u \mapsto \InnerProd{T(u)}{v}$ is a functional, so there exists unique $\Adjoint{T}(v)$ with $\InnerProd{T(u)}{v} = \InnerProd{u}{\Adjoint{T}(v)}$
6. **$\Adjoint{T}$ is linear** (prove from uniqueness)
7. **Matrix**: $[\Adjoint{T}]$ relative to orthonormal basis = $\Hermitian{[T]}$
---

## Phase 32: Spectral Theorem

1. **Normal**: $T\Adjoint{T} = \Adjoint{T}T$
2. **Self-adjoint (Hermitian)**: $\Adjoint{T} = T$
3. **Unitary/Orthogonal**: $\Adjoint{T} = T^{-1}$
4. **Positive (semi-)definite**
5. **Spectral theorem**: Normal operators on finite-dim complex IP spaces are unitarily diagonalizable

---

> **CHAPTER 8 COMPENDIUM**: Add compendium here summarizing norms, inner products, orthogonality.

---

## Document Organization

```
Document/LinearAlgebra/
├── LinearAlgebra.tex                    # \part{Linear Algebra}
│
└── VectorSpaces/
    ├── VectorSpaces.tex                 # Part file
    │
    ├── Foundations/                     # CHAPTER 1: Foundations
    │   ├── Definition.tex               # Phase 1: Vector spaces
    │   ├── Subspaces.tex                # Phase 2: Subspaces
    │   ├── Span.tex                     # Phase 3: Span
    │   ├── LinearIndependence.tex       # Phase 4: Linear independence
    │   ├── Bases.tex                    # Phase 5: Bases and dimension
    │   ├── OrderedBases.tex             # Phase 6: Ordered bases
    │   ├── MinkowskiSum.tex             # Phase 7: Minkowski structure
    │   └── DirectSum.tex                # Phase 8: Internal direct sums
    │
    ├── LinearMaps/                      # CHAPTER 2: Linear Maps
    │   ├── Definition.tex               # Phase 9: Linear transformations
    │   ├── InjSurjBij.tex               # Phase 10: Inj/Surj/Bij & Rank-Nullity
    │   └── Properties.tex
    │
    ├── Matrices/                        # CHAPTER 3: Matrices & Representations
    │   ├── Matrices.tex                 # Phase 16: Matrix fundamentals
    │   ├── ChangeOfBasis.tex            # Phase 17: Change of basis, Gaussian
    │   ├── SubspaceRepresentations.tex  # Phase 18: Parametric/implicit forms
    │   └── RankInequalities.tex         # Phase 19-21: Rank inequalities
    │
    ├── QuotientSpaces/                  # CHAPTER 4: Quotients & Iso Theorems
    │   ├── QuotientSpaces.tex           # Phase 11: Quotient spaces & UP
    │   ├── Hyperplanes.tex
    │   └── IsomorphismTheorems.tex      # Phase 12: All four iso theorems
    │
    ├── HomAndDual/                      # CHAPTER 5: Products, Hom, Dual
    │   ├── ExternalOperations.tex       # Phase 13: External products
    │   ├── HomSpaces.tex                # Phase 14: Hom spaces
    │   └── DualSpaces.tex               # Phase 15: Dual spaces, annihilators, V≅V**
    │
    ├── Endomorphisms/                   # CHAPTER 6: Endomorphisms & Spectral
    │   ├── Endomorphisms.tex            # Phase 22: Projections, symmetries
    │   ├── InvariantSubspaces.tex       # Phase 23: Invariant, diagonal, triangular
    │   ├── Eigentheory.tex              # Phase 24: Eigenvalues/spaces
    │   └── JordanForm.tex               # Phase 25: Jordan, min/char poly
    │
    ├── Multilinear/                     # CHAPTER 7: Multilinear & Determinants
    │   ├── Multilinear.tex              # Phase 26: Multilinear maps
    │   └── Determinant.tex              # Phase 27: Determinant
    │
    └── InnerProduct/                    # CHAPTER 8: Normed & Inner Product
        ├── NormedSpaces.tex             # Phase 28: Norms
        ├── InnerProduct.tex             # Phase 29: Inner products
        ├── Orthogonality.tex            # Phase 30: Orthogonal projections
        ├── Adjoint.tex                  # Phase 31: Riesz-Fréchet, adjoint
        └── SpectralTheorem.tex          # Phase 32: Spectral theorem
```

