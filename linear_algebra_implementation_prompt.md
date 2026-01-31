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
  - **Grassmann**: $\Dim[K]{U + W} + \Dim[K]{U \cap W} = \Dim[K]{U} + \Dim[K]{W}$ (NOT dim = dim - dim)
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
- Do NOT leave broken references like `??`—ensure all `\ref{}` labels are defined before referencing
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

---

## Phase 0: Macro Reference

**Macros in `Preamble/Commands/LinearAlgebra.tex`:**

| Category | Macros |
|----------|--------|
| **Vector Spaces** | `\VecSpace[K]{V}`, `\Span[K]{S}`, `\Subspace[K]` |
| **Sums** | `\SubspaceSum{i}{U_i}`, `\DirectSum{i}{V_i}`, `\ExtDirectSum{i}{V_i}`, `\DirectProd{i}{V_i}` |
| **Quotient** | `\QuotientSpace{V}{W}`, `\QuotientSpace p{V}{W}` (parenthesized), `\Coset{v}{W}` |
| **Fundamental** | `\Ker{T}`, `\Rng{T}`, `\Rank{T}`, `\Nullity{T}` |
| **Dimension** | `\Dim[K]{V}` |
| **Matrices** | `\Transpose{A}`, `\Adjoint{A}`, `\TensorProduct[K]{U}{V}`, `\Hadamard{A}{B}`, `\KroneckerProduct[K]{A}{B}` |
| **Spectral** | `\Det{A}`, `\Trace{A}`, `\Adjugate{A}`, `\Spectrum{T}`, `\Eigenspace{T}{\lambda}`, `\GenEigenspace{T}{\lambda}` |
| **Polynomials** | `\MinPoly{T}`, `\CharPoly{T}` |
| **Hom/End** | `\Hom[K]{U}{V}`, `\End[K]{V}`, `\Aut[K]{V}`, `\GL{n}{K}`, `\Dual{V}` |
| **Dual** | `\DualBasis{B}`, `\DualMap{T}` |
| **Multilinear** | `\MultiLin[K]{...}{V}`, `\MultiForm[K]{n}{V}`, `\SymForm{n}{V}`, `\AltForm{n}{V}`, `\Wedge{a}{b}` |
| **Norms/IP** | `\Norm[p]{v}`, `\InnerProd{u}{v}`, `\OrthComp{U}`, `\Proj{U}`, `\Sym{U}` |
| **Misc** | `\LinIndep`, `\LinDep`, `\MinkowskiSum{A}{B}`, `\Hermitian{A}` |

**ALWAYS use `[K]` subscript** for: `\Span[K]`, `\Subspace[K]`, `\Dim[K]`, `\Hom[K]`, `\End[K]`, `\Aut[K]`, `\TensorProduct[K]`, `\KroneckerProduct[K]`

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

---

# CONTENT SUMMARY

---

## Chapter 1: Foundations of Vector Spaces

### Phase 1: Vector Spaces (`Definition.tex`)
| Type | Name | Status |
|------|------|--------|
| Definition | Vector Space
| Example | Standard Examples
| Example | Finite Support Functions $K^{(I)}$
| Example | Field Extensions
| Proposition | Zero Scalar Annihilates
| Proposition | Scalar Times Zero Vector
| Theorem | Zero Product Property
| Proposition | Cancellation Laws

### Phase 2: Vector Subspaces (`Subspaces.tex`)
| Type | Name | Status |
|------|------|--------|
| Definition | Subspace
| Theorem | Subspace Criterion
| Proposition | Intersection of Subspaces
| Proposition | Union of Subspaces

### Phase 3: Span and Generating Systems (`Span.tex`)
| Type | Name | Status |
|------|------|--------|
| Definition | Linear Combination
| Definition | Span
| Theorem | Span Constructive Characterization
| Proposition | Properties of Span (7 items)
| Definition | Generating System

### Phase 4: Linear Independence (`LinearIndependence.tex`)
| Type | Name | Status |
|------|------|--------|
| Definition | Linear Dependence
| Theorem | Characterizations of Linear Independence
| Proposition | Adding to Linearly Independent Set
| Corollary | Singleton Sets

### Phase 5: Bases and Dimension (`Bases.tex`)
| Type | Name | Status |
|------|------|--------|
| Definition | Basis
| Theorem | Maximal/Minimal Characterizations
| Theorem | Basis Extension Theorem (Zorn)
| Corollary | Every Vector Space Has a Basis
| Theorem | Steinitz Exchange Lemma
| Theorem | Dimension Invariance
| Definition | Dimension
| Proposition | Cardinal Properties of Vector Spaces

### Phase 6: Ordered Bases and Coordinates (`OrderedBases.tex`)
| Type | Name | Status |
|------|------|--------|
| Definition | Ordered Basis
| Definition | Coordinate Vector
| Theorem | Coordinates depend on ordering

### Phase 7: Minkowski Structure (`MinkowskiSum.tex`)
| Type | Name | Status |
|------|------|--------|
| Definition | Minkowski Sum
| Proposition | Monoid Structure with Minkowski Sum and absorbing element
| Proposition | Minkowski Scalar-Set multiplication
| Proposition | absorbing elements and associativity of scalars with scalar multiplication, absorbing elements and non distributivity
| Proposition | Minkowski sum of subspaces is a subspace


### Phase 8: Internal Direct Sums (`DirectSum.tex`)
| Type | Name | Status |
|------|------|--------|
| Definition | Internal Direct Sum Family
| Theorem | Direct Sum Characterization
| Corollary | Two-Subspace Criterion
| Theorem | Grassmann Dimension Formula
| Corollary | Direct Sum Dimension

---

## Chapter 2: Linear Maps

### Phase 9: Linear Transformations (`LinearMaps/Definition.tex`)
| Type | Name | Status |
|------|------|--------|
| Definition | Linear Map
| Proposition | Zero Maps to Zero
| Theorem | Universal Property of Vector Spaces
| Definition | Kernel and Image
| Proposition | Fundamental Subspaces Are Subspaces
| Proposition | Image and Preimage Preserve Subspaces

### Phase 10: Injectivity, Surjectivity, Bijectivity (`InjSurjBij.tex`)
| Type | Name | Status |
|------|------|--------|
| Theorem | Characterizations of Injectivity (7 equiv)
| Theorem | Characterizations of Surjectivity (7 equiv)
| Theorem | Characterizations of Bijectivity (6 equiv)
| Theorem | Finite-Dim Endomorphism Equivalence

### Phase 10b: Rank-Nullity
| Type | Name | Status |
|------|------|--------|
| Theorem | Rank-Nullity (via basis extension)
| Corollary | Injective + same dim ⟹ surjective

---

## Chapter 3: Quotient Spaces & Isomorphism Theorems

### Phase 11: Quotient Spaces (`QuotientConstruction.tex`)
| Type | Name | Status |
|------|------|--------|
| Definition | Coset and Quotient Space
| Theorem | Quotient Space is a Vector Space
| Definition | Canonical Projection
| Theorem | Quotient Dimension Formula
| Theorem | Universal Property of Quotient Spaces

### Hyperplanes (`Hyperplanes.tex`)
| Type | Name | Status |
|------|------|--------|
| Definition | Hyperplane
| Proposition | Characterizations of Hyperplanes
| Example | Hyperplanes (Finite and Infinite)
| Example | Counterexample: Cardinal Arithmetic Fails
| Theorem | Subspaces as Intersections of Hyperplanes

### Phase 12: Isomorphism Theorems (`IsomorphismTheorems.tex`)
| Type | Name | Status |
|------|------|--------|
| Theorem | First Isomorphism Theorem
| Theorem | Second Isomorphism Theorem
| Theorem | Third Isomorphism Theorem
| Theorem | Fourth (Lattice) Isomorphism Theorem

---

## Chapter 4: Products, Hom Spaces, and Dual Spaces

### Phase 13: External Direct Sum/Product
- Definition: External Direct Product
- Theorem: Universal Properties

### Phase 14: Hom Spaces
- Theorem: $\Hom[K]{U}{V}$ is a $K$-vector space
- Theorem: Dimension of Hom spaces

### Phase 15: Dual Spaces
- Definition: Dual Space, Dual Basis, Annihilator
- Theorem: Canonical isomorphism $V \cong \Dual{\Dual{V}}$ (finite)
- Definition: Dual Map

---

## Chapter 5: Matrices & Subspace Representations ○

### Phase 16-17: Matrices and Change of Basis
- Definition: Matrix of linear map
- Theorem: $\Hom \cong \MatSpace{n}{m}$
- Definition: Similarity and equivalence

### Phase 18: Subspace Representations
- Theorem: Every subspace = kernel/image of some map
- Theorem: Parametric ↔ Implicit conversions

### Phases 19-20: Rank Inequalities
- Theorem: Sylvester inequality
- Theorem: Frobenius inequality

---

## Chapter 6: Endomorphisms & Spectral Theory

### Phase 22-23: Endomorphisms and Invariant Subspaces
- Definition: Projection, Symmetry, Nilpotent
- Definition: $T$-invariant subspace
- Theorem: Invariant ↔ block triangular

### Phase 24: Eigentheory
- Definition: Eigenvalue, eigenvector, eigenspace
- Theorem: Diagonalizable ↔ $V = \bigoplus E_\lambda$

### Phase 25: Jordan Form
- Theorem: Jordan normal form
- Theorem: Cayley-Hamilton
- Theorem: Diagonalizability criterion

---

## Chapter 7: Multilinear Algebra & Determinants

### Phase 26-27
- Definition: Multilinear map, Symmetric/Alternating forms
- Definition: Determinant
- Theorem: Uniqueness and Multiplicativity

---

## Chapter 8: Normed & Inner Product Spaces

### Phase 28-32
- Definition: Norm, Inner product, Orthogonality
- Theorem: Gram-Schmidt
- Theorem: Riesz-Fréchet
- Definition: Adjoint operator
- Theorem: Spectral theorem

---

# CHAPTER 1: Foundations of Vector Spaces

**File: `Document/LinearAlgebra/VectorSpaces/Foundations/`**

## Phase 1: Vector Spaces

### Categorical Framing
- **Linear Algebra studies the category $K$-Vect**
- **Objects**: $K$-vector spaces (defined in this phase)
- **Morphisms**: $K$-linear transformations (defined in Phase 9)
- This perspective unifies the treatment and motivates constructions

### File: `Document/LinearAlgebra/VectorSpaces/Foundations/Definition.tex`

1. **Intuition Block**: Category K-Vect perspective; vector spaces as the fundamental objects
2. **Definition** with 8 axioms (abelian group + ring action)
3. **Function spaces $K^I$ (FIRST)**:
   - Define pointwise operations
   - Define **indicator functions** $e_i(j) = \delta_{ij}$ (do NOT call them "basis" yet—bases not introduced)
4. **Coordinate spaces $K^n$ as special case**:
   - $K^n = K^{\{1, \ldots, n\}}$—view $n$-tuples as functions
5. **Geometric vectors** in 2D/3D:
   - Identify points with position vectors (arrows from origin)
   - **Addition** via tip-to-toe rule (equivalently parallelogram rule)
   - **Scalar multiplication** as scaling (stretch/shrink, reverse if negative)
   - 🎨 **FIGURE**: TikZ illustration of parallelogram rule and 3D vector
6. **Polynomial spaces $K[X]$** and **Matrix spaces $M_{m \times n}(K)$**
7. **Functions with Finite Support $K^{(I)}$**:
   - Definition as subspace of $K^I$
   - Every $f \in K^{(I)}$ can be written as finite sum $\sum f(i) e_i$
   - Do NOT claim $(e_i)$ is a basis here—defer to Phase 6
   - Key distinction: $K^{(I)} = K^I$ iff $I$ finite
8. **Field Extension Examples**: $\mathbb{C}/\mathbb{R}$, $\mathbb{R}/\mathbb{Q}$
9. **Properties from axioms** with proofs:
   - $0v = 0$, $a0 = 0$, $(-1)v = -v$
   - $av = 0 \implies a = 0 \LogicOr v = 0$
   - **Cancellation laws**: $av = bv$ with $v \neq 0 \implies a = b$; $av = aw$ with $a \neq 0 \implies v = w$
10. **Remark on Notation Abuse**: $0$ is polymorphic (scalar or vector from context)

---

## Phase 2: Minkowski Structure on $\mathcal{P}(V)$

## Phase 2: Vector Subspaces

1. **Definition**
2. **Subspace Criterion**: $U \neq \emptyset$ and $\Forall :{a,b \in K}{\Forall :{u,v \in U}{au + bv \in U}}$
3. **Trivial subspaces**: $\{0\}$ and $V$
   - **Non trivial examples of vector spaces**
   - 🎨 **FIGURE**: Lines and planes through origin in $\mathbb{R}^3$
4. **Intersection**: Arbitrary intersection of subspaces is subspace
5. **Union**: $U \cup W$ is subspace iff $U \subseteq W \LogicOr W \subseteq U$

**NOTE**: Hyperplanes defined AFTER direct sums and linear maps (see Phase 10).

---

## Phase 3: Span (Generated Subspaces)

1. **Linear Combination** (CRITICAL DEFINITION):
   - A **linear combination** of vectors in $S$ is a **FINITE** sum $\sum_{i=1}^{n} a_i v_i$ with $a_i \in K$, $v_i \in S$
   - **Examples**:
     - $3v_1 + 2v_2$ is a linear combination of $\{v_1, v_2\}$
     - The empty sum (= $0$) is a linear combination
     - In $\mathbb{R}^3$: $(1,2,3) = 1 \cdot e_1 + 2 \cdot e_2 + 3 \cdot e_3$ is lin. comb. of standard basis
   - 🎨 **FIGURE**: Span of 2 vectors in $\mathbb{R}^3$ = plane through origin
   - **Non-examples**:
     - Infinite series like $\sum_{i=1}^{\infty} a_i v_i$ are NOT linear combinations
   - **Counterexample**: In $K^{\mathbb{N}}$, let $e_i = (0, \ldots, 0, 1, 0, \ldots)$. Then $(1, 1, 1, \ldots) \notin \Span[K]{\{e_i\}_{i \in \mathbb{N}}}$.

2. **Definition (intersection)**: $\Span[K]{S} = \BigIntersect \Set{ U \Subspace[K] V \mid S \subseteq U }$
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
   - **Union**: $\Span[K]{S \cup T} = \Span[K]{\Span[K]{S} \cup \Span[K]{T}}$
5. **Generating systems (G)**:
   - $G \subseteq V$ is generating iff $\Span[K]{G} = V$
   - We always have a generating system, namely the whole space $V = \Span[K]{V}$
   - **Superset of generating is generating**

---

## Phase 4: Linear Independence

1. **Intuition**: Not all vectors contribute equally to span
2. **Definition**: $v \in S$ is **linearly dependent on** $S \setminus \{v\}$ iff $v \in \Span[K]{S \setminus \{v\}}$
   - **Examples**:
     - $(1,2,3)$ is linearly dependent on $(1,0,0), (0,1,0), (0,0,1)$ in $\mathbb{R}^3$, because...
     - $(1,2,3)$ is linearly independent on $(1,0,0), (0,1,0), (0,0,1)$ in $\mathbb{R}^3$, because...
     - $(1,2,3)$ is linearly dependent on $(1,0,0), (0,1,0), (0,0,1)$ in $\mathbb{R}^4$, because...
3. **Unique representation (DEFINE FIRST)**:
   - A vector $v$ has **unique representation** in terms of $L$ iff there exists a **unique finite subset** $F \subseteq L$ and **unique nonzero scalars** $(a_\ell)_{\ell \in F}$ such that $v = \sum_{\ell \in F} a_\ell \ell$
   - Nonzero scalars required for uniqueness (else could add $0 \cdot w$ for any $w$)
4. **Equivalent characterizations of linear independence** (prove all equivalences):
   - (i) no $v \in L$ is dependent on $L \setminus \{v\}$
   - (ii) $\Forall :{v \in L}{\Span[K]{L \setminus \{v\}} \subsetneq \Span[K]{L}}$
   - (iii) Every $v \in \Span[K]{L}$ has unique representation
   - (iv) $\Exists :{v \in \Span[K]{L}}{\text{unique representation}}$
   - (v) $0$ has unique representation (only trivial combination gives $0$)
   State that i and ii imply the rest needs field axioms (invertible non zero elements), while the rest that imply them do not need them in the proof.
4. **Define linear independence as any of the above characterizations**
5. We always have a linearly independent set, namely the empty set.
6. **Subset of linearly independent is linearly independent**

---

## Phase 5: Bases and Dimension

1. **Definition**: $B$ is a **basis** iff $B$ is both linearly independent AND generating
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

6. **Steinitz Exchange Lemma** (Zorn's Lemma proof):
   - Let $L$ be linearly independent, $S$ spanning
   - **Statement**: There exists injection $f: L \to S$ such that $(S \setminus \DirectImage{f}{L}) \cup L$ spans $V$
   - **Proof structure**:
     1. **Define poset**: $\mathcal{P} = \{(L_0, S_0) : L_0 \subseteq L, S_0 \subseteq S, \exists$ bijection $g: L_0 \to S_0$, $(S \setminus S_0) \cup L_0$ spans$\}$
     2. **Apply Zorn**: Chains have upper bounds (union of $L_\alpha$, union of $S_\alpha$, combined bijection); spanning preserved since any $v$ is finite linear combination
     3. **Prove $\bar{L} = L$**: If $u \in L \setminus \bar{L}$, write $u = \sum a_i \ell_i + \sum b_j s_j$; some $b_j \neq 0$ (else $u \in \Span[K]{\bar{L}}$, contradicting lin. indep.); exchange $u \leftrightarrow s_j$ to extend, contradicting maximality

7. **Corollary**: $\Card{L} \leq \Card{S}$ (injection $f: L \to S$)

8. **Remark: Finite-Dimensional Case and Choice**:
   - If $V$ has a **finite generating set**, both Basis Extension and Steinitz hold **without** Axiom of Choice
   - Zorn's Lemma for finite posets reduces to exhaustive search—provable in ZF

9. **Dimension invariance**: Any two bases have the same cardinality (follows from Steinitz)
10. **Definition of dimension**: $\Dim[K]{V}$ = common cardinality of all bases
    - For infinite-dim: use actual cardinal ($\aleph_0$, $\mathfrak{c}$), NOT "$\infty$"
11. **Examples**:
    - **Finite-dim**: $K^n$ (dim $n$), polynomials of deg $\leq n$ (dim $n+1$), $\MatSpace{m}{n}[K]$ (dim $mn$)
    - **Infinite-dim**: $K[X]$ (all polynomials, dim $\aleph_0$), $K^{\mathbb{N}}$ (sequences, dim $\mathfrak{c}$), $C([0,1], \mathbb{R})$
12. **Cardinality comparisons**: $\Card{L} \leq \Dim[K]{V} \leq \Card{G}$
13. **Corollary**: Any lin indep system has at most $\Dim[K]{V}$ elements, any generating has at least.

14. **Cardinal Properties of Vector Spaces**:
    - **(1)** Lower bound: $\max(\Card{K}, \Card{B}) \leq \Card{V}$
    - **(2)** Bijection with disjoint union: $V \sim \BigUnion[n \in \mathbb{N}] (K \setminus \{0\})^n \times \binom{B}{n}$
      - Disjoint ⟹ can sum cardinals: $\Card{V} = \sum_{n \in \mathbb{N}} (\Card{K}-1)^n \cdot \binom{\Card{B}}{n}$
    - **(3)** Finite case: $\binom{m}{n} = 0$ for $n > m$ ⟹ finite sum ⟹ Newton's binomial: $\Card{V} = \Card{K}^{\Card{B}}$
    - **(4)** Infinite case: If $\max(\Card{K}, \Card{B}) = \kappa \geq \aleph_0$:
      - $\Card{V} \leq \sum_{n \in \mathbb{N}} \kappa^n \cdot \kappa^n = \sum_{n \in \mathbb{N}} \kappa = \aleph_0 \cdot \kappa = \kappa$
    - Use `\sum_{n \in \mathbb{N}}` not `\sum_{n=0}^{\infty}`, `\Card{X}` macro

---

## Phase 6: Ordered Bases and Coordinates

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
6. **Standard Canonical Bases**:
   - $K^n = K^{(\{1,\ldots,n\})}$: $(e_1, \ldots, e_n)$
   - $K^{(\kappa)}$: $(e_\alpha)_{\alpha < \kappa}$
   - $K[X]$: $(1, X, X^2, \ldots)$
7. **Cardinal Properties via $K^{(I)}$** (corollary of coordinate isomorphism):
   - Since $V \cong K^{(I)}$, study cardinal properties of $K^{(I)}$:
     - (1) Lower bound: embeddings $k \mapsto k \cdot e_{i_0}$, $i \mapsto e_i$
     - (2) Bijection: disjoint union over support sizes
     - (3) Finite: $K^{(I)} = K^I$ when $I$ finite, so $\Card{K^{(I)}} = \Card{K}^{\Card{I}}$ by definition
     - (4) Infinite: $\sum_{n \in \mathbb{N}} \kappa = \aleph_0 \cdot \kappa = \kappa$
8. **Add macro**: `\CoordVec{v}[\mathcal{B}]` for coordinate vector

---

## Phase 7: Minkowski Structure on $\mathcal{P}(V)$

1. **Minkowski sum**: $\MinkowskiSum{A}{B} = \Set{a+b \mid a \in A, b \in B}$
2. **Minkowski scalar-set product**: $\MinkowskiProd{L}{A} = \Set{\lambda a \mid \lambda \in L, a \in A}$ for $L \subseteq K$, $A \subseteq V$
   - Special case: $\MinkowskiProd{\{\lambda\}}{A} = \lambda A$ and $\MinkowskiSum{u}{U} = \{u\} + U$

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
    - $(A+B) \cap (A+C) \not\subseteq A + (B \cap C)$: $A=\{0,1\}$, $B=\{0\}$, $C=\{1\}$ → RHS empty, LHS = $\{1\}$
    - $LA \cap LB \not\subseteq L(A \cap B)$: $L=\{1,2\}$, $A=\{1\}$, $B=\{2\}$ → RHS empty, LHS = $\{2\}$

12. **Singleton Equality for Intersections** (Proposition):
    - $\{a\} + \bigcap_i B_i = \bigcap_i (\{a\} + B_i)$
    - $\{\lambda\} \cdot \bigcap_i A_i = \bigcap_i \{\lambda\} A_i$
    - **Proof**: $x \in \bigcap_i (\{a\} + B_i)$ ⟹ $x = a + b_i$ but $b_i = x - a$ is same for all $i$

13. **Minkowski sum of subspaces is a subspace** (prove)
14. **Sum of subspaces**: $\SubspaceSum{U}{W} = \Span[K]{U \cup W}$
15. **Arbitrary sums**: $\SubspaceSum{U_i}_{i \in I} = \Span[K]{\BigUnion[i \in I] U_i}$
16. **Sum of Spans Proposition**: $\SubspaceSum{i \in I}{\Span[K]{S_i}} = \Span[K]{\BigUnion[i \in I] S_i}$ (with proof)

---

## Phase 8: Internal Direct Sums

**Definition of unique representation first, then direct sums:**

1. **Unique representation in a family of subspaces**:
   - Given $(U_i)_{i \in I}$ subspaces, a vector $v$ has **unique representation** iff there exists a **unique finite subset** $F \subseteq I$ and **unique nonzero vectors** $(u_i)_{i \in F}$ with $u_i \in U_i$ such that $v = \sum_{i \in F} u_i$
   - Nonzero components required for uniqueness (else could add $0 \in U_j$ for any $j$)

2. **Definition (arbitrary family)**: $(U_i)_{i \in I}$ is an **internal direct sum family** (notation: $\bigoplus_{i \in I} U_i$) iff every vector in $\sum_{i \in I} U_i$ has unique representation
   - This defines a subspace, NOT necessarily equal to $V$

3. **Equivalent characterization** (prove both directions):
   - $(U_i)$ is direct sum family iff $\Forall :{j \in I}{U_j \cap \SubspaceSum{i \neq j}{U_i} = \{0\}}$
   - **Proof ($\Rightarrow$)**: If $u \in U_j \cap \sum_{i \neq j} U_i$, then $u = \sum_{i \neq j} u_i$, so $0 = u + \sum_{i \neq j}(-u_i)$. By uniqueness, all components are $0$.
   - **Proof ($\Leftarrow$)**: If $\sum u_i = \sum u'_i$, then $u_j - u'_j = \sum_{i \neq j}(u'_i - u_i) \in U_j \cap \sum_{i \neq j} U_i = \{0\}$.

4. **$V$ decomposes as direct sum**: $V = \bigoplus_{i \in I} U_i$ iff $(U_i)$ is direct sum family AND $V = \sum U_i$
   - Equivalently: every $v \in V$ has unique representation in the family

5. **Two-subspace criterion**: $U \oplus W = U + W$ with the condition $U \cap W = \{0\}$
   - **Proof**: Specialize the general characterization to $I = \{1, 2\}$
   - **Decomposition**: $V = U \oplus W$ iff $V = U + W$ and $U \cap W = \{0\}$
   - 🎨 **FIGURE**: $\mathbb{R}^3 = $ (xy-plane) $\oplus$ (z-axis)

6. **Basis decomposition**: $B$ basis iff $V = \DirectSum{v \in B}{\Span[K]{\{v\}}}$ (prove using unique representation of lin. indep.)

7. **Grassmann formula**: $\Dim[K]{U + W} + \Dim[K]{U \cap W} = \Dim[K]{U} + \Dim[K]{W}$
   - **Proof**: Extend basis of $U \cap W$ to bases of $U$ and $W$ separately, show union is basis of $U + W$
   - 🎨 **FIGURE**: Two planes intersecting in a line in $\mathbb{R}^3$

---

# CHAPTER 2: Linear Maps

**File: `Document/LinearAlgebra/VectorSpaces/LinearMaps/`**

## Phase 9: Linear Transformations

### File: `Document/LinearAlgebra/LinearMaps/Definition.tex`

1. **Definition**: Perserves vector space structure $T(u + v) = T(u) + T(v)$ and $T(au) = aT(u)$ or equivalently $T(au + bv) = aT(u) + bT(v)$
   - **Examples**:
      - $T(v) = av$ is a linear transformation for any $a \in K$
      - $T(v) = v$ is the identity transformation
      - $T(v) = 0$ is the zero transformation
      - more examples
2. **Universal Property**: Unique extension from basis
   - 📊 **DIAGRAM**: Triangle showing $B \to V \to W$ with unique extension
3. **Basic Properties**:
   - $T(0) = 0$
   - $\DirectImage{T}{W}$ is subspace when $W$ is subspace
   - $\InverseImage{T}{W}$ is subspace when $W$ is subspace
4. **Fundamental Subspaces**:
   - $\Ker{T} = \InverseImage{T}{\{0\}}$
   - Image = $\DirectImage{T}{U}$
   - Both are subspaces (prove)

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
        \item $T$ is a split monomorphism (has left inverse, when $U \neq \{0\}$)
        \item $T$ is monic (left-cancellable)
        \item $T$ sends every linearly independent set to linearly independent set
        \item $T$ sends some basis to linearly independent set
        \item $T$ sends every basis to linearly independent set
    \end{enumerate}
\end{theorem}
```

**Proof order**: (1)⟺(2), then (1)/(2)⟹(3)⟹(4)⟹(1)/(2), then (1)⟹(5)⟹(6)⟹(7)⟹(1).

### Surjectivity

```latex
\begin{theorem}[Characterizations of Surjectivity]
    For $T: U \to V$ linear, TFAE:
    \begin{enumerate}
        \item $T$ is surjective
        \item $\DirectImage{T}{U} = V$
        \item $T$ is a split epimorphism (has right inverse, when $V \neq \{0\}$)
        \item $T$ is epic (right-cancellable)
        \item $T$ sends every generating system to generating system
        \item $T$ sends some basis to generating system
        \item $T$ sends every basis to generating system
    \end{enumerate}
\end{theorem}
```

### Bijectivity

```latex
\begin{theorem}[Characterizations of Bijectivity]
    For $T: U \to V$ linear, TFAE:
    \begin{enumerate}
        \item $T$ is bijective
        \item $T$ is injective and surjective
        \item $T$ is an isomorphism (has two-sided linear inverse)
        \item $T$ is monic and epic
        \item $T$ sends some basis to a basis
        \item $T$ sends every basis to a basis
    \end{enumerate}
\end{theorem}
```

### Finite dimensional endomorphisms

```latex
\begin{theorem}[Characterizations of Finite Dimensional Endomorphisms]
    For $T: V \to V$ linear, TFAE:
    \begin{enumerate}
        \item $T$ is injective
        \item $T$ is surjective
        \item $T$ is bijective
    \end{enumerate}
\end{theorem}
```
-Counterexample if not endomorphism or if not finite dimensional.

**Do NOT use rank-nullity here.**

---

## Phase 10b: Rank-Nullity Theorem (via basis extension)

**Prove BEFORE quotient spaces, using basis extension directly.**

1. **Rank-Nullity**: $\Dim[K]{U} = \Rank{T} + \Nullity{T}$
   - Proof: Extend basis of $\Ker{T}$ to basis of $U$; show remaining basis vectors map to basis of $\Rng{T}$
2. **Corollaries** (finite-dimensional):
   - Injective + same dimension ⟹ surjective
   - Surjective + same dimension ⟹ injective

---

# CHAPTER 3: Quotient Spaces & Isomorphism Theorems

**File: `Document/LinearAlgebra/VectorSpaces/QuotientSpaces/`**

## Phase 11: Quotient Spaces

### File: `Document/LinearAlgebra/VectorSpaces/QuotientSpaces.tex`

1. **Construction**: Equivalence $u \sim v$ iff $u - v \in W$
2. **Vector space structure** (verify well-definedness, axioms)
3. **Canonical projection**: $\pi: V \to \QuotientSpace{V}{W}$, $\pi(v) = \EquivClass{v}$
   - $\pi$ is surjective linear
   - $\Ker{\pi} = W$

4. **Dimension formula**: $\Dim[K]{W} + \Dim[K]{\QuotientSpace{V}{W}} = \Dim[K]{V}$ (additive form; prove via basis extension)
   - Only on finite dimensions can we say $\Dim[K]{\QuotientSpace{V}{W}} = \Dim[K]{V} - \Dim[K]{W}$

**NOW hyperplanes can be properly defined (requires kernel, dimension):**

5. **Definition**: $H \Subspace[K] V$ is a **hyperplane** iff $\Dim[K]{\QuotientSpace{V}{H}} = 1$
   - **Equivalent (all dimensions)**: $\Exists :{\text{1-dim } L}{H \oplus L = V}$
   - **Equivalent (all dimensions)**: $H = \Ker{f}$ for some non-zero linear map $f: V \to K$
   - **Equivalent (FINITE-DIM ONLY)**: $\Dim[K]{H} = \Dim[K]{V} - 1$

6. **Hyperplane Examples**:
   - **Finite-dimensional**: In $K^n$, the hyperplane $\Set{(x_1, \ldots, x_n) \mid x_1 + x_2 + \cdots + x_n = 0} = \Ker{f}$ where $f(x) = \sum x_i$
   - **Infinite-dimensional**: In $K[X]$ (polynomials), $\Set{p \in K[X] \mid p(0) = 0}$ is a hyperplane (kernel of evaluation at 0)
   - **Infinite-dimensional**: In $K^{(\mathbb{N})}$, the set $\Set{(a_n) \mid \sum_{n=0}^{\infty} a_n = 0}$ (finite sums!) is a hyperplane

7. **CRITICAL Counterexample (Cardinal vs Dimension)**:
   - Let $V = K[X]$ with $\Dim[K]{V} = \aleph_0$ (basis: $1, X, X^2, \ldots$)
   - Let $W = \Set{p \in K[X] \mid \deg(p) \geq 2 \text{ or } p = 0} = \Span[K]{\Set{X^n \mid n \geq 2}}$
   - Then $\Dim[K]{W} = \aleph_0$ and $\Card{\aleph_0} + 1 = \aleph_0 = \Dim[K]{V}$ (cardinal arithmetic!)
   - **But $W$ is NOT a hyperplane**: $\QuotientSpace{V}{W} \cong \Span[K]{\Set{1, X}}$ has $\Dim[K]{\QuotientSpace{V}{W}} = 2 \neq 1$
   - **Lesson**: In infinite dimensions, $\Dim[K]{W} + 1 = \Dim[K]{V}$ (cardinally) does NOT imply $W$ is a hyperplane
   - **Correct criterion**: Always use $\Dim[K]{\QuotientSpace{V}{W}} = 1$, not cardinal subtraction

8. **Every subspace is intersection of hyperplanes** (all dimensions):
   - Convention: empty intersection = whole space $V$
   - For infinite-dim, the intersection may be over infinitely many hyperplanes
   - Key for implicit equations (finite case): $U = \Ker{f_1} \cap \cdots \cap \Ker{f_r}$ where $f_i: V \to K$ linear

9. **Universal Property** (CRITICAL - prove fully):

📊 **DIAGRAM**: Canonical triangle $V \xrightarrow{\pi} \QuotientSpace{V}{W} \xrightarrow{\tilde{T}} X$ with $T = \tilde{T} \circ \pi$
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

**Proof**:
- (1) ($\Rightarrow$): If $\tilde{T}$ exists and $w \in W$, then $T(w) = \tilde{T}(\pi(w)) = \tilde{T}(\EquivClass{0}) = \tilde{T}(0) = 0$.
- (1) ($\Leftarrow$): Define $\tilde{T}(\EquivClass{v}) = T(v)$. Well-defined since $\EquivClass{v} = \EquivClass{u}$ implies $v - u \in W \subseteq \Ker{T}$.
- (2): $\pi$ surjective forces uniqueness.
- (3): $\tilde{T}$ surjective iff $\DirectImage{\tilde{T}}{\Quotient{V}{W}} = X$ iff $\DirectImage{T}{V} = X$.
- (4): $\tilde{T}$ injective iff $\Ker{\tilde{T}} = \{\EquivClass{0}\}$ iff $\InverseImage{\tilde{T}}{\{0\}} = \{\EquivClass{0}\}$ iff $\Ker{T} = W$.
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

📊 **DIAGRAM**: Diamond with $U$, $W$, $U+W$, $U \cap W$ and quotient isomorphism

```latex
\begin{theorem}[Second Isomorphism Theorem]
    Let $U, W \Subspace V$. Then:
    \[
        \Quotient{(U + W)}{W} \cong \Quotient{U}{(U \cap W)}
    \]
\end{theorem}
```

**Proof**: Consider inclusion $\iota: U \hookrightarrow U + W$ composed with projection $\pi: U + W \to \Quotient{(U+W)}{W}$. Show $\Ker{\Composition{\pi}{\iota}} = U \cap W$. Apply first isomorphism theorem.

**Corollary (Grassmann via Isomorphism)**:
- From $\Quotient{(U + W)}{W} \cong \Quotient{U}{(U \cap W)}$, we get $\Dim[K]{\Quotient{(U + W)}{W}} = \Dim[K]{\Quotient{U}{(U \cap W)}}$
- Using additive dimension formulas: $\Dim[K]{W} + \Dim[K]{\Quotient{(U + W)}{W}} = \Dim[K]{U + W}$ and $\Dim[K]{U \cap W} + \Dim[K]{\Quotient{U}{(U \cap W)}} = \Dim[K]{U}$
- Combining: $\Dim[K]{U + W} + \Dim[K]{U \cap W} = \Dim[K]{U} + \Dim[K]{W}$ (ADDITIVE form)
- **Finite-dimensional only**: If $\Dim[K]{V} < \infty$, then $\Dim[K]{U + W} = \Dim[K]{U} + \Dim[K]{W} - \Dim[K]{U \cap W}$

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

# CHAPTER 4: Products, Hom Spaces, and Dual Spaces

## Phase 13: External Direct Sum and Product

1. **Direct product** and **coproduct** definitions
2. **Universal properties**
   - 📊 **DIAGRAM**: Product cone with projections, coproduct cocone with injections
3. **Finite case isomorphism**
4. **Internal vs External**: Internal direct sum $\cong$ external coproduct
5. Classic theorems such as $\Hom[K]{\DirectSum{}{U_i}}{V} \cong \DirectProd{}{\Hom[K]{U_i}{V}}$ (contravariance in the first argument), $\Hom[K]{U}{\DirectProd{}{V_i}} \cong \DirectProd{}{\Hom[K]{U}{V_i}}$ (covariance in the second argument), $\Hom[K]{U}{\DirectSum{}{V_j}} \cong \DirectSum{}{\Hom[K]{U}{V_j}}$ (for U finite dimensional)

---

## Phase 14: Hom Spaces

1. **$\Hom[K]{U}{V}$ is a $K$-vector space**
2. **$\End[K]{V}$ has ring structure** with $\Composition{}{}$, moreover it has a $K$-algebra structure (define what a $K$-algebra is)
3. **Dimension**: $\Dim[K]{\Hom[K]{U}{V}} = \Dim[K]{U} \cdot \Dim[K]{V}$ (finite case), study for infinite dimensions using the theorems above.
4. **Dual space**: $\Dual{V} = \Hom[K]{V}{K}$
5. **Hom functor properties**: $\Hom[K]{\DirectSum{}{U_i}}{V} \cong \DirectProd{}{\Hom[K]{U_i}{V}}$
6. **Natural link to matrices** (see Phase 15)

---

## Phase 15: Dual Spaces

1. **Dual space**: $\Dual{V} = \Hom[K]{V}{K}$ (space of linear functionals)
2. **Dimension of dual**:
   - $\Dim[K]{\Dual{V}} = \Dim[K]{V}$ for finite-dimensional $V$
   - $\Dim[K]{\Dual{V}} > \Dim[K]{V}$ for infinite-dimensional $V$ (prove using $K^{(\kappa)}$ vs $K^\kappa$)
3. **Non-canonical isomorphism**: For finite-dim $V$, $V \cong \Dual{V}$ via any basis, but isomorphism depends on choice of basis
4. **Dual basis**: Given ordered basis $(b_1, \ldots, b_n)$, define $(b^1, \ldots, b^n)$ by $b^i(b_j) = \delta_{ij}$
   - Dual basis is always linearly independent
   - Is a basis iff $V$ is finite-dimensional
5. **Annihilators**:
   - For $S \subseteq V$, define $\Ann{S} = \Set{\varphi \in \Dual{V} \mid \Forall :{v \in S}{\varphi(v) = 0}}$
   - $\Ann{S}$ is a subspace of $\Dual{V}$
   - $\Dim[K]{\Ann{U}} + \Dim[K]{U} = \Dim[K]{V}$ for $U \Subspace[K] V$ (finite case)
6. **Dual of a linear map**: For $T: U \to V$, define $\DualMap{T}: \Dual{V} \to \Dual{U}$ by $\DualMap{T}(\varphi) = \Composition{\varphi}{T}$
   - 📊 **DIAGRAM**: Contravariant square showing $T$ and $\DualMap{T}$ in opposite directions
7. **Dual map properties**:
   - $\DualMap{T}$ injective iff $T$ surjective
   - $\DualMap{T}$ surjective iff $T$ injective
8. **Bidual and canonical isomorphism**:
   - Define evaluation map $\eta_V: V \to \Dual{\Dual{V}}$ by $\eta_V(v)(\varphi) = \varphi(v)$
   - $\eta_V$ is always injective (prove)
   - **Canonical isomorphism**: $\eta_V$ is an isomorphism iff $V$ is finite-dimensional
   - This is "canonical" because it doesn't depend on any choice of basis
9. **Matrix representation**: $[\DualMap{T}]_{\DualBasis{\mathcal{C}}}^{\DualBasis{\mathcal{B}}} = \Transpose{[T]_{\mathcal{B}}^{\mathcal{C}}}$
10. **Change of basis formula for dual**

---

# CHAPTER 5: Matrices & Subspace Representations

## Phase 16: Matrices

### 15a: Matrix Fundamentals
1. **Matrix of linear map** relative to **ordered bases**
   - CRITICAL: Requires ordered bases, not just bases
2. **Matrices of any size**: $\MatSpace{m}{n}[K]$ for cardinals $m, n$
3. **Matrix operations**:
   - Addition: $(A + B)_{ij} = A_{ij} + B_{ij}$
   - Scalar multiplication: $(\lambda A)_{ij} = \lambda A_{ij}$
   - Matrix multiplication: $(AB)_{ij} = \sum_k A_{ik} B_{kj}$
   - Hadamard product: $(A \circ B)_{ij} = A_{ij} B_{ij}$
   - Kronecker product: $A \otimes_K B$ (with block structure)
4. **Isomorphism**: $\Hom[K]{U}{V} \cong \MatSpace{n}{m}[K]$ (relative to ordered bases; for any two fixed bases in the domain and codomain there is a unique isomorphism)
5. **Composition corresponds to matrix multiplication**

### 15b: Block Matrices (with all the proofs)
1. **Block matrix**: Matrix with matrix entries that unravel to rectangles
2. **Compatibility**: Row blocks must have same height, column blocks same width
3. **Unraveling morphism property**: For operations $\circ \in \{+, \cdot, \otimes_K, \circ_{\text{Hadamard}}\}$:
   - $\text{unravel}(A \circ B) = \text{unravel}(A) \circ \text{unravel}(B)$
   - i.e., partition into blocks → apply operation as block matrices → unravel = operate as regular matrices

### 15c: Abuse of notation
1. If in the context we are talking about two bases (or the canonical bases if we are working on $K^n$), then for any matrix A we will write $\Ker{A}$ for $\Ker{T_A}$ and $\Rng{A}$ for $\Rng{T_A}$ where $T_A: K^n \to K^m$ is the linear map associated to A (unique via the Hom matrix isomorphism).
2. This is equivalent to defining $\Ker{A} = \Set{x \in K^n \mid Ax = 0}$ and $\Rng{A} = \Set{y \in K^m \mid \exists x \in K^n \text{ such that } Ax = y}$.

### 15d: Linear Systems
1. **Homogeneous system**: $Ax = 0$
   - Solution space = $\Ker{T_A}$ where $T_A: K^n \to K^m$
   - Always a subspace
2. **Non-homogeneous system**: $Ax = b$
   - Solution set = $v_0 + \Ker{T_A}$ (affine subspace/coset)
   - Non-empty iff $b \in \DirectImage{T_A}{K^n}$
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
| $U \cap W$ | Implicit | Concatenate equation systems |
| $\DirectImage{T}{U}$ | Parametric | Apply $T$ to generators |
| $\InverseImage{T}{W}$ | Implicit | Compose equations with $T$: $L_i \circ T = 0$ |

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
2. **Sylvester**: $\Dim[K]{\Ker{\Composition{S}{T}}} = \Dim[K]{\Ker{T}} + \Dim[K]{\Ker{S} \cap \DirectImage{T}{U}}$ (give the rank equivalent)
3. **Frobenius**: $\Dim[K]{\Ker{\Composition{A}{\Composition{B}{C}}}} + \Dim[K]{\Ker{B}} \leq \Dim[K]{\Ker{\Composition{A}{B}}} + \Dim[K]{\Ker{\Composition{B}{C}}}$ (give the rank equivalent)
4. **Generalization to $n$ maps**

---

# CHAPTER 6: Endomorphisms & Spectral Theory

## Phase 22: Endomorphisms - Basics

1. **Definition**: $T \in \End[K]{V}$ (linear map $V \to V$)
2. **Ring structure**: $(\End[K]{V}, +, \Composition{}{})$ is a ring (non-commutative if $\Dim[K]{V} > 1$)
3. **$K$-algebra structure**: $(\End[K]{V}, +, \Composition{}{}, \cdot)$ is a $K$-algebra (non-commutative if $\Dim[K]{V} > 1$)
4. **Projections**:
   - Define projection onto a subspace along another subspace (they have to be in direct sum for well definedness)
   - $P^2 = P$ (idempotent)
   - $V = \Ker{P} \oplus \DirectImage{P}{V}$
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
   $V = U \oplus W$ with both $U, W$ are $T$-invariant iff:
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
3. **Upper/lower triangular relative to ordered basis**: Can choose either convention since reversing basis order swaps upper↔lower (prove this) (i.e. $T$ is upper triangular relative to $(b_1, \ldots, b_n)$ iff lower triangular relative to $(b_n, \ldots, b_1)$ )
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
6. **Diagonalizable iff $V = \bigoplus_{\lambda} E_\lambda(T)$** (sum over eigenspaces)

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
2. **Orthogonal projection**: $\Proj{U}$ with $V = U \oplus \OrthComp{U}$
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
     - **Proof**: If $\varphi = 0$, take $u = 0$. Otherwise, $\Ker{\varphi}$ is hyperplane, so $V = \Ker{\varphi} \oplus \Span[K]{\{w\}}$ for some $w \notin \Ker{\varphi}$. Set $u = \frac{\overline{\varphi(w)}}{\Norm{w}^2} w$. Verify $f_u = \varphi$ by checking on basis.
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
    ├── QuotientSpaces/                  # CHAPTER 3: Quotients & Iso Theorems
    │   ├── QuotientSpaces.tex           # Phase 11: Quotient spaces & UP
    │   ├── Hyperplanes.tex
    │   └── IsomorphismTheorems.tex      # Phase 12: All four iso theorems
    │
    ├── HomAndDual/                      # CHAPTER 4: Products, Hom, Dual
    │   ├── ExternalOperations.tex       # Phase 13: External products
    │   ├── HomSpaces.tex                # Phase 14: Hom spaces
    │   └── DualSpaces.tex               # Phase 15: Dual spaces, annihilators, V≅V**
    │
    ├── Matrices/                        # CHAPTER 5: Matrices & Representations
    │   ├── Matrices.tex                 # Phase 16: Matrix fundamentals
    │   ├── ChangeOfBasis.tex            # Phase 17: Change of basis, Gaussian
    │   ├── SubspaceRepresentations.tex  # Phase 18: Parametric/implicit forms
    │   └── RankInequalities.tex         # Phase 19-21: Rank inequalities
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

