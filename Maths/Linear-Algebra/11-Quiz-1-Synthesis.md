---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [13]
prereq_clusters: ["08", "09", "10"]
status: complete
source: manual
---

# 11 — Quiz 1 Synthesis

## Concept Statement
Cross-exam the first 12 lectures as a single system. Recognise that linear independence, rank, full-rank, and nullspace triviality are all the *same* property wearing different hats.

## Lecture Sources
- Strang MIT 18.06, Lecture 13: *Quiz 1 Review*

## Core Material

### Key Cross-Lecture Equivalences

| If you show… | …you also have… |
|--------------|------------------|
| $A\mathbf{x} = \mathbf{0}$ has only $\mathbf{x} = \mathbf{0}$ | columns of $A$ linearly independent; rank $= n$; $A^T A$ invertible |
| $A\mathbf{x} = \mathbf{b}$ has solution for every $\mathbf{b}$ | rank $= m$; $A$ has $m$ pivots; $C(A) = \mathbb{R}^m$ |
| $\mathbf{x}_1, \dots, \mathbf{x}_k$ are linearly independent | the matrix formed by them has rank $k$ |

### Strategy: Find Dimension via Components

When asked for the dimension of a constructed space (intersection of two subspaces, span of a set of matrices), the universal answer is:
> Reduce to "count the linearly independent pieces."

### The Exam-Ready Sub-Summary

- *Eliminate* to $R$, count pivots → rank.
- *Read* $R$'s free-column pattern → nullspace dimension.
- *Check* $\mathbf{b}$ against $C(A)$ for solvability.
- *Realise*: invertible ↔ full rank ↔ trivial nullspace ↔ span = entire space.

## Cross-Cluster Links
- **Prereq**: [[08-Four-Fundamental-Subspaces]], [[09-Matrix-Spaces-and-Rank1]], [[10-Graphs-Networks-Incidence]]
- **Forward**: [[12-Orthogonal-Vectors-Subspaces]] (next topic after the boundary)

## Thematic Summary
Lecture 13 collapses the first 12 lectures into an exam-ready cheat sheet. The unifying trick: every rank/nullspace/independence question is equivalent to "how many pivots does $R$ have?" — once you're fluent in counting pivots, the whole first half answers itself.

## Glossary

*(No new terms; this is a synthesis node. See upstream clusters for vocabulary.)*
