CategoricalData.jl
==================

Tools for working with categorical variables, including ordered categories
(AKA ordinal variables). This package is intentionally designed to provide
low-level machinery for working with categorical data. Most of API would not
be of interest to end-users, only those working on higher-level projects like
DataArrays.jl.

# Examples

Suppose that you want to work with three categories. To represent information
about those categories, you first build a pool that enumerates the possible
categories:

```
using CategoricalData

pool = CategoricalPool(["Group A", "Group B", "Group C"])
```

To create a specific observation, you create a `CategoricalVariable` object
that points to the pool's internal index of values:

```
cv = CategoricalVariable(1, pool)
```

If you know that you're working with ordinal data, you can use an `OrdinalPool`
object, which augments a `CategoricalPool` with an ordering:

```
opool = OrdinalPool(
    ["Group A", "Group B", "Group C"],
    ["Group B", "Group C", "Group A"]
)
```

In this example, the first argument to `OrdinalPool` specifies the possible
levels that this ordered factor can take on. The second argument provides
the levels of the factors in their sorted order.

Once an `OrdinalPool` exists, you can define `OrdinalVariable` objects and
compare their position in the order:

```
ov1 = OrdinalVariable(1, opool)
ov2 = OrdinalVariable(2, opool)

ov1 < ov2
ov1 > ov2
```

# Full API

As shown above, there are constructors for:

* `CategoricalPool`
* `CategoricalVariable`
* `OrdinalPool`
* `OrdinalVariable`

There are also methods for accessing and manipulating the pool:

* `levels`: Determine which levels a pool allows
* `levels!`: Reset, en masse, the levels that a pool allows
* `add!`: Add one or more levels to the pool
* `delete!`: Delete one or more levels from the pool
* `order`: Determine the order of an ordinal pool
* `order!`: Reset, en masse, the ordering of an ordinal pool

Note that `add!` and `delete!` do not work on ordinal variables because they
not provide any mechanism for specifying how the changes to the levels affect
the ordering of the pool.
