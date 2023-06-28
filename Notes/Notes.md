# Fair Transition System

$$\mathcal{F} = \langle V, \theta, \mathcal{T}, \mathcal{J}, \mathcal{C} \rangle$$

where

- $V$ is a set of typed variables partitioned into *control* and *data* variables (a state $s \in \Sigma$ is an interpretation of $V$: $u \in V \to s[u] \in \mathcal{D}(u)$)
- $\theta$ is a set formula that defines the initial condition (possibly multiple states)
- $\mathcal{T}$ is a finite set of transitions $\tau \in \mathcal{T} : \Sigma \to 2^\Sigma$ ($\tau$ is enabled in $s$ if $\tau \not = \empty$, otherwise it is disabled)
- $\mathcal{J}$ is a set of transitions for which we require *justice*
- $\mathcal{C}$ is a set of transitions for which we require *compassion*

Each transition $\tau \in \mathcal{T}$ can be represented by a formula $\rho_\tau(V,V')$ called *transition relation* $\to$ it expresses a relationship between a state and its $\tau$-successors

The enabling condition can be expressed as $En(\tau) = \exists V' \rho_\tau(V,V')$

We have the *idling transition* $\tau_I$, which is always enabled $\to$ all the other transitions are called *diligent*

Given an infinite sequence of states $\sigma = s_0, s_1, s_2, ...$, we say that $\tau$ is *enabled* at position $k$ if $\tau$ is enabled at state $s_k$ and that it is *executed* at position $k$ if $s_{k+1} \in \tau(s_k)$

A sequence $\sigma$ is a *computation of an FST* if satisfies initiality, consequentiality, justice and compassion
If it satisfies only initiality and consequentiality, it is called a *run*

A state is *P-accessible* if it occours in some P-computation

Evolution of an *SLP* program:

- $\pi$ control variable
- $Y$ data variables
- $move(L,\hat{L}) = L \subseteq \pi \land \pi' = (\pi - L) \cup \hat{L}$ this moves the control of the program (moreless like incrementing the program counter)
- $pres(U) = \bigwedge_{u \in U} (u' = u)$ inertia of non modified variables

**Example**
- Intruction: $l: \ \overline{u}:=\overline{e};\ \hat{l}:$
- $\rho_l: move(l, \hat{l}) \land \hat{u}' = \hat{e} \land (pres(Y) - \{\hat{u}\})$

Properties:

- *Mutual exclusion*: no computation of the program may include a state where both processes are within their cirtical region
- *Accessibility*: every state of a computation where a process is at the end of its non-critical region, must be followed by a state where it is within its critical region
- *Communal accessibility*: each state of computation where a process reaches the end of its non-critical region, is followed by a state where some process, not necessarily the same, is within its critical region

# Learning automata

Idea: teacher has a secret regular language $L_0$ recognised by an automaton $\mathcal{A}_0$. The learner only knows the alphabet $\Sigma$ of the language. The learner must deduce $L_0$ through two types of queries:

- Membership queries: does $w \in L_0$, $w \in \Sigma^*$?
    - Teacher answers yes/no
- Equivalence queries: is $L_0 = L(\mathcal{A})$?
    - Teacher answers yes/a counterexample

Naive algorithm: enumeration over $\Sigma$ and equivalence queries

Complex algorithm: uses Myhill-Nerode equivalence relativized over a test set $T$

$$(\forall t \in T\ \  ut \leftrightarrow vt \in L_0) \to u \approx_{L_0, T}$$

Note:
- If $T = \empty$, we have a single equivalence class
- If $T = \{\epsilon\}$ we obtain equivalence $=_{L_0}$
- The larger $T$, finer the relation $\approx_{L_0, T}$ is

The learner has to build an automaton from pairs $(S,T), S \subseteq \Sigma^*$, where $S$ is *$T$-minimal* and *$T$-complete*

- Minimal: $\forall s \not = s' \in S, s \not \approx_{L_0,T}s'$
- Complete $\forall s \in S, \forall a \in \Sigma, \exists s_a \in S\ \ s_a \approx_{L_0,T} sa$

Learner strategy:

1. Begin with $S=T=\{\epsilon\}$
2. Enlarge $S$ until it is $T$-complete while keeping $T$-minimality
3. Build $\mathcal{A}$ with:
    - state set $S$
    - transitions $\delta(s,a)=s_a$
    - initial state $\epsilon$
    - final states $F = \{f \ | \ Membership(f)\}$
4. Check $Equivalence(\mathcal{A})$
    - if true, found $L_0$
    - if false, update $T$ by including all of the suffixes of the counterexample word and come back to 2.

# Games in logic

The evaluation game: $M \vDash \phi \iff$ Eve has a strategy to win $G_{\phi,M}$

> Eve's objective is to satisfy $\phi$, while Adam's objective is to prevent Alice from satisfying $\phi$. The arena are the subformulas $\alpha$ of $\phi$ and the binding $\lambda:FreeVars(\alpha) \to U^M$, thus when we arrive at a point where $\alpha = R(x_1,...,x_k)$, we have that, if $(\lambda(x_1), ..., \lambda(x_k)) \in R^M$, $\lambda$ satisfies $\phi$, else there is no $M$ such that $M \vDash \phi$

Our objective is to show that a property $P$ (e.g.: strongly connected components) is not definable within a certain theory

1. $P$ is defined by $\phi$ if for every $M$, $M \in P \iff M \vDash \phi$
2. $M,M'$ are *elementary equivalent if for every $\phi$, $M \vDash \phi \iff M' \vDash \phi$

**LEMMA**: if there are $M, M'$ such that $M \in P, M' \not \in P, M,M'$ elementary equivalent, then $P$ is not definable in $FO$

In practice we want to show that there is no $\phi$ that defines $P$, thus $P$ is not definable in $FO$

3. $\phi$ has *quantifier rank* $n$ if it has at most $n$ nested quantifiers
4. $M, M'$ are *$n$-equivalent* if for every $\phi$ with quantified rank $n$, $M \vDash \phi \iff M' \vDash \phi$

**LEMMA (STRONGER VERSION)**: if, for every $n$, there are $M,M'$ such that $M \in P, M' \not \in P, M,M'$ $n$-equivalent, then $P$ is not definable in $FO$

> Our new objective is to determine whether $M,M'$ are $n$-equivalent $\to$ we can construct a new game $G_{M,M'}$ whose winner determines whether $M,M'$ are $n$-equivalent (Duplicator) or not (Spoiler)

*Ehrenfeucht-Fraïssé* game $G_{M,M'}$: play for $n$ rounds on the arena which positions are tuples $(u_1,...,u_i,v_1,...,v_i) \in U^M \times ... \times U^M \times U^{M'} \times ... \times U^{M'}$

1. Spoiler chooses a variable from a structure
2. Duplicator chooses a variable from the other structure
3. Duplicator survives to the $i^{th}$ round if the substructures induced by the choices of the parties are isomorphic
4. If Duplicator survives $n$ rounds, then $M,M'$ are $n$-equivalent (Fraïssé, Ehrenfreucht Theorems)

The trick is to find two structures which are different and have $2^n-1$ variables $\to$ in this way it is pretty easy to show that the Duplicator can survive $n$ rounds for an arbitrary $n$ and, thus, the property is undefinable

**Proof: from Duplicator's strategy to $n$-equivalence**

Starting considerations:
- $\phi$ in NNF
- $M \vDash \phi$, thus Eve has a strategy in $G_{M,\phi}$
- Duplicator survives $n$ rounds in $G_{M,M'}$, thus Duplicator has a strategy in $G_{M,M'}$

I need to prove that $M' \vDash \phi$ by showing that Eve has a strategy for $G_{M',\phi}$
By induction based on the possible types of subformula:
- conjunction: Eve mirrors the move in $G_{M,\phi}$
- disjunction: Adam picks the same conjunct he picked in the $G_{M',\phi}$ game
- existential quantification: Eve binds a variable $x$, the Spoiler places the pebble in the relative position, the Duplicator answers with another variable and Eve binds the value of $x$ to the value picked by the Duplicator in $G_{M', \phi}$
- universal quantification: same steps, bu executed by Adam 

> Our objective is to keep the games $G_{M,\phi}, G_{M,M'}, G_{M',\phi}$ synchronized

**Proof: from $n$-equivalence to Duplicator's strategy**

We need a formula that covers all possible cases in the FO theory with at most $n$ nested quantifiers $\to$ Hintikka formula $\phi_M^n$

Starting considerations: since $M,M'$ are $n$-equivalent, we have that
- $M \vDash \phi_M^n$, thus Eve has a strategy for $G_{M,\phi}$
- $M' \vDash \phi_M^n$, thus Eve has a strategy for $G_{M',\phi}$

Starting from the EF game, we have to show that we can keep the three games synchronized


Note also that:

- $\phi_M^n$ can be used as a representat of the $n$-equivalence class of $M$
- For every $\phi'$ of quantified rank $n$, $\phi' \in FO[M] \iff \phi_M^n \vDash \phi'$

# Star free Regex

Remember:

1. S1S in the finite setting is as expressive as regular expressions (recognizable by DFA), both restricted and not
2. S1S (and WS1S) are as expressive as $\omega$-regular expressions (recognizable by Büchi automata)

We want to show that *star-free regular expressions* are as expressive as a fragment of S1S where quantification is allowed only on first-order variables

Note that +1 is definable through second order and **NOT** first-order quantification

**$W$ star-free language $\to W$ first-order definable in $S1S_A$**

Easy proof by induction on the structure of the star free expression: corrispondence of $\cup, !, \cdot$ with $\lor, \neg, \exists$

> Example: $A^* (a \cup c) \overline{(A^* b A^*)}$ becomes $\exists x((x \in Q_a \lor x \in Q_c) \land \neg \exists y (x < y \land y \in Q_b))$

**$W$ first-order definable in $S1S_A \to W$ star-free language**

The proof is by induction on the quantifier depth of the formula
Hardest case: existential quantifier $\exists x \phi(x)$ of depth $n+1$ ($\to$ we have a star free expression for $\phi(x)$ by inductive hypothesis)

We want to rewrite $\exists x \phi(x)$ in the form

$$\exists x (\phi_{<x} \land x \in Q_a \land \phi_{>x})$$

This formula describes the language $UaV$ where $U,V$ are star free by hypothesis

We introduce two equivalence relations:

1. $\forall u,v \in A^*, u \equiv_n v \iff u$ and $v$ satisfy the same sentences (formulas without free variables) of quantifier depth $n$
2. $\forall u,v \in A^*, r,s \in \mathbf{N}, (u,r) \equiv_n (v,s) \iff (u,r)$ and $(v,s)$ satisfy the same formulas $\phi(x)$ of quantifier depth $n$ 

Facts:
1. Both relations are of finite index
2. For any $n$, any equivalence class of either relation can be defined by a sentence (or formula) of quantifier depth $n$ (moreless like a characteristic function)
3. Any formula $\phi(x)$ is equivalent to a disjunction of formulas $\phi_{\underline{W}}(x)$
4. The two relations are congruences
    - $(u \equiv_n v \land u' \equiv_n v') \to uu' \equiv_n vv'$
    - $(u \equiv_n v \land a \in A \land u' \equiv_n v') \to (uau', |u|+1) \equiv_n (vav', |v|+1)$

The proof of 4. is based on the EF-game theoretic characterization of $\equiv_n$ and $\equiv_{(n,1)}$: if Duplicator has a $n$-round strategy for games $G_{u,v}$ and $G_{u',v'}$, then it has an $n$-round strategy for $G_{uu',vv'}$

Note that $\exists x \phi (x) \iff \bigvee_{\underline{W}}\exists x \phi_{\underline{W}} (x)$. Let's consider a triple $(U,a,V)$ with $U,V \equiv_n$-classes. If there exists $u_0 \in U$ and $v_0 \in V$ such that $(u_0av_0, |u_0|+1) \in \underline{W}$, then by 4. it holds that $(uav, |u|+1) \in \underline{W}$ forall $u \in U, v \in V$. Thus $UaV$ satisfies $\exists x \phi_{\underline{W}} (x)$

# Temporal logics

Temporal logic is a special form of *modal* logic where the accessibility relation is time

**Propositional Linear Temporal Logic**

Syntax:
- A set of propositional letters $AP = \{ P, Q, ... \}$
- A set of propositional connectives $\{\land, \neg\}$
- A set of temporal connectives $\{ X, U \}$

Structures $M:(S,x,L)$:
- $S$ set of states
- $x: \mathbf{N} \to S$ sequence of states
- $L: S \to 2^{AP}$ labelling function (it identifies which proposition letters are true in a state)

Note that $x^i = (x(i), x(i+1), x(i+2), ...)$

Semantics:
- $M, x^i \vDash P \iff P \in L(s_i)$
- $M, x^i \vDash p \land q \iff M, x^i \vDash p \land M, x^i \vDash q$
- ...

Versions of until $p\  U q$:
- weak $_\forall$: there may never be an instant in which $q$ is true
- strong $_\exists$: there must exists an instant in which $q$ is true
- strict $^>$: $q$ must be true in the future (not current timestamp) to count
- non-strict $^\geq$: also present counts for $q$

Note: the various versions can be inter-defined
- $x \vDash p \ U_\forall q \iff x \vDash p \ U_\exists q \lor Gp$
- $x \vDash p \ U^{\geq} q \iff x \vDash p \ U^{>}q \ \lor q$

# Tablau-based decision for LTL

Our method:
- *Implicit*: the accessibility relation is built-in into the structure of the tableau
- *Declarative*: first generate all possible sets of subformulae of a given formula an then prune

*Closure $\Phi_\phi$ of a formula $\phi$*: smallest set such that
- $\phi \in \Phi_\phi$
- $p \in \Phi_\phi \to \neg p \in \Phi_\phi$
- $p \in \Phi_\phi \land q \textrm{ subformula of } p \to q \in \Phi_\phi$
- $\psi \in \{Gp, Fp, p\ U q\}, \psi \in \Phi_\phi \to X\psi \in \Phi_\phi$

$\alpha$ and $\beta$ formulas:

| $\alpha$ | $k(\alpha)$ |
| :--: | :--: |
| $p \land q$ | $p,q$ |
| $Gp$ | $p, XGp$


| $\beta$ | $k_1(\beta)$ | $k_2(\beta)$ |
| :--: | :--: | :--: |
| $p \lor q$ | $p$ | $q$ |
| $Fp$ | $p$ | $XFp$ |
| $p\ U q$ | $q$ | $p, X(p\ U q)$ |

A $\phi$-atom is a subset of $\Phi_\phi$ such that satisfies the 4 rules ($R_{sat}, R_\neg, R_\alpha, R_\beta$)

We can build the tableau $T_\phi$ relative to a formula $\phi$ as a graph where:
- the atoms of $\phi$ are the nodes
- $\forall Xp \in \Phi_\phi, Xp \in A \iff p \in B \to$ there is an edge from $A$ to $B$

Given a model $\sigma$ of $\phi$, the infinite path $\phi_\sigma: A_0, A_1, ...$ in $T_\phi$ is induced by $\sigma$ if for every position $j$ and every $p \in \Phi_\phi, (\sigma,j) \vDash p \iff p \in A_j$
Note that:
- for each model there exists an infinite path in $T_\phi$ induced by it
- $\phi \in A_0$, thus $(\sigma,0) \vDash \phi$

A formula $\psi \in \Phi_\phi$ *promises* $r$ if it has one of the following forms:
- $Fr$
- $p\ U q$
- $\neg G \neg r$

An atom *fulfills* a formula $\psi$ that promises $r$ if $\neg \psi \in A \lor r \in A$. A path is *fulfilling* if each promising formula is fulfilled infinitely many times by atoms in it

Note that:
1. Fulfilling paths induce models
2. Models induce fulfilling paths
3. A formula $\phi$ is satisfiable iff the tableau $T_\phi$ contains a fulfilling path $\phi = A_0, A_1, ...$ such that $\phi \in A_0$ (follows from 1. and 2.)

A *non-transient strongly connected subgraph* (SCS) is fulfulling if every formula $\psi \in \Phi_\phi$ that promises $r$ is fulfilled by some atom $A \in S$

An SCS $S$ is $\phi$ reachable if there exists an infinite path that leads from a $\phi$-atom to an atom $A \in S$

> A formula is satisfiable if we have a reachable fulfilling SCS

We can do a step forward and only considerate *Maximal SCS* (SCS not contained in larger SCS)

# Model checking for LTL

Given an atom $A$, $state(A)$ is the conjunction of all of its state formulas in $A$ (e.g: $p, q, p \land q, \neg p$, etc...). $state(A)$ is satisfiable for $R_{sat}$

An atom $A$ is *consistent* with a state $s$ if $s \vDash state(A)$

Let:
- $\theta: A_0,A_1,...$ be a path in $T_\phi$
- $\sigma:s_0,s_1,...$ be a computation of $P$

We have that $\theta$ is a *trail* of $T_\phi$ over $\sigma$ if $A_j$ is consistent with $s_j$, forall $j$

The successor (outgoing edges) relation in $T_\phi$ is called $\delta$

Given:
- A finite state program $P$
- An LTL formula $\phi$

We can build the *behaviour graph* of $(P, \phi)$, denoted $\mathcal{B}_{(P, \phi)}$ as the product of the graph for $P$ ($G_P$) and the tableau for $\phi$. Note that:

1. Nodes are in the form $(s,A)$, where $s$ is a state and $A$ an atom
2. There is a *$\tau$-labeled edge* from $(s,A)$ to $(s',A')$ if:
    - $s'$ is a $\tau$-successor of $s$
    - $A' \in \delta(A)$ in $T_\phi$ (pruned)
3. A node is *$\phi$-initial* if:
    - $s$ is an initial state for $P$
    - $\phi \in A$ and $s$ is consistent with $A$

The algorithm for building $\mathcal{B}_{(P,\phi)}$ starts from adding all $\phi$-initial nodes and increases the graph with the relative reachable nodes (and arcs) until a fixpoint (no more nodes added) is reached

> Note that fairness conditions can be expressed as LTL formulas $\to$ I can keep them "in the background" while checking the graph

**PROPOSITIONS**

1. An infinite sequence $\pi: (s_0, A_0),(s_1, A_1), ...$, with $(s_0, A_0)$ is an initial $\phi$-node, is a path in $\mathcal{B}_{(P, \phi)}$ $\iff \sigma_\pi : s_0,s_1,...$ is a run of $P$ and $\theta_\pi$ is a trail of $T_\phi$ over $\sigma_\pi$
2. There is a $P$-computation that satisfies $\phi \iff$ there is an infinite path $\pi$ in $\mathcal{B}_{(P, \phi)}$, starting from an initial $\phi$-node, such that $\sigma_\pi$ is a computation and $\theta_\pi$ is a fulfilling trail over $\sigma_\pi$

**DEFINITIONS/FACTS**

Given a behaviour graph $\mathcal{B}_{(P, \phi)}$

1. $(s',A')$ is a *$\tau$-successor* of $(s,A)$ if there is a $\tau$ edge such that $(s,A) \to^\tau (s'A')$
2. A transition $\tau$ is *enabled* on $s \to \tau$ is enabled on all $(s,A)$

Given a subgraph $S \subseteq \mathcal{B}_{(P,\phi)}$

3. A transition $\tau$ is *taken* in $S$ if there are two nodes $A, B \in S \ . \  A \to^\tau B$
4. $S$ is *just* (*compassionate*) if every just (compassionate) transition $\tau \in \mathcal{J}$ ($\tau \in \mathcal{C}$) is either taken in $S$ or is disabled on some (all) nodes in $S$
5. $S$ is *fair* if is both just and compassionate
6. $S$ is *fulfilling* if every promising formula is fulfilled by an atom in a node of $S$
7. $S$ is *adequate* if it is both fair and fulfilling

Our objective is now to find an adequate strongly connected subgraph for the behavior graph

Note that:

- If $S \subseteq \mathcal{B}_{(P, \phi)}$ is just, then the whole graph $\mathcal{B}_{(P, \phi)}$ is just
- If $S \subseteq \mathcal{B}_{(P, \phi)}$ is fulfilling, then the whole graph $\mathcal{B}_{(P, \phi)}$ is fulfilling
- If $S \subseteq \mathcal{B}_{(P, \phi)}$ is compassionate, then it's not necessarily true that the whole graph $\mathcal{B}_{(P, \phi)}$ is compassionate

If we have a strongly connected subgraph $S$ that is not compassionate, we can always recursively search within its SCSs if there is any adequate $S'$ (by always checking also for justice and fulfilness)

# Translation from LTL+P to Büchi automata

New operators:

- $Yp$: the past timestamp has to exist (not trivial because the past is always finite) and $p$ must be satisfied at it
- $Zp$: $p$ must be satisfied at the past instant if it exists
- $p\ S\  q$: there must have been a timestamp in the past in which $q$ was satisfied and since then until the last timestamp $p$ has been satisfied at each timestamp

Note that $\neg Yp = Z \neg p$

We now define the *expanded closure* of a LTL+P formula $\phi$ as the smallest set of formulas such that:

- $\phi \in \Phi_\phi$
- $\alpha \in \Phi_\phi \to \neg \alpha \in \Phi_\phi$
- $\alpha \in \Phi_\phi \land \beta \textrm{ subformula of } \alpha \to \beta \in \Phi_\phi$
- $\alpha \in \Phi_\phi \land \alpha = \gamma\ U\ \psi \to X\alpha \in \Phi_\phi$
- $\alpha \in \Phi_\phi \land \alpha = \gamma\ S\ \psi \to Y\alpha \in \Phi_\phi$

**CONSTRUCTION OF THE AUTOMATON**

An atom $A$ is a subset of $\Phi_\phi$ such that:
1. $p \in A \iff \neg p \not \in A$
2. $p \land q \in A \iff p, q \in A$, etc...
3. $p\ U\ q \in A \iff q \in A \lor p, X(p\ U\ q) \in A$
4. $p\ S\ q \in A \iff q \in A \lor p, Y(p\ S\ q) \in A$

Having the set of nodes at our disposal, we can build the automaton $\mathcal{A}_\phi(States, edges)$: 

- An atom $A$ is *initial* in $A_\phi \iff \phi \in A$ and $A$ does not contain any $Yp$ or $\neg Z p$ formulas (no requirements on the past)
- $A$ reaches $B$ in $\mathcal{A}_\phi$ reading symbol $s$ when:
    1. $p \in A \iff p \in s \ \ \forall p \in \Sigma \cap \Phi_\phi$
    2. $Xp \in A \iff p \in B \ \ \forall Xp \in \Phi_\phi$
    3. $Yp \in B \iff p \in A \ \ \forall Yp \in \Phi_\phi$
    4. $Zp \in B \iff p \in A \ \ \forall Zp \in \Phi_\phi$
- For each promising formula $\alpha \in \{p \ U\ q\} \cap \Phi_\phi$, the atom $A_\alpha$ is $\alpha$-fulfilling $\iff p \in A_\alpha \to q \in A_\alpha$
    - Muller condition: $In(\rho)=F, F \in \mathcal{F}$
    $$\mathcal{F} = \{ F \subseteq States\  |\  \forall \alpha \in \{ p\ U\ q \} \cap \Phi_\phi \ . \ \exists A_\alpha \in F \}$$
    - Generalized Büchi condition: $In(\rho) \cap F \not = \empty, F \in \mathcal{F}$
    $$\mathcal{F} = \{ F_\alpha \subseteq States\  |\  \forall \alpha \in \{ p\ U\ q \} \cap \Phi_\phi \ . F_\alpha \textrm{ is the set of all $\alpha$-fulfilling states} \}$$

Note that the translation from $\phi$ to $\mathcal{A}_\phi$ belongs to $EXPSPACE$, but to check if the formula is satisfiable we can use Savitch's reachability method (to check if the final states are reachable from the initial states), which requires $NL$ space and allows on-the-fly generation of parts of the graph: overall, the reduction belongs to $PSPACE$

# Branching temporal logic (CTL)

A model $\mathcal{M}:(S,R,L)$ now features a relation $R \subseteq S \times S$ instead of a function

With such structure, $\mathcal{M}$ is a graph $\to$ to obtain a tree we need to unfold it

To the syntax we add quantifiers over computations (*path quantifiers*):

- $Ap$: forall possibile futures, $p$ is verified
- $Ep$: there exists a future where $p$ is verified

CTL forces path and state quantifiers to be interleaved

CTL* doesn't enforce any restriction (it uniforms LTL and CTL), but it requires that any formula with quantifiers begins with a path quantifier (both CTL and CTL* are sets of *state formulas*)

LTL and CTL are incomparable in terms of expressiveness (both are proper fragments of CTL*)

1. The following CTL formula can not be expressed in LTL: $AG(P \to EFQ)$ (because in CTL we can have infinite paths behaving differently starting from a state)
2. The following LTL formula can not be expressed in CTL:
$G(P \to FGP)$ (we cannot use two juxtaposed path quantifiers)

# Model checking for CTL

Recursive definitions for the added operators:

- $EU(\alpha, \beta) = \beta \lor (\alpha \land EXEU(\alpha, \beta))$
- $AU(\alpha, \beta) = \beta \lor (\alpha \land AXAU(\alpha, \beta))$

The Model Checking procedure for CTL takes as input a formula $\alpha$ and a Kripke structure $\mathcal{M}=(M,R,V)$ and builds iteratively the set of true formulas $V(w)$ for each state $w \in M$. It proceeds iteractively on the size $i = 1, ..., |\alpha|$ of the subformulas of $\alpha$ by enumerating them and checking for each case. Let's suppose we have a subformula $\beta$ of size $i$

1. $\beta$ is a propositional letter $\to$ straightforwardly build $V(w)\  \forall w \in M$
2. $\beta = EX\gamma \to$ for each $w \in M$, if there exists a successor $v$ of $w$ such that $\gamma \in V(v)$, then $V(w) = V(w) \cup \{ \beta \}$
3. $\beta = AX\gamma \to$ for each $w \in M$, if all successors $v$ of $w$ are such that $\gamma \in V(v)$, then $V(w) = V(w) \cup \{ \beta \}$
4. $\beta = EU(\gamma, \delta) \to$ search for all successive states $w$ in which $\delta \in V(w)$ and proceed backwards into the precedessors of $w$ to see recursively since when the formula held (thus until when $\gamma$ was true). For all of these states $w$, we have that $V(w) = V(w) \cup \{ \beta \}$ 
5. $\beta = AU(\gamma, \delta) \to$ the procedure is similar to 4., but it proceeds forwards to check all possible successors of a state $w \in M$. The valutation of a state $V(w)$ is incremented with $\beta$ if $\delta \in V(w)$ or if for all of its successors $v$, $AU(\gamma, \delta) \in V(v)$ (NOTE: the procedure goes forward and then "comes back" to $w$ to, in case, add $AU(\gamma, \delta)$ to its valutation)

> NOTE: generally, a model checking problem is defined as $\mathcal{M},w \vDash \alpha$. The above procedure "populates" the labeling function of $\mathcal{M}$ with all the true formulae for each state $w \in M$. To actually "model check" a formula $\alpha$, we would need to execute the above procedure, require a state $w$ for the valuation and then see if $\alpha \in V(w)$

Complexity: $\mathcal{O}(k(n+m))$, where $k$ is the number of times the loop runs ($|\alpha|$), $n = |M|$ and $m=|R|$. The algorithm is linear in the product of the length of the formula and the size of the model

# Symbolic model checking

If a system is made up of multiple components that can make transitions in parallel, we must take into consideration the cartesian product of the models of all the components for model checking $\to$ there can be a state explosion

Possible approaches:

1. Symbolic model checking: manipulating sets of states through their definitions instead of actual single states. It requires the fast application of boolean operations ($\to$ OBDDs)
2. Partial ordering reduction: not all orderings of events in a concurrent system are relevant for the properties of interest. By considering only a subset of relevant orderings, the exploration of the state space is significantly pruned

### OBDDs

In a *decision tree*, we have that each node is either terminal (labelled with $0$ or $1$) or non-terminal (labelled with a variable and with two successors, $low$ and $high$)

Decision trees define recursively boolean functions $f_v(x_1,...,x_n)$ as follows:

1. If $v$ is a terminal node, then $f_v(x_1,...,x_n) = value(v) $
2. Else, considering $var(v) = x_i$, we have that
$$f_v(x_1,...,x_n) = (\neg x_i \land f_{low(x_i)}(x_1,...,x_n)) \lor (x_i \land f_{high(x_i)}(x_1,...,x_n))$$

Our objective is to obtain a graph without isomorphic subtrees $\to$ define the same function, but in a much more compact way

**Building an OBDD from a DT**
3 rules to apply, in order, until a fixpoint is reached

1. Remove duplicate terminal and redisrect all edges to the representative of the terminals
2. Remove duplicate non-terminal nodes: if two non terminal nodes $u$ and $v$ are such that $var(u)=var(v), high(u)=high(v), low(u)=low(v)$, then eliminate $v$ and redirect all incoming edges to $u$
3. Remove redundant tests: if a non-terminal node $v$ is such that $low(v)=high(v)$, then delete $v$ and redirect all of its incoming edges into $low(v)$

The size of the diagram depends on the ordering of the variables, but finding an optimal ordering is an $NP$-complete problem $\to$ generally we use some heuristic

We want to show now how we can implement efficiently boolean operations of OBDDs

RESTRICT: $f|_{x_i \leftarrow b}(x_1,...,x_n) = f(x_1,...,x_{i-1},b,x_{i+1},...,x_n)$

In the OBDD, if we restrict a variable $x_i$ to a value $b$, for each node $w \ \ s.t. \ \ var(w) = x_i$, we replace $w$ by $low(w)$ if $b=0$ and by $high(w)$ else

Note that we can simplify functions through *Shannon expansion*:

$$f = (\neg x \land f|_{x \leftarrow 0}) \lor (x \land f|_{x \leftarrow 1})$$

Similarly, we can APPLY boolean operations $*$ quickly on OBDDs representing functions $f,f'$ using the following rules (considering we want to merge two OBDDs rooted in $v,v'$, whose variables are $x$ and $x'$)

1. $v,v'$ terminal nodes $\to$ $f*f' = v*v'$
2. $x=x' \to f*f' = (\neg x \land (f|_{x \leftarrow 0} * f'|_{x \leftarrow 1}) \lor (x \land (f|_{x \leftarrow 1}) * f'|_{x \leftarrow 1}))$
3. $x<x' \to f*f' = (\neg x \land (f|_{x \leftarrow 0} * f')) \lor (x \land (f|_{x \leftarrow1}) * f')$
4. $x>x' \to$ dual of 3.

This may lead to an exponential blow-up, but we can use an hash table to keep track of previous operations to avoid producing the same result twice. In this way, the number of subproblems becomes bounded by the product of the sizes of the OBDDs for $f$ and $f'$

# Fixpoint characterization of CTL operators

Given a Kripke structure $\mathcal{M} = (S,R,L)$, we have that

- each $n$-ary relation in $r \in R$ can be represented by an arity $n$ characteristic function $f_r$
- we can use $\lceil \log_2 |S| \rceil$ bits to represent $S$
- we can introduce a function for each proposition letter that captures the set of sets where it is true

We define a predicate transformer $\tau$ as a function

$$\tau: 2^S \to 2^S$$

The set of states that satisfy a generic CTL formula will be characterized as the least/greatest fixpoint of a suitable predicate transformer

> $2^S$ can be seen as a lattice closed under $\subseteq$

Note that:

- $true = S$
- $false = \empty$

because of the characteristic function characterization of relations

**DEFINITIONS**

1. $\tau$ is *monotonic* if $P \subseteq Q \implies \tau(P) \subseteq \tau(Q)$
2. $\tau$ is *$\cup$-continuous* if $P_1 \subseteq P_2 \subseteq ... \implies \tau(\bigcup_i P_i) = \bigcup_i \tau(P_i)$
3. $\tau$ is *$\cap$-continuous* if $P_1 \supseteq P_2 \supset ... \implies \tau(\bigcap_i P_i) = \bigcap_i \tau(P_i)$
4. $\tau^i(z) = \tau(\tau(...(z)))$

From Tarsky, we know that if $\tau$ is monotonic, then it must have a *least fixpoint* $\mu z. \tau z$ and a *greatest fixpoint* $\nu z . \tau z$:

- $\mu z . \tau z = \bigcap (z : \tau(z) \subseteq z) \implies$ if $\tau$ monotonic and $\cup$-continuous, then $\mu z . \tau z = \bigcup_i \tau^i (false)$
- $\nu z . \tau z = \bigcup (z : \tau(z) \supseteq z)\implies$ if $\tau$ monotonic and $\cap$-continuous, then $\nu z . \tau z = \bigcap_i \tau^i (true)$

**LEMMAS**

1. $S$ finite and $\tau$ monotonic $\implies \tau$ $\cup$ and $\cap$ continuous
2. $\tau$ monotonic $\implies$, for every $i$, $\tau^i(false) \subseteq \tau^{i+1}(false)$ and $\tau^i(true) \supseteq \tau^{i+1}(true)$
3. $S$ finite, $\tau$ monotonic $\implies$ $\exists i_0, j_0 . \forall i, j \geq i_0, j_0, \tau^i(false) = \tau^{i_0}(false) \land \tau^j(true) = \tau^{j_0}(true)$
4. $S$ finite, $\tau$ monotonic $\implies$ $\exists i,j . \tau^i(false) = \mu z . \tau(z) \land \tau^j(true) = \nu z . \tau z$

We can use a simple fixpoint algorithm to compute the two fixpoints (the complexity is $\mathcal{O}(|S|)$)

Let's consider the following operators:

- $AF f_1 = \mu z . f_1 \lor AX z$
- $EF f_1 = \mu z . f_1 \lor EX z$
- $AG f_1 = \nu z . f_1 \land AX z$
- $EG f_1 = \nu z . f_1 \land EX z$
- $A[ f_1 \ U \ f_2 ] = \mu z . f_2 \lor (f_1 \land AX z)$
- $E[ f_1 \ U \ f_2 ] = \mu z . f_2 \lor (f_1 \land EX z)$

For each of the transformers, we should prove the 4 lemmas above.

Now we can exploit OBDDs and fixpoint characterizations of CTL operators to produce a simple algorithm for *symbolic model checking*

Given a kripke structure $\mathcal{M}=(S,R,L)$ and a formula $\alpha$, the procedure $CHECK$ works (according to the form of $\alpha$) in the following way:

- $\alpha$ propositional letter $\implies$ dealt with the labeling function $L$
- boolean connectives $\implies$ dealt with APPLY
- temporal modalities:
    - $EX f_1 \implies CHECK_{EX}(CHECK(f_1))$
    - $EG f_1 \implies CHECK_{EG}(CHECK(f_1))$
    - $E(f_1 \ U \ f_2) \implies CHECK_{EU}(CHECK(f_1), CHECK(f_1))$ 

where
- $CHECK : CTL \to OBDD$
- $CHECK_{EX} : OBDD \to OBDD$
$$CHECK_{EX}(f(\overline(v))) = \exists \overline(v)' . [R(\overline{v}, \overline{v}') \land f(\overline{v}')]$$
- $CHECK_{EG} : OBDD \to OBDD$: use the greatest fixpoint algorithm with an OBDD for $true$.
- $CHECK_{EU} : OBDD \times OBDD \to OBDD$: use the least fixpoint algorithm with an OBDD for $false$

# Bounded Model Checking

