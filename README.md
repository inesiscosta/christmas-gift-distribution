# Christmas Gift Distribution

Santa has toy factories around the world, each with limited stock, and every country has export limits and a minimum number of gifts it must receive. Children can ask for several toys but get at most one. How many children can be made happy this Christmas?

This program answers that with integer linear programming, using [PuLP](https://coin-or.github.io/pulp/) and the GLPK solver. Built with Miguel Raposo for the Algorithm Analysis and Synthesis course at Instituto Superior Técnico.

## The Model

**Variables.** One binary variable for every (child, factory) request. It's `1` if that child gets their toy from that factory.

**Objective.** Maximize the number of requests fulfilled, which is the number of happy children.

**Constraints:**

| Rule | Constraint |
|---|---|
| Each child gets at most one gift | for each child, the sum of their request variables ≤ 1 |
| Factories can't exceed their stock | for each factory, the sum of requests it fulfils ≤ its stock |
| Countries have export limits | for each country, gifts sent from its factories to children abroad ≤ its export limit |
| Countries have a minimum | for each country, gifts received by its children ≥ its minimum |

Factories with no stock and requests to unknown factories are dropped before the model is built, which keeps it smaller. If no assignment satisfies every minimum, the program prints `-1`.

## Input

```
n m t                                    factories, countries, children
i  j  f_max_i                    × n     factory i is in country j and has f_max_i toys
j  p_max_j  p_min_j              × m     country j exports at most p_max_j and must receive at least p_min_j
k  j  f1 f2 …                    × t     child k lives in country j and asks for toys from factories f1, f2, …
```

### Example

```console
$ printf '3 2 3\n1 1 1\n2 1 1\n3 2 1\n1 2 1\n2 2 1\n1 1 2 3\n2 1 2 1\n3 2 1\n' | python src/main.py
3
```

Three factories with one toy each, across two countries, and three children. All three children can get a toy.

## Running

Needs Python 3, PuLP and GLPK:

```sh
pip install pulp
sudo apt-get install glpk-utils   # or: brew install glpk

python src/main.py < input.txt
```

## Tests

```sh
pip install pytest
python -m pytest src/tests/test_public.py
```

`src/tests/test_generator.py` generates larger random instances.

## License

[MIT](LICENSE) © 2024 Inês Costa and Miguel Raposo
