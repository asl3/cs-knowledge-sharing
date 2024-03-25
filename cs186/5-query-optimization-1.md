# Relational Query Optimization 1: The Plan Space

## Query Optimization

- Magical bridge between declarative domain-specific language (what we want as answer) and custom imperative computer programs (how to computer answer)
- CS186 focuses on System R optimizers
  - Cascades optimizer is another common one

### General flow

- SQL Query -> Query Parser -> Query Rewriter -> Query Optimizer -> Query Executor

  - Catalog Manager helps determine what is legal

### Vocab

- **Query parser**: Checks correctness and authorization, generates parse tree
- **Query rewriter**: Convert queries to canonical form, flatten views, subqueries into fewer query blocks
- **Cost-based query optimizer**: Optimize 1 query block at a time, use catalog stats to find least-cost plan per query block, can always be optimized further
  - Query blocks:
    - Select, Project, Join
    - GroupBy/Agg
    - Order By

## Query Optimization Components

1. Plan space
2. Cost estimation
3. Search strategy

Goal of query optimization: find plan with least actual cost

- (reality: find plan with least estimated cost)
