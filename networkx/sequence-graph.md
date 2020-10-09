# Visualising sequences as directed graphs

NetworkX is a Python library for working with graphs.
It's great for constructing and analysing networks, but can strugle to render some graphs
without a little help.

As an example, let's take the [Collatz conjecture](https://en.wikipedia.org/wiki/Collatz_conjecture),
a sequence defined by:

```
positive integer n
repeat:
    if even: half it
    if odd: 3n + 1
```

As Wikipedia says:

> The conjecture is that no matter what value of n, the sequence will always reach 1.

Using a Jupyter notebook, let's fill out and display the sequence for all numbers up to 20:

```python
# pip install networkx matplotlib scipy
import networkx as nx
import pylab as plt

def collatz(n, network):
    if n in network:
        return

    if n % 2 == 0:
        m = n // 2
    else:
        m = 3 * n + 1

    network[n] = m

    collatz(m, network)

network = {}
for n in range(1, 20):
    collatz(n, network)

G = nx.DiGraph(list(network.items()))


method_names = [
    "draw",
    "draw_circular",
    "draw_kamada_kawai",
    "draw_random",
    "draw_spectral",
    "draw_spring",
    "draw_planar",
    "draw_shell",
]
for method_name in method_names:
    plt.figure()
    plt.title(method_name)

    method = getattr(nx, method_name)
    method(G, with_labels=True, node_color='white')
```

We can see the built-in NetworkX layout engines all make the result very hard to inspect:

![NetworkX built-in graph layout engines](https://raw.githubusercontent.com/tomviner/til/master/networkx/nx-graphs.jpg)

The trick here, as
[mentioned in the tutorial](https://networkx.github.io/documentation/stable/tutorial.html#drawing-graphs),
is to use the 3rd party Graphviz library as the
layout engine, like this:


```python
plt.figure(figsize=(15, 10))
plt.title('graphviz_layout')

# pip install pygraphviz
nx.draw(
    G,
    with_labels=True,
    node_color='white',
    pos=nx.nx_agraph.graphviz_layout(G),
)
```

![Graphviz layout engine](https://raw.githubusercontent.com/tomviner/til/master/networkx/graphviz-graph.png)
