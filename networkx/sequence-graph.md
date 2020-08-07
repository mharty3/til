# Visualising sequences as a directed graph

```python
plt.figure(1, figsize=(15, 10))

G = nx.MultiDiGraph(iter(network.items()))

nx.draw(G, arrows=True, with_labels=True, node_color='white', pos=nx.nx_agraph.graphviz_layout(G))
```
