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