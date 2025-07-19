# Consolidated Source Index

This document combines multiple source files. Below is the index of the included files.

- [doc\examples_sphinx-gallery\README.rst](#doc-examples_sphinx-gallery-readme-rst)
- [doc\examples_sphinx-gallery\articulation_points.py](#doc-examples_sphinx-gallery-articulation_points-py)
- [doc\examples_sphinx-gallery\betweenness.py](#doc-examples_sphinx-gallery-betweenness-py)
- [doc\examples_sphinx-gallery\bipartite_matching.py](#doc-examples_sphinx-gallery-bipartite_matching-py)
- [doc\examples_sphinx-gallery\bipartite_matching_maxflow.py](#doc-examples_sphinx-gallery-bipartite_matching_maxflow-py)
- [doc\examples_sphinx-gallery\bridges.py](#doc-examples_sphinx-gallery-bridges-py)
- [doc\examples_sphinx-gallery\cluster_contraction.py](#doc-examples_sphinx-gallery-cluster_contraction-py)
- [doc\examples_sphinx-gallery\complement.py](#doc-examples_sphinx-gallery-complement-py)
- [doc\examples_sphinx-gallery\configuration.py](#doc-examples_sphinx-gallery-configuration-py)
- [doc\examples_sphinx-gallery\connected_components.py](#doc-examples_sphinx-gallery-connected_components-py)
- [doc\examples_sphinx-gallery\delaunay-triangulation.py](#doc-examples_sphinx-gallery-delaunay-triangulation-py)
- [doc\examples_sphinx-gallery\erdos_renyi.py](#doc-examples_sphinx-gallery-erdos_renyi-py)
- [doc\examples_sphinx-gallery\generate_dag.py](#doc-examples_sphinx-gallery-generate_dag-py)
- [doc\examples_sphinx-gallery\isomorphism.py](#doc-examples_sphinx-gallery-isomorphism-py)
- [doc\examples_sphinx-gallery\lesmis\lesmis.txt](#doc-examples_sphinx-gallery-lesmis-lesmis-txt)
- [doc\examples_sphinx-gallery\maxflow.py](#doc-examples_sphinx-gallery-maxflow-py)
- [doc\examples_sphinx-gallery\minimum_spanning_trees.py](#doc-examples_sphinx-gallery-minimum_spanning_trees-py)
- [doc\examples_sphinx-gallery\online_user_actions.py](#doc-examples_sphinx-gallery-online_user_actions-py)
- [doc\examples_sphinx-gallery\personalized_pagerank.py](#doc-examples_sphinx-gallery-personalized_pagerank-py)
- [doc\examples_sphinx-gallery\quickstart.py](#doc-examples_sphinx-gallery-quickstart-py)
- [doc\examples_sphinx-gallery\ring_animation.py](#doc-examples_sphinx-gallery-ring_animation-py)
- [doc\examples_sphinx-gallery\shortest_path_visualisation.py](#doc-examples_sphinx-gallery-shortest_path_visualisation-py)
- [doc\examples_sphinx-gallery\simplify.py](#doc-examples_sphinx-gallery-simplify-py)
- [doc\examples_sphinx-gallery\spanning_trees.py](#doc-examples_sphinx-gallery-spanning_trees-py)
- [doc\examples_sphinx-gallery\topological_sort.py](#doc-examples_sphinx-gallery-topological_sort-py)
- [doc\examples_sphinx-gallery\visual_style.py](#doc-examples_sphinx-gallery-visual_style-py)
- [doc\examples_sphinx-gallery\visualize_cliques.py](#doc-examples_sphinx-gallery-visualize_cliques-py)
- [doc\examples_sphinx-gallery\visualize_communities.py](#doc-examples_sphinx-gallery-visualize_communities-py)
- [doc\source\analysis.rst](#doc-source-analysis-rst)
- [doc\source\api\index.rst](#doc-source-api-index-rst)
- [doc\source\conf.py](#doc-source-conf-py)
- [doc\source\configuration.rst](#doc-source-configuration-rst)
- [doc\source\faq.rst](#doc-source-faq-rst)
- [doc\source\generation.rst](#doc-source-generation-rst)
- [doc\source\include\global.rst](#doc-source-include-global-rst)
- [doc\source\index.rst](#doc-source-index-rst)
- [doc\source\install.rst](#doc-source-install-rst)
- [doc\source\requirements.txt](#doc-source-requirements-txt)
- [doc\source\tutorial.es.rst](#doc-source-tutorial-es-rst)
- [doc\source\tutorial.rst](#doc-source-tutorial-rst)
- [doc\source\visualisation.rst](#doc-source-visualisation-rst)


---

## <a name="doc-examples_sphinx-gallery-readme-rst"></a>File: doc\examples_sphinx-gallery\README.rst

.. _gallery-of-examples:

Gallery of Examples
===================

Gallery of examples for `python-igraph` illustrating graph generation, analysis, and plotting.

Impatient? Check out the :ref:`tutorials-quickstart`.

Too little detail? Read the :doc:`extended tutorial <../tutorial>`.


---

## <a name="doc-examples_sphinx-gallery-articulation_points-py"></a>File: doc\examples_sphinx-gallery\articulation_points.py

```python
"""
.. _tutorials-articulation-points:

===================
Articulation Points
===================

This example shows how to compute and visualize the `articulation points <https://en.wikipedia.org/wiki/Biconnected_component>`_ in a graph using :meth:`igraph.GraphBase.articulation_points`. For an example on bridges instead, see :ref:`tutorials-bridges`.

"""
import igraph as ig
import matplotlib.pyplot as plt

# %%
# First, we construct a graph. This example shows usage of graph formulae:
g = ig.Graph.Formula(
    "0-1-2-0, 3:4:5:6 - 3:4:5:6, 2-3-7-8",
)

# %%
# Now we are aready to find the articulation points as a vertex sequence
articulation_points = g.vs[g.articulation_points()]

# %%
# Finally, we can plot the graph
fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    vertex_size=30,
    vertex_color="lightblue",
    vertex_label=range(g.vcount()),
    vertex_frame_color = ["red" if v in articulation_points else "black" for v in g.vs],
    vertex_frame_width = [3 if v in articulation_points else 1 for v in g.vs],
    edge_width=0.8,
    edge_color='gray'
)
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-betweenness-py"></a>File: doc\examples_sphinx-gallery\betweenness.py

```python
"""
.. _tutorials-betweenness:

=======================
Betweenness
=======================

This example demonstrates how to visualize both vertex and edge betweenness with a custom defined color palette. We use the methods :meth:`igraph.GraphBase.betweenness` and :meth:`igraph.GraphBase.edge_betweenness` respectively, and demonstrate the effects on a standard `Krackhardt Kite <https://www.wikiwand.com/en/Krackhardt_kite_graph>`_ graph, as well as a `Watts-Strogatz <https://en.wikipedia.org/wiki/Watts%E2%80%93Strogatz_model>`_ random graph.

"""
import random
import matplotlib.pyplot as plt
from matplotlib.cm import ScalarMappable
from matplotlib.colors import LinearSegmentedColormap, Normalize
import igraph as ig


# %%
# We define a function that plots the graph  on a Matplotlib axis, along with
# its vertex and edge betweenness values. The function also generates some
# color bars on the sides to see how they translate to each other. We use
# `Matplotlib's Normalize class <https://matplotlib.org/stable/api/_as_gen/matplotlib.colors.Normalize.html>`_
# to ensure that our color bar ranges are correct, as well as *igraph*'s
# :meth:`igraph.utils.rescale` to rescale the betweennesses in the interval
# ``[0, 1]``.
def plot_betweenness(g, vertex_betweenness, edge_betweenness, ax, cax1, cax2):
    '''Plot vertex/edge betweenness, with colorbars

    Args:
        g: the graph to plot.
        ax: the Axes for the graph
        cax1: the Axes for the vertex betweenness colorbar
        cax2: the Axes for the edge betweenness colorbar
    '''

    # Rescale betweenness to be between 0.0 and 1.0
    scaled_vertex_betweenness = ig.rescale(vertex_betweenness, clamp=True)
    scaled_edge_betweenness = ig.rescale(edge_betweenness, clamp=True)
    print(f"vertices: {min(vertex_betweenness)} - {max(vertex_betweenness)}")
    print(f"edges: {min(edge_betweenness)} - {max(edge_betweenness)}")

    # Define mappings betweenness -> color
    cmap1 = LinearSegmentedColormap.from_list("vertex_cmap", ["pink", "indigo"])
    cmap2 = LinearSegmentedColormap.from_list("edge_cmap", ["lightblue", "midnightblue"])

    # Plot graph
    g.vs["color"] = [cmap1(betweenness) for betweenness in scaled_vertex_betweenness]
    g.vs["size"]  = ig.rescale(vertex_betweenness, (10, 50))
    g.es["color"] = [cmap2(betweenness) for betweenness in scaled_edge_betweenness]
    g.es["width"] = ig.rescale(edge_betweenness, (0.5, 1.0))
    ig.plot(
        g,
        target=ax,
        layout="fruchterman_reingold",
        vertex_frame_width=0.2,
    )

    # Color bars
    norm1 = ScalarMappable(norm=Normalize(0, max(vertex_betweenness)), cmap=cmap1)
    norm2 = ScalarMappable(norm=Normalize(0, max(edge_betweenness)), cmap=cmap2)
    plt.colorbar(norm1, cax=cax1, orientation="horizontal", label='Vertex Betweenness')
    plt.colorbar(norm2, cax=cax2, orientation="horizontal", label='Edge Betweenness')


# %%
# First, generate a graph, e.g. the Krackhardt Kite Graph:
random.seed(0)
g1 = ig.Graph.Famous("Krackhardt_Kite")

# %%
# Then we can compute vertex and edge betweenness:
vertex_betweenness1 = g1.betweenness()
edge_betweenness1 = g1.edge_betweenness()

# %% As a second example, we generate and analyze a Watts Strogatz graph:
g2 = ig.Graph.Watts_Strogatz(dim=1, size=150, nei=2, p=0.1)
vertex_betweenness2 = g2.betweenness()
edge_betweenness2 = g2.edge_betweenness()

# %%
# Finally, we plot the two graphs, each with two colorbars for vertex/edge
# betweenness
fig, axs = plt.subplots(
    3, 2,
    figsize=(7, 6),
    gridspec_kw={"height_ratios": (20, 1, 1)},
)
plot_betweenness(g1, vertex_betweenness1, edge_betweenness1, *axs[:, 0])
plot_betweenness(g2, vertex_betweenness2, edge_betweenness2, *axs[:, 1])
fig.tight_layout(h_pad=1)
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-bipartite_matching-py"></a>File: doc\examples_sphinx-gallery\bipartite_matching.py

```python
"""
.. _tutorials-bipartite-matching:

==========================
Maximum Bipartite Matching
==========================

This example demonstrates an efficient way to find and visualise a maximum biparite matching using :meth:`igraph.Graph.maximum_bipartite_matching`.
"""
import igraph as ig
import matplotlib.pyplot as plt

# %%
# First, we construct a bipartite graph, assigning:
#  - nodes 0-4 to one side
#  - nodes 5-8 to the other side
g = ig.Graph.Bipartite(
    [0, 0, 0, 0, 0, 1, 1, 1, 1],
    [(0, 5), (1, 6), (1, 7), (2, 5), (2, 8), (3, 6), (4, 5), (4, 6)]
)

# %%
# We can easily check that the graph is indeed bipartite:
assert g.is_bipartite()

# %%
# Now can can compute the maximum bipartite matching:
matching = g.maximum_bipartite_matching()

# %%
# It's easy to print matching pairs of vertices
matching_size = 0
print("Matching is:")
for i in range(5):
    print(f"{i} - {matching.match_of(i)}")
    if matching.is_matched(i):
        matching_size += 1
print("Size of maximum matching is:", matching_size)

# %%
# Finally, we can plot the bipartite graph, highlighting the edges connecting
# maximal matches by a red color:
fig, ax = plt.subplots(figsize=(7, 3))
ig.plot(
    g,
    target=ax,
    layout=g.layout_bipartite(),
    vertex_size=30,
    vertex_label=range(g.vcount()),
    vertex_color="lightblue",
    edge_width=[3 if e.target == matching.match_of(e.source) else 1.0 for e in g.es],
    edge_color=["red" if e.target == matching.match_of(e.source) else "black" for e in g.es]
)

```

---

## <a name="doc-examples_sphinx-gallery-bipartite_matching_maxflow-py"></a>File: doc\examples_sphinx-gallery\bipartite_matching_maxflow.py

```python
"""
.. _tutorials-bipartite-matching-maxflow:

==========================================
Maximum Bipartite Matching by Maximum Flow
==========================================

This example presents how to visualise bipartite matching using maximum flow (see :meth:`igraph.Graph.maxflow`).

.. note::  :meth:`igraph.Graph.maximum_bipartite_matching` is usually a better way to find the maximum bipartite matching. For a demonstration on how to use that method instead, check out :ref:`tutorials-bipartite-matching`.
"""
import igraph as ig
import matplotlib.pyplot as plt

# %%
# We start by creating the bipartite directed graph.
g = ig.Graph(
    9,
    [(0, 4), (0, 5), (1, 4), (1, 6), (1, 7), (2, 5), (2, 7), (2, 8), (3, 6), (3, 7)],
    directed=True
)

# %%
# We assign:
#  - nodes 0-3 to one side
#  - nodes 4-8 to the other side
g.vs[range(4)]["type"] = True
g.vs[range(4, 9)]["type"] = False

# %%
# Then we add a source (vertex 9) and a sink (vertex 10)
g.add_vertices(2)
g.add_edges([(9, 0), (9, 1), (9, 2), (9, 3)])  # connect source to one side
g.add_edges([(4, 10), (5, 10), (6, 10), (7, 10), (8, 10)])  # ... and sinks to the other

flow = g.maxflow(9, 10)
print("Size of maximum matching (maxflow) is:", flow.value)

# %%
# Let's compare the output against :meth:`igraph.Graph.maximum_bipartite_matching`:

# delete the source and sink, which are unneeded for this function.
g2 = g.copy()
g2.delete_vertices([9, 10])
matching = g2.maximum_bipartite_matching()
matching_size = sum(1 for i in range(4) if matching.is_matched(i))
print("Size of maximum matching (maximum_bipartite_matching) is:", matching_size)

# %%
# Last, we can display the original flow graph nicely with the matchings added.
# To achieve a pleasant visual effect, we set the positions of source and sink
# manually:
layout = g.layout_bipartite()
layout[9] = (2, -1)
layout[10] = (2, 2)

fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    layout=layout,
    vertex_size=30,
    vertex_label=range(g.vcount()),
    vertex_color=["lightblue" if i < 9 else "orange" for i in range(11)],
    edge_width=[1.0 + flow.flow[i] for i in range(g.ecount())]
)
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-bridges-py"></a>File: doc\examples_sphinx-gallery\bridges.py

```python
"""
.. _tutorials-bridges:

========
Bridges
========

This example shows how to compute and visualize the `bridges <https://en.wikipedia.org/wiki/Bridge_(graph_theory)>`_ in a graph using :meth:`igraph.GraphBase.bridges`. For an example on articulation points instead, see :ref:`tutorials-articulation-points`.
"""
import igraph as ig
import matplotlib.pyplot as plt

# %%
# Let's start with a simple example. We begin by constructing a graph that
# includes a few bridges:
g = ig.Graph(14, [(0, 1), (1, 2), (2, 3), (0, 3), (0, 2), (1, 3), (3, 4),
        (4, 5), (5, 6), (6, 4), (6, 7), (7, 8), (7, 9), (9, 10), (10 ,11),
        (11 ,7), (7, 10), (8, 9), (8, 10), (5, 12), (12, 13)])

# %%
# Then we can use a function to actually find the bridges, i.e. the edges that
# connect different parts of the graph:
bridges = g.bridges()

# %%
# We set a separate color for those edges, to emphasize then in a plot:
g.es["color"] = "gray"
g.es[bridges]["color"] = "red"
g.es["width"] = 0.8
g.es[bridges]["width"] = 1.2

# %%
# Finally, we plot the graph using that emphasis:
fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    vertex_size=30,
    vertex_color="lightblue",
    vertex_label=range(g.vcount())
)
plt.show()

# %%
# Advanced: Cutting Effect
# --------------------------
# Bridges are edges that when removed, will separate the graph into more components then they started with. To emphasise the removal of edges from the graph, we can add small "x" effect to each of the bridges by using edge labels.

# %%
# As before, we begin by constructing the graph:
g = ig.Graph(14, [(0, 1), (1, 2), (2, 3), (0, 3), (0, 2), (1, 3), (3, 4),
        (4, 5), (5, 6), (6, 4), (6, 7), (7, 8), (7, 9), (9, 10), (10 ,11),
        (11 ,7), (7, 10), (8, 9), (8, 10), (5, 12), (12, 13)])

# %%
# We then find and set the color for the bridges, but this time we also set a
# label for those edges:
bridges = g.bridges()
g.es["color"] = "gray"
g.es[bridges]["color"] = "red"
g.es["width"] = 0.8
g.es[bridges]["width"] = 1.2
g.es["label"] = ""
g.es[bridges]["label"] = "x"

# %%
# Finally, we can plot the graph:
fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    vertex_size=30,
    vertex_color="lightblue",
    vertex_label=range(g.vcount()),
    edge_background="#FFF0",    # transparent background color
    edge_align_label=True,      # make sure labels are aligned with the edge
    edge_label=g.es["label"],
    edge_label_color="red"
)
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-cluster_contraction-py"></a>File: doc\examples_sphinx-gallery\cluster_contraction.py

```python
"""
.. _tutorials-cluster-graph:

===========================
Generating Cluster Graphs
===========================

This example shows how to find the communities in a graph, then contract each community into a single node using :class:`igraph.clustering.VertexClustering`. For this tutorial, we'll use the *Donald Knuth's Les Miserables Network*, which shows the coapperances of characters in the novel *Les Miserables*.
"""
import igraph as ig
import matplotlib.pyplot as plt

# %%
# We begin by load the graph from file. The file containing this network can be
# downloaded `here <https://www-personal.umich.edu/~mejn/netdata/>`_.
g = ig.load("./lesmis/lesmis.gml")

# %%
# Now that we have a graph in memory, we can generate communities using
# :meth:`igraph.Graph.community_edge_betweenness` to separate out vertices into
# clusters. (For a more focused tutorial on just visualising communities, check
# out :ref:`tutorials-visualize-communities`).
communities = g.community_edge_betweenness()

# %%
# For plots, it is convenient to convert the communities into a VertexClustering:
communities = communities.as_clustering()

# %%
# We can also easily print out who belongs to each community:
for i, community in enumerate(communities):
    print(f"Community {i}:")
    for v in community:
        print(f"\t{g.vs[v]['label']}")

# %%
# Finally we can proceed to plotting the graph. In order to make each community
# stand out, we set "community colors" using an igraph palette:
num_communities = len(communities)
palette1 = ig.RainbowPalette(n=num_communities)
for i, community in enumerate(communities):
    g.vs[community]["color"] = i
    community_edges = g.es.select(_within=community)
    community_edges["color"] = i

# %%
# We can use a dirty hack to move the labels below the vertices ;-)
g.vs["label"] = ["\n\n" + label for label in g.vs["label"]]

# %%
# Finally, we can plot the communities:
fig1, ax1 = plt.subplots()
ig.plot(
    communities,
    target=ax1,
    mark_groups=True,
    palette=palette1,
    vertex_size=15,
    edge_width=0.5,
)
fig1.set_size_inches(20, 20)


# %%
# Now let's try and contract the information down, using only a single vertex
# to represent each community. We start by defining x, y, and size attributes
# for each node in the original graph:
layout = g.layout_kamada_kawai()
g.vs["x"], g.vs["y"] = list(zip(*layout))
g.vs["size"] = 15
g.es["size"] = 15

# %%
# Then we can generate the cluster graph that compresses each community into a
# single, "composite" vertex using
# :meth:`igraph.VertexClustering.cluster_graph`:
cluster_graph = communities.cluster_graph(
    combine_vertices={
        "x": "mean",
        "y": "mean",
        "color": "first",
        "size": "sum",
    },
    combine_edges={
        "size": "sum",
    },
)

# %%
# .. note::
#
#      We took the mean of x and y values so that the nodes in the cluster
#      graph are placed at the centroid of the original cluster.
#
# .. note::
#
#     ``mean``, ``first``, and ``sum`` are all built-in collapsing functions,
#     along with ``prod``, ``median``, ``max``, ``min``, ``last``, ``random``.
#     You can also define your own custom collapsing functions, which take in a
#     list and return a single element representing the combined attribute
#     value. For more details on igraph contraction, see
#     :meth:`igraph.GraphBase.contract_vertices`.


# %%
# Finally, we can assign colors to the clusters and plot the cluster graph,
# including a legend to make things clear:
palette2 = ig.GradientPalette("gainsboro", "black")
g.es["color"] = [palette2.get(int(i)) for i in ig.rescale(cluster_graph.es["size"], (0, 255), clamp=True)]

fig2, ax2 = plt.subplots()
ig.plot(
    cluster_graph,
    target=ax2,
    palette=palette1,
    # set a minimum size on vertex_size, otherwise vertices are too small
    vertex_size=[max(20, size) for size in cluster_graph.vs["size"]],
    edge_color=g.es["color"],
    edge_width=0.8,
)

# Add a legend
legend_handles = []
for i in range(num_communities):
    handle = ax2.scatter(
        [], [],
        s=100,
        facecolor=palette1.get(i),
        edgecolor="k",
        label=i,
    )
    legend_handles.append(handle)

ax2.legend(
    handles=legend_handles,
    title='Community:',
    bbox_to_anchor=(0, 1.0),
    bbox_transform=ax2.transAxes,
)

fig2.set_size_inches(10, 10)

```

---

## <a name="doc-examples_sphinx-gallery-complement-py"></a>File: doc\examples_sphinx-gallery\complement.py

```python
"""
.. _tutorials-complement:

================
Complement
================

This example shows how to generate the `complement graph <https://en.wikipedia.org/wiki/Complement_graph>`_ of a graph (sometimes known as the anti-graph) using :meth:`igraph.GraphBase.complementer`.
"""
import igraph as ig
import matplotlib.pyplot as plt
import random

# %%
# First, we generate a random graph
random.seed(0)
g1 = ig.Graph.Erdos_Renyi(n=10, p=0.5)

# %%
# .. note::
#     We set the random seed to ensure the graph comes out exactly the same
#     each time in the gallery. You don't need to do that if you're exploring
#     really random graphs ;-)

# %%
# Then we generate the complement graph:
g2 = g1.complementer(loops=False)

# %%
# The union graph of the two is of course the full graph, i.e. a graph with
# edges connecting all vertices to all other vertices. Because we decided to
# ignore loops (aka self-edges) in the complementer, the full graph does not
# include loops either.
g_full = g1 | g2

# %%
# In case there was any doubt, the complement of the full graph is an
# empty graph, with the same vertices but no edges:
g_empty = g_full.complementer(loops=False)

# %%
# To demonstrate these concepts more clearly, here's a layout of each of the
# four graphs we discussed (input, complement, union/full, complement of
# union/empty):
fig, axs = plt.subplots(2, 2)
ig.plot(
    g1,
    target=axs[0, 0],
    layout="circle",
    vertex_color="black",
)
axs[0, 0].set_title('Original graph')
ig.plot(
    g2,
    target=axs[0, 1],
    layout="circle",
    vertex_color="black",
)
axs[0, 1].set_title('Complement graph')

ig.plot(
    g_full,
    target=axs[1, 0],
    layout="circle",
    vertex_color="black",
)
axs[1, 0].set_title('Union graph')
ig.plot(
    g_empty,
    target=axs[1, 1],
    layout="circle",
    vertex_color="black",
)
axs[1, 1].set_title('Complement of union graph')
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-configuration-py"></a>File: doc\examples_sphinx-gallery\configuration.py

```python
"""
.. _tutorials-configuration:

======================
Configuration Instance
======================

This example shows how to use igraph's :class:`configuration instance <igraph.Configuration>` to set default igraph settings. This is useful for setting global settings so that they don't need to be explicitly stated at the beginning of every igraph project you work on.
"""
import igraph as ig
import matplotlib.pyplot as plt
import random

# %%
# First we define the default plotting backend, layout, and color palette.
ig.config["plotting.backend"] = "matplotlib"
ig.config["plotting.layout"] = "fruchterman_reingold"
ig.config["plotting.palette"] = "rainbow"

# %%
# Then, we save them. By default, ``ig.config.save()`` will save files to
# ``~/.igraphrc`` on Linux and Max OS X systems, or in
# ``%USERPROFILE%\.igraphrc`` for Windows systems:
ig.config.save()

# %%
# The code above only needs to be run once (to store the new config options
# into the ``.igraphrc`` file). Whenever you use igraph and this file exists,
# igraph will read its content and use those options as defaults. For
# example, let's create and plot a new graph to demonstrate:
random.seed(1)
g = ig.Graph.Barabasi(n=100, m=1)

# %%
# We now calculate a color value between 0-200 for all nodes, for instance by
# computing the vertex betweenness:
betweenness = g.betweenness()
colors = [int(i * 200 / max(betweenness)) for i in betweenness]

# %%
# Finally, we can plot the graph. You will notice that even though we did not
# create a dedicated figure and axes, matplotlib is now used by default:
ig.plot(g, vertex_color=colors, vertex_size=15, edge_width=0.3)
plt.show()

# %%
# The full list of config settings can be found at
# :class:`igraph.Configuration`.
#
# .. note::
#
#    You can have multiple config files: specify each location via
#    ``ig.config.save("./path/to/config/file")``. To load a specific config,
#    import igraph and then call ``ig.config.load("./path/to/config/file")``
#
#
# .. note::
#
#     To use a consistent style between individual plots (e.g. vertex sizes,
#     colors, layout etc.) check out :ref:`tutorials-visual-style`.

```

---

## <a name="doc-examples_sphinx-gallery-connected_components-py"></a>File: doc\examples_sphinx-gallery\connected_components.py

```python
"""
.. _tutorials-connected-components:

=====================
Connected Components
=====================

This example demonstrates how to visualise the connected components in a graph using :meth:`igraph.GraphBase.connected_components`.
"""
import igraph as ig
import matplotlib.pyplot as plt
import random

# %%
# First, we generate a randomized geometric graph with random vertex sizes. The
# seed is set to the example is reproducible in our manual: you don't really
# need it to understand the concepts.
random.seed(0)
g = ig.Graph.GRG(50, 0.15)

# %%
# Now we can cluster the graph into weakly connected components, i.e. subgraphs
# that have no edges connecting them to one another:
components = g.connected_components(mode='weak')

# %%
# Finally, we can visualize the distinct connected components of the graph:
fig, ax = plt.subplots()
ig.plot(
    components,
    target=ax,
    palette=ig.RainbowPalette(),
    vertex_size=7,
    vertex_color=list(map(int, ig.rescale(components.membership, (0, 200), clamp=True))),
    edge_width=0.7
)
plt.show()

# %%
# .. note::
#
#     We use the integers from 0 to 200 instead of 0 to 255 in our vertex
#     colors, since 255 in the :class:`igraph.drawing.colors.RainbowPalette`
#     corresponds to looping back to red. This gives us nicely distinct hues.

```

---

## <a name="doc-examples_sphinx-gallery-delaunay-triangulation-py"></a>File: doc\examples_sphinx-gallery\delaunay-triangulation.py

```python
"""
.. _tutorials-delaunay-triangulation:

======================
Delaunay Triangulation
======================

This example demonstrates how to calculate the `Delaunay triangulation <https://en.wikipedia.org/wiki/Delaunay_triangulation>`_ of an input graph. We start by generating a set of points on a 2D grid using random ``numpy`` arrays and a graph with those vertex coordinates and no edges.

"""
import numpy as np
from scipy.spatial import Delaunay
import igraph as ig
import matplotlib.pyplot as plt


# %%
# We start by generating a random graph in the 2D unit cube, fixing the random
# seed to ensure reproducibility.
np.random.seed(0)
x, y = np.random.rand(2, 30)
g = ig.Graph(30)
g.vs['x'] = x
g.vs['y'] = y

# %%
# Because we already set the `x` and `y` vertex attributes, we can use
# :meth:`igraph.Graph.layout_auto` to wrap them into a :class:`igraph.Layout`
# object.
layout = g.layout_auto()

# %%
# Now we can calculate the delaunay triangulation using `scipy`'s :class:`scipy:scipy.spatial.Delaunay` class:
delaunay = Delaunay(layout.coords)

# %%
# Given the triangulation, we can add the edges into the original graph:
for tri in delaunay.simplices:
    g.add_edges([
        (tri[0], tri[1]),
        (tri[1], tri[2]),
        (tri[0], tri[2]),
    ])

# %%
# Because adjacent triangles share an edge/side, the graph now contains some
# edges more than once. It's useful to simplify the graph to get rid of those
# multiedges, keeping each side only once:
g.simplify()

# %%
# Finally, plotting the graph gives a good idea of what the triangulation looks
# like:
fig, ax = plt.subplots()
ig.plot(
    g,
    layout=layout,
    target=ax,
    vertex_size=4,
    vertex_color="lightblue",
    edge_width=0.8
)
plt.show()

# %%
# Alternative plotting style
# --------------------------
# We can use :doc:`matplotlib <matplotlib:index>` to plot the faces of the
# triangles instead of the edges. First, we create a hue gradient for the
# triangle faces:
palette = ig.GradientPalette("midnightblue", "lightblue", 100)

# %%
# Then we can "plot" the triangles using
# :class:`matplotlib:matplotlib.patches.Polygon` and the graph using
# :func:`igraph.plot() <igraph.drawing.plot>`:
fig, ax = plt.subplots()
for tri in delaunay.simplices:
    # get the points of the triangle
    tri_points = [delaunay.points[tri[i]] for i in range(3)]

    # calculate the vertical center of the triangle
    center = (tri_points[0][1] + tri_points[1][1] + tri_points[2][1]) / 3

    # draw triangle onto axes
    poly = plt.Polygon(tri_points, color=palette.get(int(center * 100)))
    ax.add_patch(poly)

ig.plot(
    g,
    layout=layout,
    target=ax,
    vertex_size=0,
    edge_width=0.2,
    edge_color="white",
)
ax.set(xlim=(0, 1), ylim=(0, 1))
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-erdos_renyi-py"></a>File: doc\examples_sphinx-gallery\erdos_renyi.py

```python
"""
.. _tutorials-random:

=================
Erdős-Rényi Graph
=================

This example demonstrates how to generate `Erdős–Rényi graphs <https://en.wikipedia.org/wiki/Erd%C5%91s%E2%80%93R%C3%A9nyi_model>`_ using :meth:`igraph.GraphBase.Erdos_Renyi`. There are two variants of graphs:

- ``Erdos_Renyi(n, p)`` will generate a graph from the so-called :math:`G(n,p)` model where each edge between any two pair of nodes has an independent probability ``p`` of existing.
- ``Erdos_Renyi(n, m)`` will pick a graph uniformly at random out of all graphs with ``n`` nodes and ``m`` edges. This is referred to as the :math:`G(n,m)` model.

We generate two graphs of each, so we can confirm that our graph generator is truly random.
"""
import igraph as ig
import matplotlib.pyplot as plt
import random

# %%
# First, we set a random seed for reproducibility
random.seed(0)

# %%
# Then, we generate two :math:`G(n,p)` Erdős–Rényi graphs with identical parameters:
g1 = ig.Graph.Erdos_Renyi(n=15, p=0.2, directed=False, loops=False)
g2 = ig.Graph.Erdos_Renyi(n=15, p=0.2, directed=False, loops=False)

# %%
# For comparison, we also generate two :math:`G(n,m)` Erdős–Rényi graphs with a fixed number
# of edges:
g3 = ig.Graph.Erdos_Renyi(n=20, m=35, directed=False, loops=False)
g4 = ig.Graph.Erdos_Renyi(n=20, m=35, directed=False, loops=False)

# %%
# We can print out summaries of each graph to verify their randomness
ig.summary(g1)
ig.summary(g2)
ig.summary(g3)
ig.summary(g4)

# IGRAPH U--- 15 18 --
# IGRAPH U--- 15 21 --
# IGRAPH U--- 20 35 --
# IGRAPH U--- 20 35 --

# %%
# Finally, we can plot the graphs to illustrate their structures and
# differences:
fig, axs = plt.subplots(2, 2)
# Probability
ig.plot(
    g1,
    target=axs[0, 0],
    layout="circle",
    vertex_color="lightblue"
)
ig.plot(
    g2,
    target=axs[0, 1],
    layout="circle",
    vertex_color="lightblue"
)
axs[0, 0].set_ylabel('Probability')
# N edges
ig.plot(
    g3,
    target=axs[1, 0],
    layout="circle",
    vertex_color="lightblue",
    vertex_size=15
)
ig.plot(
    g4,
    target=axs[1, 1],
    layout="circle",
    vertex_color="lightblue",
    vertex_size=15
)
axs[1, 0].set_ylabel('N. edges')
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-generate_dag-py"></a>File: doc\examples_sphinx-gallery\generate_dag.py

```python
"""

.. _tutorials-dag:

======================
Directed Acyclic Graph
======================

This example demonstrates how to create a random directed acyclic graph (DAG), which is useful in a number of contexts including for Git commit history.
"""
import igraph as ig
import matplotlib.pyplot as plt
import random


# %%
# First, we set a random seed for reproducibility.
random.seed(0)

# %%
# First, we generate a random undirected graph with a fixed number of edges, without loops.
g = ig.Graph.Erdos_Renyi(n=15, m=30, directed=False, loops=False)

# %%
# Then we convert it to a DAG *in place*. This method samples DAGs with a given number of edges and vertices uniformly.
g.to_directed(mode="acyclic")

# %%
# We can print out a summary of the DAG.
ig.summary(g)


# %%
# Finally, we can plot the graph using the Sugiyama layout from :meth:`igraph.Graph.layout_sugiyama`:
fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    layout="sugiyama",
    vertex_size=15,
    vertex_color="grey",
    edge_color="#222",
    edge_width=1,
)
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-isomorphism-py"></a>File: doc\examples_sphinx-gallery\isomorphism.py

```python
"""
.. _tutorials-isomorphism:

===========
Isomorphism
===========

This example shows how to check for `isomorphism <https://en.wikipedia.org/wiki/Graph_isomorphism>`_ between small graphs using :meth:`igraph.GraphBase.isomorphic`.
"""
import igraph as ig
import matplotlib.pyplot as plt

# %%
# First we generate three different graphs:
g1 = ig.Graph([(0, 1), (0, 2), (0, 4), (1, 2), (1, 3), (2, 3), (2, 4), (3, 4)])
g2 = ig.Graph([(4, 2), (4, 3), (4, 0), (2, 3), (2, 1), (3, 1), (3, 0), (1, 0)])
g3 = ig.Graph([(4, 1), (4, 3), (4, 0), (2, 3), (2, 1), (3, 1), (3, 0), (1, 0)])

# %%
# To check whether they are isomorphic, we can use a simple method:
print("Are the graphs g1 and g2 isomorphic?")
print(g1.isomorphic(g2))
print("Are the graphs g1 and g3 isomorphic?")
print(g1.isomorphic(g3))
print("Are the graphs g2 and g3 isomorphic?")
print(g2.isomorphic(g3))

# Output:
# Are the graphs g1 and g2 isomorphic?
# True
# Are the graphs g1 and g3 isomorphic?
# False
# Are the graphs g2 and g3 isomorphic?
# False

# %%
# .. note::
#    `Graph isomorphism <https://en.wikipedia.org/wiki/Graph_isomorphism>`_ is an equivalence
#    relationship, i.e. if `g1 ~ g2` and `g2 ~ g3`, then automatically `g1 ~ g3`. Therefore,
#    we could have skipped the last check.

# %%
# We can plot the graphs to get an idea about the problem:
visual_style = {
    "vertex_color": "lightblue",
    "vertex_label": [0, 1, 2, 3, 4],
    "vertex_size": 25,
}

fig, axs = plt.subplots(1, 3)
ig.plot(
    g1,
    layout=g1.layout("circle"),
    target=axs[0],
    **visual_style,
)
ig.plot(
    g2,
    layout=g1.layout("circle"),
    target=axs[1],
    **visual_style,
)
ig.plot(
    g3,
    layout=g1.layout("circle"),
    target=axs[2],
    **visual_style,
)
fig.text(0.38, 0.5, '$\\simeq$' if g1.isomorphic(g2) else '$\\neq$', fontsize=15, ha='center', va='center')
fig.text(0.65, 0.5, '$\\simeq$' if g2.isomorphic(g3) else '$\\neq$', fontsize=15, ha='center', va='center')
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-lesmis-lesmis-txt"></a>File: doc\examples_sphinx-gallery\lesmis\lesmis.txt

The file lesmis.gml contains the weighted network of coappearances of
characters in Victor Hugo's novel "Les Miserables".  Nodes represent
characters as indicated by the labels and edges connect any pair of
characters that appear in the same chapter of the book.  The values on the
edges are the number of such coappearances.  The data on coappearances were
taken from D. E. Knuth, The Stanford GraphBase: A Platform for
Combinatorial Computing, Addison-Wesley, Reading, MA (1993).


---

## <a name="doc-examples_sphinx-gallery-maxflow-py"></a>File: doc\examples_sphinx-gallery\maxflow.py

```python
"""
.. _tutorials-maxflow:

============
Maximum Flow
============

This example shows how to construct a max flow on a directed graph with edge capacities using :meth:`igraph.Graph.maxflow`.

"""
import igraph as ig
import matplotlib.pyplot as plt

# %%
# First, we generate a graph and assign a "capacity" to each edge:
g = ig.Graph(
    6,
    [(3, 2), (3, 4), (2, 1), (4,1), (4, 5), (1, 0), (5, 0)],
    directed=True
)
g.es["capacity"] = [7, 8, 1, 2, 3, 4, 5]

# %%
# To find the max flow, we can simply run:
flow = g.maxflow(3, 0, capacity=g.es["capacity"])

print("Max flow:", flow.value)
print("Edge assignments:", flow.flow)

# Output:
# Max flow: 6.0
# Edge assignments [1.0, 5.0, 1.0, 2.0, 3.0, 3.0, 3.0]

# %%
# Finally, we can plot the directed graph to look at the situation:
fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    layout="circle",
    vertex_label=range(g.vcount()),
    vertex_color="lightblue"
)
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-minimum_spanning_trees-py"></a>File: doc\examples_sphinx-gallery\minimum_spanning_trees.py

```python
"""
.. _tutorials-minimum-spanning-trees:

======================
Minimum Spanning Trees
======================

This example shows how to generate a `minimum spanning tree <https://en.wikipedia.org/wiki/Minimum_spanning_tree>`_ from an input graph using :meth:`igraph.Graph.spanning_tree`. If you only need a regular spanning tree, check out :ref:`tutorials-spanning-trees`.

"""
import random
import igraph as ig
import matplotlib.pyplot as plt

# %%
# We start by generating a grid graph with random integer weights between 1 and
# 20:
random.seed(0)
g = ig.Graph.Lattice([5, 5], circular=False)
g.es["weight"] = [random.randint(1, 20) for _ in g.es]

# %%
# We can then compute a minimum spanning tree using
# :meth:`igraph.Graph.spanning_tree`, making sure to pass in the randomly
# generated weights.
mst_edges = g.spanning_tree(weights=g.es["weight"], return_tree=False)

# %%
# We can print out the minimum edge weight sum
print("Minimum edge weight sum:", sum(g.es[mst_edges]["weight"]))

# Minimum edge weight sum: 136

# %%
# Finally, we can plot the graph, highlighting the edges that are part of the
# minimum spanning tree.
g.es["color"] = "lightgray"
g.es[mst_edges]["color"] = "midnightblue"
g.es["width"] = 1.0
g.es[mst_edges]["width"] = 3.0

fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    layout="grid",
    vertex_color="lightblue",
    edge_width=g.es["width"],
    edge_label=g.es["weight"],
    edge_background="white",
)
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-online_user_actions-py"></a>File: doc\examples_sphinx-gallery\online_user_actions.py

```python
"""
.. _tutorials-online-user-actions:

===================
Online user actions
===================

This example reproduces a typical data science situation in an internet company. We start from a pandas DataFrame with online user actions, for instance for an online text editor: the user can create a page, edit it, or delete it. We want to construct and visualize a graph of the users highlighting collaborations on the same page/project.
"""

import igraph as ig
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# %%
# Let's start by preparing some toy data representing online users. Each row
# indicates a certain action taken by a user (e.g. click on a button within a
# website). Actual user data usually come with time stamp, but that's not
# essential for this example.
action_dataframe = pd.DataFrame([
    ['dsj3239asadsa3', 'createPage', 'greatProject'],
    ['2r09ej221sk2k5', 'editPage', 'greatProject'],
    ['dsj3239asadsa3', 'editPage', 'greatProject'],
    ['789dsadafj32jj', 'editPage', 'greatProject'],
    ['oi32ncwosap399', 'editPage', 'greatProject'],
    ['4r4320dkqpdokk', 'createPage', 'miniProject'],
    ['320eljl3lk3239', 'editPage', 'miniProject'],
    ['dsj3239asadsa3', 'editPage', 'miniProject'],
    ['3203ejew332323', 'createPage', 'private'],
    ['3203ejew332323', 'editPage', 'private'],
    ['40m11919332msa', 'createPage', 'private2'],
    ['40m11919332msa', 'editPage', 'private2'],
    ['dsj3239asadsa3', 'createPage', 'anotherGreatProject'],
    ['2r09ej221sk2k5', 'editPage', 'anotherGreatProject'],
    ],
    columns=['userid', 'action', 'project'],
)

# %%
# The goal of this example is to check when two users worked on the same page.
# We choose to use a weighted adjacency matrix for this, i.e. a table with rows
# and columns indexes by the users that has nonzero entries whenever folks
# collaborate. First, let's get the users and prepare an empty matrix:
users = action_dataframe['userid'].unique()
adjacency_matrix = pd.DataFrame(
    np.zeros((len(users), len(users)), np.int32),
    index=users,
    columns=users,
)

# %%
# Then, let's iterate over all projects one by one, and add all collaborations:
for _project, project_data in action_dataframe.groupby('project'):
    project_users = project_data['userid'].values
    for i1, user1 in enumerate(project_users):
        for user2 in project_users[:i1]:
            adjacency_matrix.at[user1, user2] += 1

# %%
# There are many ways to achieve the above matrix, so don't be surprised if you
# came up with another algorithm ;-) Now it's time to make the graph:
g = ig.Graph.Weighted_Adjacency(adjacency_matrix, mode='plus')

# %%
# We can take a look at the graph via plotting functions. We can first make a
# layout:
layout = g.layout('circle')

# %%
# Then we can prepare vertex sizes based on their closeness to other vertices
vertex_size = g.closeness()
vertex_size = [10 * v**2 if not np.isnan(v) else 10 for v in vertex_size]

# %%
# Finally, we can plot the graph:
fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    layout=layout,
    vertex_label=g.vs['name'],
    vertex_color="lightblue",
    vertex_size=vertex_size,
    edge_width=g.es["weight"],
)
plt.show()

# %%
# Loops indicate "self-collaborations", which are not very meaningful. To
# filter out loops without losing the edge weights, we can use:
g = g.simplify(combine_edges='first')

fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    layout=layout,
    vertex_label=g.vs['name'],
    vertex_color="lightblue",
    vertex_size=vertex_size,
    edge_width=g.es["weight"],
)
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-personalized_pagerank-py"></a>File: doc\examples_sphinx-gallery\personalized_pagerank.py

```python
"""
.. _tutorials-personalized_pagerank:

===============================
Personalized PageRank on a grid
===============================

This example demonstrates how to calculate and visualize personalized PageRank on a grid. We use the :meth:`igraph.Graph.personalized_pagerank` method, and demonstrate the effects on a grid graph.
"""

# %%
# .. note::
#
#    The PageRank score of a vertex reflects the probability that a random walker will be at that vertex over the long run. At each step the walker has a 1 - damping chance to restart the walk and pick a starting vertex according to the probabilities defined in the reset vector.

import igraph as ig
import matplotlib.cm as cm
import matplotlib.pyplot as plt
import numpy as np

# %%
# We define a function that plots the graph on a Matplotlib axis, along with
# its personalized PageRank values. The function also generates a
# color bar on the side to see how the values change.
# We use `Matplotlib's Normalize class <https://matplotlib.org/stable/api/_as_gen/matplotlib.colors.Normalize.html>`_
# to set the colors and ensure that our color bar range is correct.


def plot_pagerank(graph: ig.Graph, p_pagerank: list[float]):
    """Plots personalized PageRank values on a grid graph with a colorbar.

    Parameters
    ----------
    graph : ig.Graph
        graph to plot
    p_pagerank : list[float]
        calculated personalized PageRank values
    """
    # Create the axis for matplotlib
    _, ax = plt.subplots(figsize=(8, 8))

    # Create a matplotlib colormap
    # coolwarm goes from blue (lowest value) to red (highest value)
    cmap = cm.coolwarm

    # Normalize the PageRank values for colormap
    normalized_pagerank = ig.rescale(p_pagerank)

    graph.vs["color"] = [cmap(pr) for pr in normalized_pagerank]
    graph.vs["size"] = ig.rescale(p_pagerank, (20, 40))
    graph.es["color"] = "gray"
    graph.es["width"] = 1.5

    # Plot the graph
    ig.plot(graph, target=ax, layout=graph.layout_grid())

    # Add a colorbar
    sm = cm.ScalarMappable(norm=plt.Normalize(min(p_pagerank), max(p_pagerank)), cmap=cmap)
    plt.colorbar(sm, ax=ax, label="Personalized PageRank")

    plt.title("Graph with Personalized PageRank")
    plt.axis("equal")
    plt.show()


# %%
# First, we generate a graph, e.g. a Lattice Graph, which basically is a ``dim x dim`` grid:
dim = 5
grid_size = (dim, dim)  # dim rows, dim columns
g = ig.Graph.Lattice(dim=grid_size, circular=False)

# %%
# Then we initialize the ``reset_vector`` (it's length should be equal to the number of vertices in the graph):
reset_vector = np.zeros(g.vcount())

# %%
# Then we set the nodes to prioritize, for example nodes with indices ``0`` and ``18``:
reset_vector[0] = 1
reset_vector[18] = 0.65

# %%
# Then we calculate the personalized PageRank:
personalized_page_rank = g.personalized_pagerank(damping=0.85, reset=reset_vector)

# %%
# Finally, we plot the graph with the personalized PageRank values:
plot_pagerank(g, personalized_page_rank)


# %%
# Alternatively, we can play around with the ``damping`` parameter:
personalized_page_rank = g.personalized_pagerank(damping=0.45, reset=reset_vector)

# %%
# Here we can see the same plot with the new damping parameter:
plot_pagerank(g, personalized_page_rank)

```

---

## <a name="doc-examples_sphinx-gallery-quickstart-py"></a>File: doc\examples_sphinx-gallery\quickstart.py

```python
"""
.. _tutorials-quickstart:

===========
Quick Start
===========
For the eager folks out there, this intro will give you a quick overview of the following operations:

- Construct a graph
- Set attributes of nodes and edges
- Plot a graph using matplotlib
- Save the plot as an image
- Export and import a graph as a ``.gml`` file

To find out more features that igraph has to offer, check out the :ref:`gallery`!

"""
import igraph as ig
import matplotlib.pyplot as plt

# Construct a graph with 5 vertices
n_vertices = 5
edges = [(0, 1), (0, 2), (0, 3), (0, 4), (1, 2), (1, 3), (1, 4), (3, 4)]
g = ig.Graph(n_vertices, edges)

# Set attributes for the graph, nodes, and edges
g["title"] = "Small Social Network"
g.vs["name"] = ["Daniel Morillas", "Kathy Archer", "Kyle Ding", "Joshua Walton", "Jana Hoyer"]
g.vs["gender"] = ["M", "F", "F", "M", "F"]
g.es["married"] = [False, False, False, False, False, False, False, True]

# Set individual attributes
g.vs[1]["name"] = "Kathy Morillas"
g.es[0]["married"] = True

# Plot in matplotlib
# Note that attributes can be set globally (e.g. vertex_size), or set individually using arrays (e.g. vertex_color)
fig, ax = plt.subplots(figsize=(5,5))
ig.plot(
    g,
    target=ax,
    layout="circle", # print nodes in a circular layout
    vertex_size=30,
    vertex_color=["steelblue" if gender == "M" else "salmon" for gender in g.vs["gender"]],
    vertex_frame_width=4.0,
    vertex_frame_color="white",
    vertex_label=g.vs["name"],
    vertex_label_size=7.0,
    edge_width=[2 if married else 1 for married in g.es["married"]],
    edge_color=["#7142cf" if married else "#AAA" for married in g.es["married"]]
)

plt.show()

# Save the graph as an image file
fig.savefig('social_network.png')
fig.savefig('social_network.jpg')
fig.savefig('social_network.pdf')

# Export and import a graph as a GML file.
g.save("social_network.gml")
g = ig.load("social_network.gml")

```

---

## <a name="doc-examples_sphinx-gallery-ring_animation-py"></a>File: doc\examples_sphinx-gallery\ring_animation.py

```python
"""
.. _tutorials-ring-animation:

====================
Ring Graph Animation
====================

This example demonstrates how to use :doc:`matplotlib:api/animation_api` in
order to animate a ring graph sequentially being revealed.

"""
import igraph as ig
import matplotlib.pyplot as plt
import matplotlib.animation as animation

# sphinx_gallery_thumbnail_path = '_static/gallery_thumbnails/ring_animation.gif'

# %%
# Create a ring graph, which we will then animate
g = ig.Graph.Ring(10, directed=True)

# %%
# Compute a 2D ring layout that looks like an actual ring
layout = g.layout_circle()

# %%
# Prepare an update function. This "callback" function will be run at every
# frame and takes as a single argument the frame number. For simplicity, at
# each frame we compute a subgraph with only a fraction of the vertices and
# edges. As time passes, the graph becomes more and more complete until the
# whole ring is closed.
#
# .. note::
#    The beginning and end of the animation are a little tricky because only
#    a vertex or edge is added, not both. Don't worry if you cannot understand
#    all details immediately.
def _update_graph(frame):
    # Remove plot elements from the previous frame
    ax.clear()

    # Fix limits (unless you want a zoom-out effect)
    ax.set_xlim(-1.5, 1.5)
    ax.set_ylim(-1.5, 1.5)

    if frame < 10:
        # Plot subgraph
        gd = g.subgraph(range(frame))
    elif frame == 10:
        # In the second-to-last frame, plot all vertices but skip the last
        # edge, which will only be shown in the last frame
        gd = g.copy()
        gd.delete_edges(9)
    else:
        # Last frame
        gd = g

    ig.plot(gd, target=ax, layout=layout[:frame], vertex_color="yellow")

    # Capture handles for blitting
    if frame == 0:
        nhandles = 0
    elif frame == 1:
        nhandles = 1
    elif frame < 11:
        # vertex, 2 for each edge
        nhandles = 3 * frame
    else:
        # The final edge closing the circle
        nhandles = 3 * (frame - 1) + 2

    handles = ax.get_children()[:nhandles]
    return handles

# %%
# Run the animation
fig, ax = plt.subplots()
ani = animation.FuncAnimation(fig, _update_graph, 12, interval=500, blit=True)
plt.ion()
plt.show()

# %%
# .. note::
#
#    We use *igraph*'s :meth:`Graph.subgraph()` (see
#    :meth:`igraph.GraphBase.induced_subgraph`) in order to obtain a section of
#    the ring graph at a time for each frame. While sufficient for an easy
#    example, this approach is not very efficient. Thinking of more efficient
#    approaches, e.g. vertices with zero radius, is a useful exercise to learn
#    the combination of igraph and matplotlib.

```

---

## <a name="doc-examples_sphinx-gallery-shortest_path_visualisation-py"></a>File: doc\examples_sphinx-gallery\shortest_path_visualisation.py

```python
"""
.. _tutorials-shortest-paths:

==============
Shortest Paths
==============

This example demonstrates how to find the shortest distance between two vertices
of a weighted or an unweighted graph.
"""
import igraph as ig
import matplotlib.pyplot as plt

# %%
# To find the shortest path or distance between two nodes, we can use :meth:`igraph.GraphBase.get_shortest_paths`. If we're only interested in counting the unweighted distance, then we can do the following:
g = ig.Graph(
    6,
    [(0, 1), (0, 2), (1, 3), (2, 3), (2, 4), (3, 5), (4, 5)]
)
results = g.get_shortest_paths(1, to=4, output="vpath")

# results = [[1, 0, 2, 4]]

# %%
# We can print the result of the computation:
if len(results[0]) > 0:
    # The distance is the number of vertices in the shortest path minus one.
    print("Shortest distance is: ", len(results[0])-1)
else:
    print("End node could not be reached!")

# %%
# If the edges have weights, things are a little different. First, let's add
# weights to our graph edges:
g.es["weight"] = [2, 1, 5, 4, 7, 3, 2]

# %%
# To get the shortest paths on a weighted graph, we pass the weights as an
# argument. For a change, we choose the output format as ``"epath"`` to
# receive the path as an edge list, which can be used to calculate the length
# of the path.
results = g.get_shortest_paths(0, to=5, weights=g.es["weight"], output="epath")

# results = [[1, 3, 5]]

if len(results[0]) > 0:
    # Add up the weights across all edges on the shortest path
    distance = 0
    for e in results[0]:
        distance += g.es[e]["weight"]
    print("Shortest weighted distance is: ", distance)
else:
    print("End node could not be reached!")

# %%
# .. note::
#
#     - :meth:`igraph.GraphBase.get_shortest_paths` returns a list of lists becuase the `to` argument can also accept a list of vertex IDs. In that case, the shortest path to all each vertex is found and stored in the results array.
#     - If you're interested in finding *all* shortest paths, take a look at :meth:`igraph.GraphBase.get_all_shortest_paths`.

# %%
# In case you are wondering how the visualization figure was done, here's the code:
g.es['width'] = 0.5
g.es[results[0]]['width'] = 2.5

fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    layout='circle',
    vertex_color='steelblue',
    vertex_label=range(g.vcount()),
    edge_width=g.es['width'],
    edge_label=g.es["weight"],
    edge_color='#666',
    edge_align_label=True,
    edge_background='white'
)
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-simplify-py"></a>File: doc\examples_sphinx-gallery\simplify.py

```python
"""
========
Simplify
========

This example shows how to remove self loops and multiple edges using :meth:`igraph.GraphBase.simplify`.
"""
import igraph as ig
import matplotlib.pyplot as plt

# %%
# We start with a graph that includes loops and multiedges:
g1 = ig.Graph([
    (0, 1),
    (1, 2),
    (2, 3),
    (3, 4),
    (4, 0),
    (0, 0),
    (1, 4),
    (1, 4),
    (0, 2),
    (2, 4),
    (2, 4),
    (2, 4),
    (3, 3)],
)

# %%
# To simplify the graph, we must remember that the function operates in place,
# i.e. directly changes the graph that it is run on. So we need to first make a
# copy of our graph, and then simplify that copy to keep the original graph
# untouched:
g2 = g1.copy()
g2.simplify()

# %%
# We can then proceed to plot both graphs to see the difference. First, let's
# choose a consistent visual style:
visual_style = {
    "vertex_color": "lightblue",
    "vertex_size": 20,
    "vertex_label": [0, 1, 2, 3, 4],
}

# %%
# And finally, let's plot them in twin axes, with rectangular frames around
# each plot:
fig, axs = plt.subplots(1, 2, sharex=True, sharey=True)
ig.plot(
    g1,
    layout="circle",
    target=axs[0],
    **visual_style,
)
ig.plot(
    g2,
    layout="circle",
    target=axs[1],
    **visual_style,
)
axs[0].set_title('Multigraph...')
axs[1].set_title('...simplified')
# Draw rectangles around axes
axs[0].add_patch(plt.Rectangle(
    (0, 0), 1, 1, fc='none', ec='k', lw=4, transform=axs[0].transAxes,
    ))
axs[1].add_patch(plt.Rectangle(
    (0, 0), 1, 1, fc='none', ec='k', lw=4, transform=axs[1].transAxes,
    ))
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-spanning_trees-py"></a>File: doc\examples_sphinx-gallery\spanning_trees.py

```python
"""
.. _tutorials-spanning-trees:

==============
Spanning Trees
==============

This example shows how to generate a spanning tree from an input graph using :meth:`igraph.Graph.spanning_tree`. For the related idea of finding a *minimum spanning tree*, see :ref:`tutorials-minimum-spanning-trees`.
"""
import igraph as ig
import matplotlib.pyplot as plt
import random

# %%
# First we create a two-dimensional, 6 by 6 lattice graph:
g = ig.Graph.Lattice([6, 6], circular=False)

# %%
# We can compute the 2D layout of the graph:
layout = g.layout("grid")

# %%
# To spice things up a little, we rearrange the vertex ids and compute a new
# layout. While not terribly useful in this context, it does make for a more
# interesting-looking spanning tree ;-)
random.seed(0)
permutation = list(range(g.vcount()))
random.shuffle(permutation)
g = g.permute_vertices(permutation)
new_layout = g.layout("grid")
for i in range(36):
    new_layout[permutation[i]] = layout[i]
layout = new_layout

# %%
# We can now generate a spanning tree:
spanning_tree = g.spanning_tree(weights=None, return_tree=False)

# %%
# Finally, we can plot the graph with a highlight color for the spanning tree.
# We follow the usual recipe: first we set a few aesthetic options and then we
# leverage :func:`igraph.plot() <igraph.drawing.plot>` and matplotlib for the
# heavy lifting:
g.es["color"] = "lightgray"
g.es[spanning_tree]["color"] = "midnightblue"
g.es["width"] = 0.5
g.es[spanning_tree]["width"] = 3.0

fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    layout=layout,
    vertex_color="lightblue",
    edge_width=g.es["width"]
)
plt.show()

# %%
# .. note::
#   To invert the y axis such that the root of the tree is on top of the plot,
#   you can call `ax.invert_yaxis()` before `plt.show()`.

```

---

## <a name="doc-examples_sphinx-gallery-topological_sort-py"></a>File: doc\examples_sphinx-gallery\topological_sort.py

```python
"""
.. _tutorials-topological-sort:

===================
Topological sorting
===================

This example demonstrates how to get a topological sorting on a directed acyclic graph (DAG). A topological sorting of a directed graph is a linear ordering based on the precedence implied by the directed edges. It exists iff the graph doesn't have any cycle. In ``igraph``, we can use :meth:`igraph.GraphBase.topological_sorting` to get a topological ordering of the vertices.
"""
import igraph as ig
import matplotlib.pyplot as plt


# %%
# First off, we generate a directed acyclic graph (DAG):
g = ig.Graph(
    edges=[(0, 1), (0, 2), (1, 3), (2, 4), (4, 3), (3, 5), (4, 5)],
    directed=True,
)

# %%
# We can verify immediately that this is actually a DAG:
assert g.is_dag

# %%
# A topological sorting can be computed quite easily by calling
# :meth:`igraph.GraphBase.topological_sorting`, which returns a list of vertex IDs.
# If the given graph is not DAG, the error will occur.
results = g.topological_sorting(mode='out')
print('Topological sort of g (out):', *results)

# %%
# In fact, there are two modes of :meth:`igraph.GraphBase.topological_sorting`,
# ``'out'`` ``'in'``. ``'out'`` is the default and starts from a node with
# indegree equal to 0. Vice versa, ``'in'`` starts from a node with outdegree
# equal to 0. To call the other mode, we can simply use:
results = g.topological_sorting(mode='in')
print('Topological sort of g (in):', *results)

# %%
# We can use :meth:`igraph.Vertex.indegree` to find the indegree of the node.
for i in range(g.vcount()):
    print('degree of {}: {}'.format(i, g.vs[i].indegree()))

# %
# Finally, we can plot the graph to make the situation a little clearer.
# Just to change things up a bit, we use the matplotlib visualization mode
# inspired by `xkcd <https://xkcd.com/>_:
with plt.xkcd():
    fig, ax = plt.subplots(figsize=(5, 5))
    ig.plot(
        g,
        target=ax,
        layout='kk',
        vertex_size=25,
        edge_width=4,
        vertex_label=range(g.vcount()),
        vertex_color="white",
    )

```

---

## <a name="doc-examples_sphinx-gallery-visual_style-py"></a>File: doc\examples_sphinx-gallery\visual_style.py

```python
"""
.. _tutorials-visual-style:

Visual styling
===========================

This example shows how to change the visual style of network plots.
"""
import igraph as ig
import matplotlib.pyplot as plt
import random

# %%
# To configure the visual style of a plot, we can create a dictionary with the
# various setting we want to customize:
visual_style = {
    "edge_width": 0.3,
    "vertex_size": 15,
    "palette": "heat",
    "layout": "fruchterman_reingold"
}

# %%
# Let's see it in action! First, we generate four random graphs:
random.seed(1)
gs = [ig.Graph.Barabasi(n=30, m=1) for i in range(4)]

# %%
# Then, we calculate a color colors between 0-255 for all nodes, e.g. using
# betweenness just as an example:
betweenness = [g.betweenness() for g in gs]
colors = [[int(i * 255 / max(btw)) for i in btw] for btw in betweenness]

# %%
# Finally, we can plot the graphs using the same visual style for all graphs:
fig, axs = plt.subplots(2, 2)
axs = axs.ravel()
for g, color, ax in zip(gs, colors, axs):
    ig.plot(g, target=ax, vertex_color=color, **visual_style)
plt.show()


# %%
# .. note::
#    If you would like to set global defaults, for example, always using the
#    Matplotlib plotting backend, or using a particular color palette by
#    default, you can use igraph's `configuration instance
#    :class:`igraph.configuration.Configuration`. A quick example on how to use
#    it can be found here: :ref:`tutorials-configuration`.

# %%
# In the matplotlib backend, igraph creates a special container
# :class:`igraph.drawing.matplotlib.graph.GraphArtist` which is a matplotlib Artist
# and the first child of the target Axes. That object can be used to customize
# the plot appearance after the initial drawing, e.g.:
g = ig.Graph.Barabasi(n=30, m=1)
fig, ax = plt.subplots()
ig.plot(g, target=ax)
artist = ax.get_children()[0]
# Option 1:
artist.set(vertex_color="blue")
# Option 2:
artist.set_vertex_color("blue")
plt.show()

# %%
# .. note::
#    The :meth:`igraph.drawing.matplotlib.graph.GraphArtist.set` method can
#    be used to change multiple properties at once and is generally more
#    efficient than multiple calls to specific ``artist.set_...`` methods.

# %%
# In the matplotlib backend, you can also specify the size of self-loops,
# either as a number or a sequence of numbers, e.g.:
g = ig.Graph(n=5)
g.add_edge(2, 3)
g.add_edge(0, 0)
g.add_edge(1, 1)
fig, ax = plt.subplots()
ig.plot(
    g,
    target=ax,
    vertex_size=20,
    edge_loop_size=[
        0,  # ignored, the first edge is not a loop
        30,  # loop for vertex 0
        80,  # loop for vertex 1
    ],
)
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-visualize_cliques-py"></a>File: doc\examples_sphinx-gallery\visualize_cliques.py

```python
"""
.. _tutorials-cliques:

============
Cliques
============

This example shows how to compute and visualize cliques of a graph using :meth:`igraph.GraphBase.cliques`.

"""
import igraph as ig
import matplotlib.pyplot as plt

# %%
# First, let's create a graph, for instance the famous karate club graph:
g = ig.Graph.Famous('Zachary')

# %%
# Computing cliques can be done as follows:
cliques = g.cliques(4, 4)

# %%
# We can plot the result of the computation. To make things a little more
# interesting, we plot each clique highlighted in a separate axes:
fig, axs = plt.subplots(3, 4)
axs = axs.ravel()
for clique, ax in zip(cliques, axs):
    ig.plot(
        ig.VertexCover(g, [clique]),
        mark_groups=True, palette=ig.RainbowPalette(),
        vertex_size=5,
        edge_width=0.5,
        target=ax,
    )
plt.axis('off')
plt.show()


# %%
# Advanced: improving plotting style
# ----------------------------------
# If you want a little more style, you can color the vertices/edges within each
# clique to make them stand out:
fig, axs = plt.subplots(3, 4)
axs = axs.ravel()
for clique, ax in zip(cliques, axs):
    # Color vertices yellow/red based on whether they are in this clique
    g.vs['color'] = 'yellow'
    g.vs[clique]['color'] = 'red'

    # Color edges black/red based on whether they are in this clique
    clique_edges = g.es.select(_within=clique)
    g.es['color'] = 'black'
    clique_edges['color'] = 'red'
    # also increase thickness of clique edges
    g.es['width'] = 0.3
    clique_edges['width'] = 1

    ig.plot(
        ig.VertexCover(g, [clique]),
        mark_groups=True,
        palette=ig.RainbowPalette(),
        vertex_size=5,
        target=ax,
    )
plt.axis('off')
plt.show()

```

---

## <a name="doc-examples_sphinx-gallery-visualize_communities-py"></a>File: doc\examples_sphinx-gallery\visualize_communities.py

```python
"""
.. _tutorials-visualize-communities:

=====================
Communities
=====================

This example shows how to visualize communities or clusters of a graph.
"""
import igraph as ig
import matplotlib.pyplot as plt

# %%
# First, we generate a graph. We use a famous graph here for simplicity:
g = ig.Graph.Famous("Zachary")

# %%
# Edge betweenness is a standard way to detect communities. We then covert into
# a :class:`igraph.VertexClustering` object for subsequent ease of use:
communities = g.community_edge_betweenness()
communities = communities.as_clustering()

# %%
# Next, we color each vertex and edge based on its community membership:
num_communities = len(communities)
palette = ig.RainbowPalette(n=num_communities)
for i, community in enumerate(communities):
    g.vs[community]["color"] = i
    community_edges = g.es.select(_within=community)
    community_edges["color"] = i


# %%
# Last, we plot the graph. We use a fancy technique called proxy artists to
# make a legend. You can find more about that in matplotlib's
# :doc:`matplotlib:users/explain/axes/legend_guide`:
fig, ax = plt.subplots()
ig.plot(
    communities,
    palette=palette,
    edge_width=1,
    target=ax,
    vertex_size=20,
)

# Create a custom color legend
legend_handles = []
for i in range(num_communities):
    handle = ax.scatter(
        [], [],
        s=100,
        facecolor=palette.get(i),
        edgecolor="k",
        label=i,
    )
    legend_handles.append(handle)
ax.legend(
    handles=legend_handles,
    title='Community:',
    bbox_to_anchor=(0, 1.0),
    bbox_transform=ax.transAxes,
)
plt.show()

# %%
# For an example on how to generate the cluster graph from a vertex cluster,
# check out :ref:`tutorials-cluster-graph`.

```

---

## <a name="doc-source-analysis-rst"></a>File: doc\source\analysis.rst

.. include:: include/global.rst

.. currentmodule:: igraph


Graph analysis
==============

|igraph| enables analysis of graphs/networks from simple operations such as adding and removing nodes to complex theoretical constructs such as community detection. Read the :doc:`api/index` for details on each function and class.

The context for the following examples will be to import |igraph| (commonly as `ig`), have the :class:`Graph` class and to have one or more graphs available::

    >>> import igraph as ig
    >>> from igraph import Graph
    >>> g = Graph(edges=[[0, 1], [2, 3]])

To get a summary representation of the graph, use :meth:`Graph.summary`. For instance::

    >>> g.summary(verbosity=1)

will provide a fairly detailed description.

To copy a graph, use :meth:`Graph.copy`. This is a "shallow" copy: any mutable objects in the attributes are not copied (they would refer to the same instance).
If you want to copy a graph including all its attributes, use Python's `deepcopy` module.

Vertices and edges
++++++++++++++++++

Vertices are numbered 0 to `n-1`, where n is the number of vertices in the graph. These are called the "vertex ids".
To count vertices, use :meth:`Graph.vcount`::

    >>> n = g.vcount()

Edges also have ids from 0 to `m-1` and are counted by :meth:`Graph.ecount`::

    >>> m = g.ecount()

To get a sequence of vertices, use their ids and :attr:`Graph.vs`::

    >>> for v in g.vs:
    >>>     print(v)

Similarly for edges, use :attr:`Graph.es`::

    >>> for e in g.es:
    >>>     print(e)

You can index and slice vertices and edges like indexing and slicing a list::

    >>> g.vs[:4]
    >>> g.vs[0, 2, 4]
    >>> g.es[3]

.. note:: The `vs` and `es` attributes are special sequences with their own useful methods. See the :doc:`api/index` for a full list.

If you prefer a vanilla edge list, you can use :meth:`Graph.get_edge_list`.

Incidence
++++++++++++++++++++++++++++++
To get the vertices at the two ends of an edge, use :attr:`Edge.source` and :attr:`Edge.target`::

    >>> e = g.es[0]
    >>> v1, v2 = e.source, e.target

Vice versa, to get the edge if from the source and target vertices, you can use :meth:`Graph.get_eid` or, for multiple pairs of source/targets,
:meth:`Graph.get_eids`. The boolean version, asking whether two vertices are directly connected, is :meth:`Graph.are_adjacent`.

To get the edges incident on a vertex, you can use :meth:`Vertex.incident`, :meth:`Vertex.out_edges` and
:meth:`Vertex.in_edges`. The three are equivalent on undirected graphs but not directed ones of course::

    >>> v = g.vs[0]
    >>> edges = v.incident()

The :meth:`Graph.incident` function fulfills the same purpose with a slightly different syntax based on vertex IDs::

    >>> edges = g.incident(0)

To get the full adjacency/incidence list representation of the graph, use :meth:`Graph.get_adjlist`, :meth:`Graph.g.get_inclist()` or, for a bipartite graph, :meth:`Graph.get_biadjacency`.

Neighborhood
++++++++++++

To compute the neighbors, successors, and predecessors, the methods :meth:`Graph.neighbors`, :meth:`Graph.successors` and
:meth:`Graph.predecessors` are available. The three give the same answer in undirected graphs and have a similar dual syntax::

    >>> neis = g.vs[0].neighbors()
    >>> neis = g.neighbors(0)

To get the list of vertices within a certain distance from one or more initial vertices, you can use :meth:`Graph.neighborhood`::

    >>> g.neighborhood([0, 1], order=2)

and to find the neighborhood size, there is :meth:`Graph.neighborhood_size`.

Degrees
+++++++
To compute the degree, in-degree, or out-degree of a node, use :meth:`Vertex.degree`, :meth:`Vertex.indegree`, and :meth:`Vertex.outdegree`::

    >>> deg = g.vs[0].degree()
    >>> deg = g.degree(0)

To compute the max degree in a list of vertices, use :meth:`Graph.maxdegree`.

:meth:`Graph.knn` computes the average degree of the neighbors.

Adding and removing vertices and edges
++++++++++++++++++++++++++++++++++++++

To add nodes to a graph, use :meth:`Graph.add_vertex` and :meth:`Graph.add_vertices`::

    >>> g.add_vertex()
    >>> g.add_vertices(5)

This changes the graph `g` in place. You can specify the name of the vertices if you wish.

To remove nodes, use :meth:`Graph.delete_vertices`::

    >>> g.delete_vertices([1, 2])

Again, you can specify the names or the actual :class:`Vertex` objects instead.

To add edges, use :meth:`Graph.add_edge` and :meth:`Graph.add_edges`::

    >>> g.add_edge(0, 2)
    >>> g.add_edges([(0, 2), (1, 3)])

To remove edges, use :meth:`Graph.delete_edges`::

    >>> g.delete_edges([0, 5]) # remove by edge id

You can also remove edges between source and target nodes.

To contract vertices, use :meth:`Graph.contract_vertices`. Edges between contracted vertices will become loops.

Graph operators
+++++++++++++++

It is possible to compute the union, intersection, difference, and other set operations (operators) between graphs.

To compute the union of the graphs (nodes/edges in either are kept)::

    >>> gu = ig.union([g, g2, g3])

Similarly for the intersection (nodes/edges present in all are kept)::

    >>> gu = ig.intersection([g, g2, g3])

These two operations preserve attributes and can be performed with a few variations. The most important one is that vertices can be matched across the graphs by id (number) or by name.

These and other operations are also available as methods of the :class:`Graph` class::

    >>> g.union(g2)
    >>> g.intersection(g2)
    >>> g.disjoint_union(g2)
    >>> g.difference(g2)
    >>> g.complementer()  # complement graph, same nodes but missing edges

and even as numerical operators::

    >>> g |= g2
    >>> g_intersection = g and g2

Topological sorting
+++++++++++++++++++

To sort a graph topologically, use :meth:`Graph.topological_sorting`::

    >>> g = ig.Graph.Tree(10, 2, mode=ig.TREE_OUT)
    >>> g.topological_sorting()

Graph traversal
+++++++++++++++

A common operation is traversing the graph. |igraph| currently exposes breadth-first search (BFS) via :meth:`Graph.bfs` and :meth:`Graph.bfsiter`::

    >>> [vertices, layers, parents] = g.bfs()
    >>> it = g.bfsiter()  # Lazy version

Depth-first search has a similar infrastructure via :meth:`Graph.dfs` and :meth:`Graph.dfsiter`::

    >>> [vertices, parents] = g.dfs()
    >>> it = g.dfsiter()  # Lazy version

To perform a random walk from a certain vertex, use :meth:`Graph.random_walk`::

    >>> vids = g.random_walk(0, 3)

Pathfinding and cuts
++++++++++++++++++++
Several pathfinding algorithms are available:

- :meth:`Graph.shortest_paths` or :meth:`Graph.get_shortest_paths`
- :meth:`Graph.get_all_shortest_paths`
- :meth:`Graph.get_all_simple_paths`
- :meth:`Graph.spanning_tree` finds a minimum spanning tree

As well as functions related to cuts and paths:

- :meth:`Graph.mincut` calculates the minimum cut between the source and target vertices
- :meth:`Graph.st_mincut` - as previous one, but returns a simpler data structure
- :meth:`Graph.mincut_value` - as previous one, but returns only the value
- :meth:`Graph.all_st_cuts`
- :meth:`Graph.all_st_mincuts`
- :meth:`Graph.edge_connectivity` or :meth:`Graph.edge_disjoint_paths` or :meth:`Graph.adhesion`
- :meth:`Graph.vertex_connectivity` or :meth:`Graph.cohesion`

See also the section on flow.

Global properties
+++++++++++++++++

A number of global graph measures are available.

Basic:

- :meth:`Graph.diameter` or :meth:`Graph.get_diameter`
- :meth:`Graph.girth`
- :meth:`Graph.radius`
- :meth:`Graph.average_path_length`

Distributions:

- :meth:`Graph.degree_distribution`
- :meth:`Graph.path_length_hist`

Connectedness:

- :meth:`Graph.all_minimal_st_separators`
- :meth:`Graph.minimum_size_separators`
- :meth:`Graph.cut_vertices` or :meth:`Graph.articulation_points`

Cliques and motifs:

- :meth:`Graph.clique_number` (aka :meth:`Graph.omega`)
- :meth:`Graph.cliques`
- :meth:`Graph.maximal_cliques`
- :meth:`Graph.largest_cliques`
- :meth:`Graph.motifs_randesu` and :meth:`Graph.motifs_randesu_estimate`
- :meth:`Graph.motifs_randesu_no` counts the number of motifs

Directed acyclic graphs:

- :meth:`Graph.is_dag`
- :meth:`Graph.feedback_arc_set`
- :meth:`Graph.topological_sorting`

Optimality:

- :meth:`Graph.farthest_points`
- :meth:`Graph.modularity`
- :meth:`Graph.maximal_cliques`
- :meth:`Graph.largest_cliques`
- :meth:`Graph.independence_number` (aka :meth:`Graph.alpha`)
- :meth:`Graph.maximal_independent_vertex_sets`
- :meth:`Graph.largest_independent_vertex_sets`
- :meth:`Graph.mincut`
- :meth:`Graph.mincut_value`
- :meth:`Graph.feedback_arc_set`
- :meth:`Graph.maximum_bipartite_matching` (bipartite graphs)

Other complex measures are:

- :meth:`Graph.assortativity`
- :meth:`Graph.assortativity_degree`
- :meth:`Graph.assortativity_nominal`
- :meth:`Graph.density`
- :meth:`Graph.transitivity_undirected`
- :meth:`Graph.transitivity_avglocal_undirected`
- :meth:`Graph.dyad_census`
- :meth:`Graph.triad_census`
- :meth:`Graph.reciprocity` (directed graphs)
- :meth:`Graph.isoclass` (only 3 or 4 vertices)
- :meth:`Graph.biconnected_components` aka :meth:`Graph.blocks`

Boolean properties:

- :meth:`Graph.is_bipartite`
- :meth:`Graph.is_connected`
- :meth:`Graph.is_dag`
- :meth:`Graph.is_directed`
- :meth:`Graph.is_named`
- :meth:`Graph.is_simple`
- :meth:`Graph.is_weighted`
- :meth:`Graph.has_multiple`

Vertex properties
+++++++++++++++++++
A spectrum of vertex-level properties can be computed. Similarity measures include:

- :meth:`Graph.similarity_dice`
- :meth:`Graph.similarity_jaccard`
- :meth:`Graph.similarity_inverse_log_weighted`
- :meth:`Graph.diversity`

Structural:

- :meth:`Graph.authority_score`
- :meth:`Graph.hub_score`
- :meth:`Graph.betweenness`
- :meth:`Graph.bibcoupling`
- :meth:`Graph.closeness`
- :meth:`Graph.constraint`
- :meth:`Graph.cocitation`
- :meth:`Graph.coreness` (aka :meth:`Graph.shell_index`)
- :meth:`Graph.eccentricity`
- :meth:`Graph.eigenvector_centrality`
- :meth:`Graph.harmonic_centrality`
- :meth:`Graph.pagerank`
- :meth:`Graph.personalized_pagerank`
- :meth:`Graph.strength`
- :meth:`Graph.transitivity_local_undirected`

Connectedness:

- :meth:`Graph.subcomponent`
- :meth:`Graph.is_separator`
- :meth:`Graph.is_minimal_separator`

Edge properties
+++++++++++++++
As for vertices, edge properties are implemented. Basic properties include:

- :meth:`Graph.is_loop`
- :meth:`Graph.is_multiple`
- :meth:`Graph.is_mutual`
- :meth:`Graph.count_multiple`

and more complex ones:

- :meth:`Graph.edge_betweenness`

Matrix representations
+++++++++++++++++++++++
Matrix-related functionality includes:

- :meth:`Graph.get_adjacency`
- :meth:`Graph.get_adjacency_sparse` (sparse CSR matrix version)
- :meth:`Graph.laplacian`

Clustering
++++++++++
|igraph| includes several approaches to unsupervised graph clustering and community detection:

- :meth:`Graph.components` (aka :meth:`Graph.connected_components`): the connected components
- :meth:`Graph.cohesive_blocks`
- :meth:`Graph.community_edge_betweenness`
- :meth:`Graph.community_fastgreedy`
- :meth:`Graph.community_infomap`
- :meth:`Graph.community_label_propagation`
- :meth:`Graph.community_leading_eigenvector`
- :meth:`Graph.community_leiden`
- :meth:`Graph.community_multilevel` (a version of Louvain)
- :meth:`Graph.community_optimal_modularity` (exact solution, < 100 vertices)
- :meth:`Graph.community_spinglass`
- :meth:`Graph.community_walktrap`

Simplification, permutations and rewiring
+++++++++++++++++++++++++++++++++++++++++
To check is a graph is simple, you can use :meth:`Graph.is_simple`::

    >>> g.is_simple()

To simplify a graph (remove multiedges and loops), use :meth:`Graph.simplify`::

    >>> g_simple = g.simplify()

To return a directed/undirected copy of the graph, use :meth:`Graph.as_directed` and :meth:`Graph.as_undirected`, respectively.

To permute the order of vertices, you can use :meth:`Graph.permute_vertices`::

    >>> g = ig.Tree(6, 2)
    >>> g_perm = g.permute_vertices([1, 0, 2, 3, 4, 5])

The canonical permutation can be obtained via :meth:`Graph.canonical_permutation`, which can then be directly passed to the function above.

To rewire the graph at random, there are:

- :meth:`Graph.rewire` - preserves the degree distribution
- :meth:`Graph.rewire_edges` - fixed rewiring probability for each endpoint

Line graph
++++++++++

To compute the line graph of a graph `g`, which represents the connectedness of the *edges* of g, you can use :meth:`Graph.linegraph`::

    >>> g = Graph(n=4, edges=[[0, 1], [0, 2]])
    >>> gl = g.linegraph()

In this case, the line graph has two vertices, representing the two edges of the original graph, and one edge, representing the point where those two original edges touch.

Composition and subgraphs
+++++++++++++++++++++++++

The function :meth:`Graph.decompose` decomposes the graph into subgraphs. Vice versa, the function :meth:`Graph.compose` returns the composition of two graphs.

To compute the subgraph spannes by some vertices/edges, use :meth:`Graph.subgraph` (aka :meth:`Graph.induced_subgraph`) and :meth:`Graph.subgraph_edges`::

    >>> g_sub = g.subgraph([0, 1])
    >>> g_sub = g.subgraph_edges([0])

To compute the minimum spanning tree, use :meth:`Graph.spanning_tree`.

To compute graph k-cores, the method :meth:`Graph.k_core` is available.

The dominator tree from a given node can be obtained with :meth:`Graph.dominator`.

Bipartite graphs can be decomposed using :meth:`Graph.bipartite_projection`. The size of the projections can be computed using :meth:`Graph.bipartite_projection_size`.

Morphisms
+++++++++

|igraph| enables comparisons between graphs:

- :meth:`Graph.isomorphic`
- :meth:`Graph.isomorphic_vf2`
- :meth:`Graph.subisomorphic_vf2`
- :meth:`Graph.subisomorphic_lad`
- :meth:`Graph.get_isomorphisms_vf2`
- :meth:`Graph.get_subisomorphisms_vf2`
- :meth:`Graph.get_subisomorphisms_lad`
- :meth:`Graph.get_automorphisms_vf2`
- :meth:`Graph.count_isomorphisms_vf2`
- :meth:`Graph.count_subisomorphisms_vf2`
- :meth:`Graph.count_automorphisms_vf2`

Flow
++++

Flow is a characteristic of directed graphs. The following functions are available:

- :meth:`Graph.maxflow` between two nodes
- :meth:`Graph.maxflow_value` - similar to the previous one, but only the value is returned
- :meth:`Graph.gomory_hu_tree`

Flow and cuts are closely related, therefore you might find the following functions useful as well:

- :meth:`Graph.mincut` calculates the minimum cut between the source and target vertices
- :meth:`Graph.st_mincut` - as previous one, but returns a simpler data structure
- :meth:`Graph.mincut_value` - as previous one, but returns only the value
- :meth:`Graph.all_st_cuts`
- :meth:`Graph.all_st_mincuts`
- :meth:`Graph.edge_connectivity` or :meth:`Graph.edge_disjoint_paths` or :meth:`Graph.adhesion`
- :meth:`Graph.vertex_connectivity` or :meth:`Graph.cohesion`


---

## <a name="doc-source-api-index-rst"></a>File: doc\source\api\index.rst

.. include:: ../include/global.rst

.. currentmodule:: igraph


API reference
=============


---

## <a name="doc-source-conf-py"></a>File: doc\source\conf.py

```python
# -*- coding: utf-8 -*-
#
# igraph documentation build configuration file, created by
# sphinx-quickstart on Thu Jun 17 11:36:14 2010.
#
# This file is execfile()d with the current directory set to its containing dir.
#
# Note that not all possible configuration values are present in this
# autogenerated file.
#
# All configuration values have a default; values that are commented out
# serve to show the default.

from datetime import datetime

import sys
import os
import importlib
from pathlib import Path


# Check if we are inside readthedocs, the conf is quite different there
rtd_version = os.getenv("READTHEDOCS_VERSION", "")
rtd_version_type = os.getenv("READTHEDOCS_VERSION_TYPE", "unknown")

# Utility functions
# NOTE: these could be improved, esp by importing igraph, but that
# currently generates a conflict with pydoctor. It is funny because pydoctor's
# docs indeed import itself... perhaps there's a decent way to solve this.
def get_root_dir():
    """Get project root folder"""
    return str(Path(".").absolute().parent.parent)


def get_igraphdir():
    """Get igraph folder"""
    return Path(importlib.util.find_spec("igraph").origin).parent


def get_igraph_version():
    """Get igraph version"""
    if rtd_version and rtd_version_type == "tag":
        return rtd_version

    version_file = get_igraphdir() / "version.py"
    with open(version_file, "rt") as f:
        version_info = f.readline().rstrip("\n").split("=")[1].strip()[1:-1].split(", ")
    version = ".".join(version_info)

    return version


# -- General configuration -----------------------------------------------------

_igraph_dir = str(get_igraphdir())
_igraph_version = get_igraph_version()

# If your documentation needs a minimal Sphinx version, state it here.
# needs_sphinx = '1.0'

# Add any Sphinx extension module names here, as strings. They can be extensions
# coming with Sphinx (named 'sphinx.ext.*') or your custom ones.
extensions = [
    "sphinx.ext.coverage",
    "sphinx.ext.mathjax",
    "sphinx.ext.intersphinx",
    "sphinx_gallery.gen_gallery",
    #'sphinx_panels',
    "pydoctor.sphinx_ext.build_apidocs",
]

# If extensions (or modules to document with autodoc) are in another directory,
# add these directories to sys.path here. If the directory is relative to the
# documentation root, use os.path.abspath to make it absolute, like shown here.
# sys.path.append(os.path.abspath('.'))

# The suffix of source filenames.
source_suffix = ".rst"

# The encoding of source files.
# source_encoding = 'utf-8-sig'

# The master toctree document.
master_doc = "index"

# General information about the project.
project = "igraph"
copyright = "2010-{0}, The igraph development team".format(datetime.now().year)

# The version info for the project you're documenting, acts as replacement for
# |version| and |release|, also used in various other places throughout the
# built documents.
#
# The short X.Y version.
version = _igraph_version
# The full version, including alpha/beta/rc tags.
release = version

# The language for content autogenerated by Sphinx. Refer to documentation
# for a list of supported languages.
# language = None

# There are two options for replacing |today|: either, you set today to some
# non-false value, then it is used:
# today = ''
# Else, today_fmt is used as the format for a strftime call.
# today_fmt = '%B %d, %Y'

# List of patterns, relative to source directory, that match files and
# directories to ignore when looking for source files.
exclude_patterns = ["include/*.rst"]

# The reST default role (used for this markup: `text`) to use for all documents.
# default_role = None

# If true, '()' will be appended to :func: etc. cross-reference text.
# add_function_parentheses = True

# If true, the current module name will be prepended to all description
# unit titles (such as .. function::).
# add_module_names = True

# If true, sectionauthor and moduleauthor directives will be shown in the
# output. They are ignored by default.
# show_authors = False

# The name of the Pygments (syntax highlighting) style to use.
pygments_style = "sphinx"

# A list of ignored prefixes for module index sorting.
# modindex_common_prefix = []


# -- Options for HTML output ---------------------------------------------------

# Inspired by pydoctor's RTD page itself
# https://github.com/twisted/pydoctor/blob/master/docs/source/conf.py
html_theme = "sphinx_rtd_theme"
html_static_path = ["_static"]
html_css_files = ["custom.css"]

# The name for this set of Sphinx documents.  If None, it defaults to
# "<project> v<release> documentation".
# html_title = None

# A shorter title for the navigation bar.  Default is the same as html_title.
# html_short_title = None

# The name of an image file (relative to this directory) to place at the top
# of the sidebar.
# html_logo = "_static/logo-black.svg"

# The name of an image file (within the static path) to use as favicon of the
# docs.  This file should be a Windows icon file (.ico) being 16x16 or 32x32
# pixels large.
# html_favicon = None

# If not '', a 'Last updated on:' timestamp is inserted at every page bottom,
# using the given strftime format.
# html_last_updated_fmt = '%b %d, %Y'

# If true, SmartyPants will be used to convert quotes and dashes to
# typographically correct entities.
# html_use_smartypants = True

# Custom sidebar templates, maps document names to template names.
# html_sidebars = {}

# Additional templates that should be rendered to pages, maps page names to
# template names.
# html_additional_pages = {}

# If true, "Created using Sphinx" is shown in the HTML footer. Default is True.
# html_show_sphinx = True

# If true, "(C) Copyright ..." is shown in the HTML footer. Default is True.
# html_show_copyright = True

# If true, an OpenSearch description file will be output, and all pages will
# contain a <link> tag referring to it.  The value of this option must be the
# base URL from which the finished HTML is served.
# html_use_opensearch = ''

# If nonempty, this is the file name suffix for HTML files (e.g. ".xhtml").
# html_file_suffix = ''

# Output file base name for HTML help builder.
htmlhelp_basename = "igraphdoc"

# Integration with Read the Docs since RTD is not manipulating the Sphinx
# config files on its own any more.
# This is according to:
# https://about.readthedocs.com/blog/2024/07/addons-by-default/
html_baseurl = os.environ.get("READTHEDOCS_CANONICAL_URL", "")
html_context = {}
if os.environ.get("READTHEDOCS", "") == "True":
    html_context["READTHEDOCS"] = True

# -- Options for pydoctor ------------------------------------------------------


def get_pydoctor_html_outputdir(pydoctor_url_path):
    """Get HTML output dir for pydoctor"""
    # NOTE: obviously this is a little tricky, but it does work for both
    # the sphinx-build script and the python -m sphinx module calls. It works
    # locally, on github pages, and on RTD.
    return str(Path(sys.argv[-1]) / pydoctor_url_path.strip("/"))


# API docs relative to the rest of the docs, needed for pydoctor to play nicely
# with intersphinx (https://pypi.org/project/pydoctor/#pydoctor-21-2-0)
# NOTE: As of 2022 AD, pydoctor requires this to be a subfolder of the docs.
pydoctor_url_path = "api/"

pydoctor_args = [
    "--project-name=igraph",
    "--project-version=" + version,
    "--project-url=https://python.igraph.org",
    "--introspect-c-modules",
    "--docformat=epytext",
    "--intersphinx=https://docs.python.org/3/objects.inv",
    "--html-output=" + get_pydoctor_html_outputdir(pydoctor_url_path),
    "--html-viewsource-base=https://github.com/igraph/python-igraph/tree/main/src/igraph",
    "--project-base-dir=" + _igraph_dir,
    "--template-dir=" + get_root_dir() + "/doc/source/_pydoctor_templates",
    "--theme=readthedocs",
    _igraph_dir,
]
pydoctor_url_path = "/en/{rtd_version}/api"


# -- Options for sphinx-gallery ------------------------------------------------

sphinx_gallery_conf = {
    "examples_dirs": "../examples_sphinx-gallery",  # path to your example scripts
    "gallery_dirs": "tutorials",  # path to where to save gallery generated output
    "filename_pattern": "/",
    "matplotlib_animations": True,
    "remove_config_comments": True,
}

# -- Options for LaTeX output --------------------------------------------------

# The paper size ('letter' or 'a4').
# latex_paper_size = 'letter'

# The font size ('10pt', '11pt' or '12pt').
# latex_font_size = '10pt'

# Grouping the document tree into LaTeX files. List of tuples
# (source start file, target name, title, author, documentclass [howto/manual]).
latex_documents = [
    (
        "index",
        "igraph.tex",
        "igraph Documentation",
        "The igraph development team",
        "manual",
    ),
]

# The name of an image file (relative to this directory) to place at the top of
# the title page.
# latex_logo = None

# For "manual" documents, if this is true, then toplevel headings are parts,
# not chapters.
# latex_use_parts = False

# If true, show page references after internal links.
# latex_show_pagerefs = False

# If true, show URL addresses after external links.
# latex_show_urls = False

# Additional stuff for the LaTeX preamble.
# latex_preamble = ''

# Documents to append as an appendix to all manuals.
# latex_appendices = []

# If false, no module index is generated.
# latex_domain_indices = True


# -- Options for manual page output --------------------------------------------

# One entry per manual page. List of tuples
# (source start file, name, description, authors, manual section).
man_pages = [
    ("index", "igraph", "igraph Documentation", ["The igraph development team"], 1)
]


# -- Options for Epub output ---------------------------------------------------

# Bibliographic Dublin Core info.
epub_title = "igraph"
epub_author = "The igraph development team"
epub_publisher = "The igraph development team"
epub_copyright = "2010-2022, The igraph development team"

# The language of the text. It defaults to the language option
# or en if the language is not set.
# epub_language = ''

# The scheme of the identifier. Typical schemes are ISBN or URL.
# epub_scheme = ''

# The unique identifier of the text. This can be a ISBN number
# or the project homepage.
# epub_identifier = ''

# A unique identification for the text.
# epub_uid = ''

# HTML files that should be inserted before the pages created by sphinx.
# The format is a list of tuples containing the path and title.
# epub_pre_files = []

# HTML files shat should be inserted after the pages created by sphinx.
# The format is a list of tuples containing the path and title.
# epub_post_files = []

# A list of files that should not be packed into the epub file.
# epub_exclude_files = []

# The depth of the table of contents in toc.ncx.
# epub_tocdepth = 3


# -- Intersphinx ------------------------------------------------

intersphinx_mapping = {
    "numpy": ("https://numpy.org/doc/stable/", None),
    "scipy": ("https://docs.scipy.org/doc/scipy/", None),
    "matplotlib": ("https://matplotlib.org/stable", None),
    "pandas": ("https://pandas.pydata.org/pandas-docs/stable/", None),
    "networkx": ("https://networkx.org/documentation/stable/", None),
}

```

---

## <a name="doc-source-configuration-rst"></a>File: doc\source\configuration.rst

.. include:: include/global.rst

.. currentmodule:: igraph

=============
Configuration
=============
|igraph| includes customization options that can be set via the :class:`configuration.Configuration` object and can be preserved via a configuration file. This file is stored at ``~/.igraphrc`` by default on Linux and Mac OS X systems, and at ``C:\Documents and Settings\username\.igraphrc`` on Windows systems.

To modify config options and store the result to file for future reuse:

.. code-block:: python

    import igraph as ig

    # Set configuration variables
    ig.config["plotting.backend"] = "matplotlib"
    ig.config["plotting.palette"] = "rainbow"

    # Save configuration to default file location
    ig.config.save()

Once your configuration file exists, |igraph| will load its contents automatically upon import.

It is possible to keep multiple configuration files in nonstandard locations by passing an argument to ``config.save``, e.g. ``ig.config.save("/path/to/config/file")``. To load a specific config the next time you import igraph, use:

.. code-block:: python

   import igraph as ig
   ig.config.load("/path/to/config/file")


---

## <a name="doc-source-faq-rst"></a>File: doc\source\faq.rst

.. include:: include/global.rst

.. currentmodule:: igraph

==========================
Frequently asked questions
==========================

I tried to install |igraph| but got an error! What do I do?
-----------------------------------------------------------
First, look at our :doc:`installation instructions <install>` including the
troubleshooting section. If that does not solve your problem, reach out via
the `igraph forum <https://igraph.discourse.group/>`_. We'll try our best to
help you!


I've just installed |igraph|. What do I do now?
-----------------------------------------------
Take a peek at the :doc:`tutorials/quickstart`! You can then go through a few
more examples in our :ref:`gallery <gallery-of-examples>`, read detailed instructions on graph :doc:`generation <generation>`, :doc:`analysis <analysis>` and :doc:`visualisation <visualisation>`, and check out the full :doc:`API documentation <api/index>`.


I thought |igraph| was an R package, is this the same package?
--------------------------------------------------------------
|igraph| is a software library written in C with interfaces in various programming
languages such as R, Python, and Mathematica. Many functions will have similar names
and functionality across languages, but the matching is not perfect, so you will
occasionally find functions that are supported in one language but not another.
See the FAQ below for instructions about how to request a feature.


I would like to use |igraph| but don't know Python, what to do?
---------------------------------------------------------------
|igraph| can be used from multiple programming languages such as C, R, Python,
and Mathematica. While the exact function names differ a bit, most functionality
is shared, so if you can code any of them you can use |igraph|: just refer to
the installation instructions for the appropriate language on our
`homepage <https://igraph.org>`_.

If you are not familiar with programming at all, or if you don't know any Python
but would still like to use the Python interface for |igraph|, you should start by
learning Python first. There are many resources online including online classes,
videos, tutorials, etc. |igraph| does not use a lot of advanced Python-specific
tricks, so once you can use a standard module such as :mod:`pandas` or
:mod:`matplotlib`, |igraph| should be easy to pick up.


I would like to have a function for operation/algorithm X, can you add it?
--------------------------------------------------------------------------
We are continuously extending |igraph| to include new functionality, and
requests from our community are the best way to guide those efforts. Of
course, we are just a few folks so we cannot guarantee that each and every
obscure community detection algorithm will be included in the package.
Please open a new thread on our `forum <https://igraph.discourse.group/>`_
describing your request. If your request is to adapt an existing function
or specific piece of code, you can directly open a
`GitHub issue <https://github.com/igraph/python-igraph/issues>`_
(make sure a similar issue does not exist yet! - If it does, comment there
instead.)


What's the difference between |igraph| and similar packages (networkx, graph-tool)?
-----------------------------------------------------------------------------------
All those packages focus on graph/network analysis.

.. warning::
   The following differences and similarities are considered correct as of the time of writing (Jan 2022). If you identify incorrect or outdated information, please open a `Github issue <https://github.com/igraph/python-igraph/issues>`_ and we'll update it.

**Differences:**

 - |igraph| supports **multiple programming languages** (e.g. C, Python, R, Mathematica). `networkx`_ and `graph-tool`_ are Python only.
 - |igraph|'s core library is written in C, which makes it often faster than `networkx`_. `graph-tool`_ is written in heavily templated C++, so it can be as fast as |igraph| but supports fewer architectures. Compiling `graph-tool` can take much longer than |igraph| (hours versus around a minute).
 - |igraph| vertices are *ordered with contiguous numerical IDs, from 0 upwards*, and an *optional* "vertex name". `networkx`_ nodes are *defined* by their name and not ordered.
 - Same holds for edges, ordered with integer IDs in |igraph|, not so in `networkx`_.
 - |igraph| can plot graphs using :mod:`matplotlib` and has experimental support for `plotly`_, so it can produce animations, notebook widgets, and interactive plots (e.g. zoom, panning). `networkx`_ has excellent :mod:`matplotlib` support but no `plotly`_ support. `graph-tool`_ only supports static images via Cairo and GTK+.
 - In terms of design, |igraph| really shines when you have a relatively static network that you want to analyse, while it can struggle with very dynamic networks that gain and lose vertices and edges all the time. This might change in the near future as we improve |igraph|'s core C library. At the moment, `networkx`_ is probably better suited for simulating such highly dynamic graphs.

**Similarities:**

 - Many tasks can be achieved equally well with |igraph|, `graph-tool`_, and `networkx`_.
 - All can read and write a number of graph file formats.
 - All can visualize graphs, with different strengths and weaknesses.

.. note::
  |igraph| includes conversion functions from/to `networkx`_, so you can create and manipulate a network with |igraph| and later on convert it to `networkx`_ or `graph-tool`_ if you need. Vice versa, you can load a graph in `networkx`_ or `graph-tool`_ and convert the graph into an |igraph| object if you need more speed, a specific algorithm, matplotlib animations, etc. You can even use |igraph| to convert graphs from `networkx`_ to `graph-tool`_ and vice versa!



I would like to contribute to |igraph|, where do I start?
---------------------------------------------------------
Thank you for your enthusiasm! |igraph| is a great opportunity to give back
to the open source community or just learn about graphs. Depending on your
skills in software engineering, programming, communication, or data science
some tasks might be better suited than others.

If you want to code straight away, take a look at the
`GitHub issues <https://github.com/igraph/python-igraph/issues>`_ and see if
you find one that sounds easy enough and sparks your interest, and write a
message saying you're interested in taking it on. We'll reply ASAP and guide
you as of your next steps.

The C core library also has various `"theory issues" <https://github.com/igraph/igraph/labels/theory>`_. You can contribute to these issues without any programming
knowledge by researching graph literature or finding the solution to a graph
problem. Once the theory obstacle has been overcome, others can move on to the
coding part: a real team effort!

If none of those look feasible, or if you have a specific idea, or still if
you would like to contribute in other ways than pure programming, reach out
on our `forum <https://igraph.discourse.group/>`_ and we'll come up with
some ideas.


.. _networkx: https://networkx.org/documentation/stable/
.. _graph-tool: https://graph-tool.skewed.de/
.. _plotly: https://plotly.com/python/


---

## <a name="doc-source-generation-rst"></a>File: doc\source\generation.rst

.. include:: include/global.rst

.. _generation:

.. currentmodule:: igraph

Graph generation
================

The first step of most |igraph| applications is to generate a graph. This section will explain a number of ways to do that. Read the :doc:`api/index` for details on each function and class.

The :class:`Graph` class is the main object used to generate graphs::

    >>> from igraph import Graph

To copy a graph, use :meth:`Graph.copy`::

    >>> g_new = g.copy()

From nodes and edges
++++++++++++++++++++

Nodes are always numbered from 0 upwards. To create a generic graph with a specified number of nodes (e.g. 10) and a list of edges between them, you can use the generic constructor:

    >>> g = Graph(n=10, edges=[[0, 1], [2, 3]])

If not specified, the graph is undirected. To make a directed graph::

    >>> g = Graph(n=10, edges=[[0, 1], [2, 3]], directed=True)

To specify edge weights (or any other vertex/edge attributes), use dictionaries::

    >>> g = Graph(
    ...     n=4, edges=[[0, 1], [2, 3]],
    ...     edge_attrs={'weight': [0.1, 0.2]},
    ...     vertex_attrs={'color': ['b', 'g', 'g', 'y']}
    ... )

To create a bipartite graph from a list of types and a list of edges, use :meth:`Graph.Bipartite`.

From Python builtin structures (lists, tuples, dicts)
+++++++++++++++++++++++++++++++++++++++++++++++++++++
|igraph| supports a number of "conversion" methods to import graphs from Python builtin data structures such as dictionaries, lists and tuples:

 - :meth:`Graph.DictList`: from a list of dictionaries
 - :meth:`Graph.TupleList`: from a list of tuples
 - :meth:`Graph.ListDict`: from a dict of lists
 - :meth:`Graph.DictDict`: from a dict of dictionaries

Equivalent methods are available to export a graph, i.e. to convert a graph into
a representation that uses Python builtin data structures:

 - :meth:`Graph.to_dict_list`
 - :meth:`Graph.to_tuple_list`
 - :meth:`Graph.to_list_dict`
 - :meth:`Graph.to_dict_dict`

See the :doc:`api/index` of each function for details and examples.

From matrices
+++++++++++++

To create a graph from an adjacency matrix, use :meth:`Graph.Adjacency` or, for weighted matrices, :meth:`Graph.Weighted_Adjacency`::

    >>> g = Graph.Adjacency([[0, 1, 1], [0, 0, 0], [0, 0, 1]])

This graph is directed and has edges `[0, 1]`, `[0, 2]` and `[2, 2]` (a self-loop).

To create a bipartite graph from a bipartite adjacency matrix, use :meth:`Graph.Biadjacency`::

    >>> g = Graph.Biadjacency([[0, 1, 1], [1, 1, 0]])

From files
++++++++++

To load a graph from a file in any of the supported formats, use :meth:`Graph.Load`. For instance::

    >>> g = Graph.Load('myfile.gml', format='gml')

If you don't specify a format, |igraph| will try to figure it out or, if that fails, it will complain.

From external libraries
+++++++++++++++++++++++

|igraph| can read from and write to `networkx` and `graph-tool` graph formats::

    >>> g = Graph.from_networkx(nwx)

and

::

    >>> g = Graph.from_graph_tool(gt)

From pandas DataFrame(s)
++++++++++++++++++++++++

A common practice is to store edges in a `pandas.DataFrame`, where the two first columns are the source and target vertex ids,
and any additional column indicates edge attributes. You can generate a graph via :meth:`Graph.DataFrame`::

    >>> g = Graph.DataFrame(edges, directed=False)

It is possible to set vertex attributes at the same time via a separate DataFrame. The first column is assumed to contain all
vertex ids (including any vertices without edges) and any additional columns are vertex attributes::

    >>> g = Graph.DataFrame(edges, directed=False, vertices=vertices)

From a formula
++++++++++++++

To create a graph from a string formula, use :meth:`Graph.Formula`, e.g.::

    >>> g = Graph.Formula('D-A:B:F:G, A-C-F-A, B-E-G-B, A-B, F-G, H-F:G, H-I-J')

.. note:: This particular formula also assigns the 'name' attribute to vertices.

Complete graphs
+++++++++++++++

To create a complete graph, use :meth:`Graph.Full`::

    >>> g = Graph.Full(n=3)

where `n` is the number of nodes. You can specify directedness and whether self-loops are included::

    >>> g = Graph.Full(n=3, directed=True, loops=True)

A similar method, :meth:`Graph.Full_Bipartite`, generates a complete bipartite graph. Finally, the metho :meth:`Graph.Full_Citation` created the full citation graph, in which a vertex with index `i` has a directed edge to all vertices with index strictly smaller than `i`.

Tree and star
+++++++++++++

:meth:`Graph.Tree` can be used to generate regular trees, in which almost each vertex has the same number of children::

    >>> g = Graph.Tree(n=7, n_children=2)

creates a tree with seven vertices - of which four are leaves. The root (0) has two children (1 and 2), each of which has two children (the four leaves). Regular trees can be directed or undirected (default).

The method :meth:`Graph.Star` creates a star graph, which is a subtype of a tree.

Lattice
+++++++

:meth:`Graph.Lattice` creates a regular square lattice of the chosen size. For instance::

    >>> g = Graph.Lattice(dim=[3, 3], circular=False)

creates a 3×3 grid in two dimensions (9 vertices total). `circular` is used to connect each edge of the lattice back onto the other side, a process also known as "periodic boundary condition" that is sometimes helpful to smoothen out edge effects.

The one dimensional case (path graph or cycle graph) is important enough to deserve its own constructor :meth:`Graph.Ring`, which can be circular or not::

    >>> g = Graph.Ring(n=4, circular=False)

Graph Atlas
+++++++++++

The book ‘An Atlas of Graphs’ by Roland C. Read and Robin J. Wilson contains all unlabeled undirected graphs with up to seven vertices, numbered from 0 up to 1252. You can create any graph from this list by index with :meth:`Graph.Atlas`, e.g.::

    >>> g = Graph.Atlas(44)

The graphs are listed:

 - in increasing order of number of nodes;
 - for a fixed number of nodes, in increasing order of the number of edges;
 - for fixed numbers of nodes and edges, in increasing order of the degree sequence, for example 111223 < 112222;
 - for fixed degree sequence, in increasing number of automorphisms.

Famous graphs
+++++++++++++

A curated list of famous graphs, which are often used in the literature for benchmarking and other purposes, is available on the `igraph C core manual <https://igraph.org/c/doc/igraph-Generators.html#igraph_famous>`_. You can generate any graph in that list by name, e.g.::

    >>> g = Graph.Famous('Zachary')

will teach you some about martial arts.


Random graphs
+++++++++++++

Stochastic graphs can be created according to several different models or games:

 - Barabási-Albert model: :meth:`Graph.Barabasi`
 - Erdős-Rényi: :meth:`Graph.Erdos_Renyi`
 - Watts-Strogatz :meth:`Graph.Watts_Strogatz`
 - stochastic block model :meth:`Graph.SBM`
 - random tree :meth:`Graph.Tree_Game`
 - forest fire game :meth:`Graph.Forest_Fire`
 - random geometric graph :meth:`Graph.GRG`
 - growing :meth:`Graph.Growing_Random`
 - establishment game :meth:`Graph.Establishment`
 - preference, the non-growing variant of establishment :meth:`Graph.Preference`
 - asymmetric preference :meth:`Graph.Asymmetric_Prefernce`
 - recent degree :meth:`Graph.Recent_Degree`
 - k-regular (each node has degree k) :meth:`Graph.K_Regular`
 - non-growing graph with edge probabilities proportional to node fitnesses :meth:`Graph.Static_Fitness`
 - non-growing graph with prescribed power-law degree distribution(s) :meth:`Graph.Static_Power_Law`
 - random graph with a given degree sequence :meth:`Graph.Degree_Sequence`
 - bipartite :meth:`Graph.Random_Bipartite`

Other graphs
++++++++++++

Finally, there are some ways of generating graphs that are not covered by the previous sections:

 - Kautz graphs :meth:`Graph.Kautz`
 - De Bruijn graphs :meth:`Graph.De_Bruijn`
 - graphs from LCF notation :meth:`Graph.LCF`
 - small graphs of any "isomorphism class" :meth:`Graph.Isoclass`
 - graphs with a specified degree sequence :meth:`Graph.Realize_Degree_Sequence`


---

## <a name="doc-source-include-global-rst"></a>File: doc\source\include\global.rst

.. |igraph| replace:: *igraph*


---

## <a name="doc-source-index-rst"></a>File: doc\source\index.rst

.. igraph documentation master file, created by sphinx-quickstart on Thu Dec 11 16:02:35 2008.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. include:: include/global.rst

.. currentmodule:: igraph

.. raw:: html

    <style type="text/css">
    div.twocol {
        padding-left: 0;
        padding-right: 0;
        display: flex;
    }

    div.twocol > div {
        flex-grow: 1;
        padding: 0;
        margin-right: 20px;
    }
    </style>


python-igraph |release|
=======================
Python interface of `igraph`_, a fast and open source C library to manipulate and analyze graphs (aka networks). It can be used to:

 - Create, manipulate, and analyze networks.
 - Convert graphs from/to `networkx`_, `graph-tool`_ and many file formats.
 - Plot networks using `Cairo`_, `matplotlib`_, and `plotly`_.


Installation
============

.. container:: twocol

    .. container::

        Install using `pip <https://pypi.org/project/igraph>`__:

        .. code-block:: bash

            pip install igraph

    .. container::

        Install using `conda <https://docs.continuum.io/anaconda/>`__:

        .. code-block:: bash

            conda install -c conda-forge python-igraph

Further details are available in the :doc:`Installation Guide <install>`.

Documentation
=============

.. container:: twocol

    .. container::


       **Tutorials**

       - :doc:`Quick start <tutorials/quickstart>`
       - :doc:`Gallery of examples <tutorials/index>`
       - :doc:`Extended tutorial <tutorial>` (:doc:`Español <tutorial.es>`)

    .. container::

       **Detailed docs**

       - :doc:`Generation <generation>`
       - :doc:`Analysis <analysis>`
       - :doc:`Visualization <visualisation>`
       - :doc:`Configuration <configuration>`

.. container:: twocol

    .. container::

       **Reference**

       - :doc:`api/index`
       - `Source code <https://github.com/igraph/python-igraph>`_

    .. container::

       **Support**

       - :doc:`FAQs <faq>`
       - `Forum <https://igraph.discourse.group/>`_

Documentation for `python-igraph <= 0.10.1` is available on our `old website <https://igraph.org/python/versions/0.10.1>`_.

.. toctree::
   :maxdepth: 1
   :hidden:

   install
   tutorials/index
   tutorial
   tutorial.es
   api/index
   generation
   analysis
   visualisation
   configuration
   faq


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`


.. _igraph: https://igraph.org
.. _networkx: https://networkx.org/documentation/stable/
.. _graph-tool: https://graph-tool.skewed.de/
.. _Cairo: https://www.cairographics.org
.. _matplotlib: https://matplotlib.org
.. _plotly: https://plotly.com/python/

Citation
========

If you use igraph in your research, please cite

    Csardi, G., & Nepusz, T. (2006). The igraph software package for complex network research. InterJournal, Complex Systems, 1695.


---

## <a name="doc-source-install-rst"></a>File: doc\source\install.rst

.. include:: include/global.rst

.. Installing igraph

.. _installing-igraph:

===================
Installing |igraph|
===================

Binary package (recommended)
============================
It is recommended to install a binary package that includes both C core and Python interface. You can choose either of `PyPI <https://pypi.org/project/igraph/>`_ or `Conda <https://anaconda.org/conda-forge/python-igraph>`_. Linux users can also use their package manager.

PyPI
----
PyPI has installers for Windows, Linux, and macOS. We aim to provide binary packages for the three latest minor versions of Python 3.x.

To install the Python interface of |igraph| globally, use the following command
(you might need administrator/root privileges)::

  $ pip install igraph

If you prefer to install |igraph| in a user folder using a `virtual environment
<https://packaging.python.org/en/latest/tutorials/installing-packages/#creating-virtual-environments>`_, use the following commands instead::

  $ python -m venv my_environment
  $ source my_environment/bin/activate
  $ pip install igraph

As usual, if you do not want to activate the virtualenv, you can call the ``pip``
executable in it directly::

  $ python -m venv my_environment
  $ my_environment/bin/pip install igraph

Conda
-----
Packages are kindly provided by `conda-forge <https://conda-forge.org/>`_::

  $ conda install -c conda-forge python-igraph

Like virtualenv, Conda also offers virtual environments. If you prefer that option::

  $ conda create -n my_environment
  $ conda activate my_environment
  $ conda install -c conda-forge python-igraph

Package managers on Linux and other systems
-------------------------------------------
|igraph|'s Python interface and its dependencies are included in several package management
systems, including those of the most popular Linux distributions (Arch Linux,
Debian and Ubuntu, Fedora, etc.) as well as some cross-platform systems like
NixPkgs or MacPorts.

.. note:: |igraph| is updated quite often: if you need a more recent version than your
   package manager offers, use ``pip`` or ``conda`` as shown above. For bleeding-edge
   versions, compile from source (see below).

Compiling |igraph| from source
==============================
You might want to compile |igraph| to test a recently added feature ahead of release or
to install |igraph| on architectures not covered by our continuous development pipeline.

.. note:: In all cases, the Python interface needs to be compiled against
   a **matching** version of the |igraph| core C library. If you used ``git``
   to check out the source tree, ``git`` was probably smart enough to check out
   the matching version of igraph's C core as a submodule into
   ``vendor/source/igraph``. You can use ``git submodule update --init
   --recursive`` to check out the submodule manually, or you can run ``git
   submodule status`` to print the exact revision of igraph's C core that
   should be used with the Python interface.

Compiling using pip
-------------------

If you want the development version of |igraph|, call::

  $ pip install git+https://github.com/igraph/python-igraph

``pip`` is smart enough to download the sources from Github, initialize the
submodule for the |igraph| C core, compile it, and then compile the Python
interface against it and install it. As above, a virtual environment is
a commonly used sandbox to test experimental packages.

If you want the latest release from PyPI but prefer to (or have to) install from source, call::

  $ pip install --no-binary ':all:' igraph

.. note:: If there is no binary for your system anyway, you can just try without the ``--no-binary`` option and
   obtain the same result.

Compiling step by step
----------------------

This section should be rarely used in practice but explains how to compile and
install |igraph| step by step from a local checkout, i.e. _not_ relying on
``pip`` to fetch the sources. (You would still need ``pip`` to install from
source, or a PEP 517-compliant build frontend like
`build <https://pypa-build.readthedocs.io/en/stable/>`_ to build an installable
Python wheel.

First, obtain the bleeding-edge source code from Github::

  $ git clone https://github.com/igraph/python-igraph.git

or download a recent `release from PyPI <https://pypi.org/project/python-igraph/#files>`_ or from the
`Github releases page <https://github.com/igraph/python-igraph/releases/>`_. Decompress the archive if
needed.

Second, go into the folder::

  $ cd python-igraph

(it might have a slightly different name depending on the release).

Third, if you cloned the source from Github, initialize the ``git`` submodule for the |igraph| C core::

  $ git submodule update --init

.. note:: If you prefer to compile and link |igraph| against an existing |igraph| C core, for instance
   the one you installed with your package manager, you can skip the ``git`` submodule initialization step. If you
   downloaded a tarball, you also need to remove the ``vendor/source/igraph`` folder because the setup script
   will look for the vendored |igraph| copy first. However, a particular
   version of the Python interface is guaranteed to work only with the version
   of the C core that is bundled with it (or with the revision that the ``git``
   submodule points to).

Fourth, call ``pip`` to compile and install the package from source::

  $ pip install .

Alternatively, you can call ``build`` or another PEP 517-compliant build frontend
to build an installable Python wheel. Here we use `pipx <https://pipx.pypa.io>`_
to invoke ``build`` in a separate virtualenv::

  $ pipx run build

Testing your installation
-------------------------

Use ``tox`` or another standard test runner tool to run all the unit tests.
Here we use `pipx <https://pipx.pypa.io>`_ to invoke ``tox``::

  $ pipx run tox

You can also call ``tox`` directly from the root folder of the igraph source
tree if you already installed ``tox`` system-wide::

  $ tox

Troubleshooting
===============

Q: I am trying to install |igraph| on Windows but am getting DLL import errors
------------------------------------------------------------------------------
A: The most common reason for this error is that you do not have the Visual C++
Redistributable library installed on your machine. Python's own installer is
supposed to install it, but in case it was not installed on your system, you can
`download it from Microsoft <https://learn.microsoft.com/en-US/cpp/windows/latest-supported-vc-redist?view=msvc-170>`_.

Q: I am trying to use |igraph| but get errors about something called Cairo
----------------------------------------------------------------------------------
A: |igraph| by default uses a third-party called `Cairo <https://www.cairographics.org>`_ for plotting.
If Cairo is not installed on your computer, you might get an import error. This error is most commonly
encountered on Windows machines.

There are two solutions to this problem: installing Cairo or, if you are using a recent versions of
|igraph|, switching to the :mod:`matplotlib` plotting backend.

**1. Install Cairo**: As explained `here <https://pycairo.readthedocs.io/en/latest/getting_started.html>`_,
you need to install Cairo headers using your package manager (Linux) or `homebrew <https://brew.sh/>`_
(macOS) and then::

  $ pip install pycairo

To check if Cairo is installed correctly on your system, run the following example::

  >>> import igraph as ig
  >>> g = ig.Graph.Famous("petersen")
  >>> ig.plot(g)

If PyCairo was successfully installed, this will display a Petersen graph.

**2. Switch to matplotlib**: You can :doc:`configure <configuration>` |igraph| to use matplotlib
instead of Cairo. First, install it::

  $ pip install matplotlib

To use matplotlib for a single plot, create a :class:`matplotlib.figure.Figure` and
:class:`matplotlib.axes.Axes` beforehand (e.g. using :func:`matplotlib.pyplot.subplots`)::

  >>> import matplotlib.pyplot as plt
  >>> import igraph as ig
  >>> fig, ax = plt.subplots()
  >>> g = ig.Graph.Famous("petersen")
  >>> ig.plot(g, target=ax)
  >>> plt.show()

To use matplotlib for a whole session/notebook::

  >>> import matplotlib.pyplot as plt
  >>> import igraph as ig
  >>> ig.config["plotting.backend"] = "matplotlib"
  >>> g = ig.Graph.Famous("petersen")
  >>> ig.plot(g)
  >>> plt.show()

To preserve this preference across sessions/notebooks, you can store it in the default
configuration file used by |igraph|::

  >>> import igraph as ig
  >>> ig.config["plotting.backend"] = "matplotlib"
  >>> ig.config.save()

From now on, |igraph| will default to matplotlib for plotting.


---

## <a name="doc-source-requirements-txt"></a>File: doc\source\requirements.txt

pip
wheel
requests>=2.28.1

sphinx==7.4.7
sphinx-gallery>=0.14.0
sphinx-rtd-theme>=1.3.0
pydoctor>=23.4.0

numpy
scipy
pandas
matplotlib


---

## <a name="doc-source-tutorial-es-rst"></a>File: doc\source\tutorial.es.rst

.. include:: include/global.rst

.. _tutorial_es:

.. currentmodule:: igraph

==================
Tutorial (Español)
==================

Esta página es un tutorial detallado de las capacidades de |igraph| para Python. Para obtener una impresión rápida de lo que |igraph| puede hacer, consulte el :doc:`tutorials/quickstart`. Si aún no ha instalado |igraph|, siga las instrucciones de :doc:`install`.

.. note::
   Para el lector impaciente, vea la página :doc:`tutorials/index` para ejemplos cortos y autocontenidos.

Comenzar con |igraph|
=====================

La manera más común de usar |igraph| es como una importanción con nombre dentro de un ambiente de Python (por ejemplo, un simple shell de Python, a `IPython`_ shell, un `Jupyter`_ notebook o una instancia JupyterLab, `Google Colab <https://colab.research.google.com/>`_, o un `IDE <https://www.spyder-ide.org/>`_)::

  $ python
  Python 3.9.6 (default, Jun 29 2021, 05:25:02)
  [Clang 12.0.5 (clang-1205.0.22.9)] on darwin
  Type "help", "copyright", "credits" or "license" for more information.
  >>> import igraph as ig

Para llamar a funciones, es necesario anteponerles el prefijo ``ig`` (o el nombre que hayas elegido)::

  >>> import igraph as ig
  >>> print(ig.__version__)
  0.9.8

.. note::
   Es posible utilizar *importación con asterisco* para |igraph|::

    >>> from igraph import *

   pero en general se desaconseja <https://stackoverflow.com/questions/2386714/why-is-import-bad>`_.

Hay una segunda forma de iniciar |igraph|, que consiste en llamar al script :command:`igraph` desde tu terminal::

  $ igraph
  No configuration file, using defaults
  igraph 0.9.6 running inside Python 3.9.6 (default, Jun 29 2021, 05:25:02)
  Type "copyright", "credits" or "license" for more information.
  >>>

.. note::
   Para los usuarios de Windows encontrarán el script dentro del subdirectorio file:`scripts`
   de Python y puede que tengan que añadirlo manualmente a su ruta.

Este script inicia un intérprete de comandos apropiado (`IPython`_ o `IDLE <https://docs.python.org/3/library/idle.html>`_ si se encuentra, de lo contrario un intérprete de comandos Python puro) y utiliza *importación con asterisco* (véase más arriba). Esto es a veces conveniente para usar las funciones de |igraph|.

.. note::
   Puede especificar qué shell debe utilizar este script a través
   :doc:`configuration` de |igraph|.

Este tutorial asumirá que has importado igraph usando el de nombres ``ig``.

Creando un grafo
================

La forma más sencilla de crear un grafo es con el constructor :class:`Graph`. Para hacer un grafo vacío:

  >>> g = ig.Graph()

Para hacer un grafo con 10 nodos (numerados ``0`` to ``9``) y dos aristas que conecten los nodos ``0-1`` y ``0-5``::

  >>> g = ig.Graph(n=10, edges=[[0, 1], [0, 5]])

Podemos imprimir el grafo para obtener un resumen de sus nodos y aristas::

  >>> print(g)
  IGRAPH U--- 10 2 --
  + edges:
  0--1 0--5

Tenemos entonces: grafo no dirigido (**U**ndirected) con **10** vértices y **2** aristas, que se enlistan en la última parte. Si el grafo tiene un atributo "nombre", también se imprime.

.. note::
   ``summary`` es similar a ``print`` pero no enlista las aristas, lo cual
   es conveniente para grafos grandes con millones de aristas::

     >>> summary(g)
     IGRAPH U--- 10 2 --

Añadir y borrar vértices y aristas
==================================

Empecemos de nuevo con un grafo vacío. Para añadir vértices a un grafo existente, utiliza :meth:`Graph.add_vertices`::

  >>> g = ig.Graph()
  >>> g.add_vertices(3)

En |igraph|, los vértices se numeran siempre a partir de cero El número de un vértice es el *ID del vértice*. Un vértice puede tener o no un nombre.

Del mismo modo, para añadir aristas se utiliza :meth:`Graph.add_edges`::

  >>> g.add_edges([(0, 1), (1, 2)])

Las aristas se añaden especificando el vértice origen y el vértice destino de cada arista. Esta llamada añade dos aristas, una que conecta los vértices ``0`` y ``1``, y otra que conecta los vértices ``1`` y ``2``. Las aristas también se numeran a partir de cero (el *ID del arista*) y tienen un nombre opcional.

.. warning::

  Crear un grafo vacío y añadir vértices y aristas como se muestra aquí puede ser mucho más lento que crear un grafo con sus vértices y aristas como se ha demostrado anteriormente. Si la velocidad es una preocupación, deberías evitar especialmente añadir vértices y aristas *de uno en uno*. Si necesitas hacerlo de todos modos, puedes usar :meth:`Graph.add_vertex` y :meth:`Graph.add_edge`.

Si intentas añadir aristas a vértices con IDs no válidos (por ejemplo, intentas añadir una arista al vértice ``5`` cuando el grafo sólo tiene tres vértices), obtienes un error :exc:`igraph.InternalError`::

  >>> g.add_edges([(5, 4)])
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "/usr/lib/python3.10/site-packages/igraph/__init__.py", line 376, in add_edges
      res = GraphBase.add_edges(self, es)
  igraph._igraph.InternalError: Error at src/graph/type_indexededgelist.c:270: cannot add edges. -- Invalid vertex id

El mensaje intenta explicar qué ha fallado (``cannot add edges. -- Invalid
vertex id``) junto con la línea correspondiente del código fuente en la que se ha producido el error.

.. note::
   El rastreo completo, incluida la información sobre el código fuente, es útil cuando
   se informa de errores en nuestro
   `Página de problemas de GitHub <https://github.com/igraph/python-igraph/issues>`_. Por favor, inclúyalo
   completo si crea un nuevo asunto.

Añadamos más vértices y aristas a nuestro grafo::

  >>> g.add_edges([(2, 0)])
  >>> g.add_vertices(3)
  >>> g.add_edges([(2, 3), (3, 4), (4, 5), (5, 3)])
  >>> print(g)
  IGRAPH U---- 6 7 --
  + edges:
  0--1 1--2 0--2 2--3 3--4 4--5 3--5

Ahora tenemos un grafo no dirigido con 6 vértices y 7 aristas. Los IDs de los vértices y aristas son siempre *continuos*, por lo que si eliminas un vértice todos los vértices subsiguientes serán renumerados. Cuando se renumera un vértice, las aristas **no** se renumeran, pero sí sus vértices de origen y destino. Utilice :meth:`Graph.delete_vertices` y :meth:`Graph.delete_edges` para realizar estas operaciones. Por ejemplo, para eliminar la arista que conecta los vértices ``2-3``, obten sus IDs y luego eliminalos::

  >>> g.get_eid(2, 3)
  3
  >>> g.delete_edges(3)

Generar grafos
==============

|igraph| incluye generadores de grafos tanto deterministas como estocásticos. Los generadores *deterministas* producen el mismo grafo cada vez que se llama a la función, por ejemplo::

  >>> g = ig.Graph.Tree(127, 2)
  >>> summary(g)
  IGRAPH U--- 127 126 --

Utiliza :meth:`Graph.Tree` para generar un grafo regular en forma de árbol con 127 vértices, cada vértice con dos hijos (y un padre, por supuesto). No importa cuántas veces llames a :meth:`Graph.Tree`, el grafo generado será siempre el mismo si utilizas los mismos parámetros::

  >>> g2 = ig.Graph.Tree(127, 2)
  >>> g2.get_edgelist() == g.get_edgelist()
  True

El fragmento de código anterior también muestra el método :meth:`~Graph.get_edgelist()`, que devuelve una lista de vértices de origen y destino para todas las aristas, ordenados por el ID de la arista. Si imprimes los 10 primeros elementos, obtienes::

  >>> g2.get_edgelist()[:10]
  [(0, 1), (0, 2), (1, 3), (1, 4), (2, 5), (2, 6), (3, 7), (3, 8), (4, 9), (4, 10)]

Los generadores *estocásticos* producen un grafo diferente cada vez; por ejemplo, :meth:`Graph.GRG`::

  >>> g = ig.Graph.GRG(100, 0.2)
  >>> summary(g)
  IGRAPH U---- 100 516 --
  + attr: x (v), y (v)

.. note::
   `+ attr`` muestra atributos para vértices (v) y aristas (e), en este caso dos atributos de
   vértice y ningún atributo de arista.

Esto genera un grafo geométrico aleatorio: Se eligen *n* puntos de forma aleatoria y uniforme dentro del cuadrado unitario y los pares de puntos más cercanos entre sí respecto a una distancia predefinida *d* se conectan mediante una arista. Si se generan GRGs con los mismos parámetros, serán diferentes::

  >>> g2 = ig.Graph.GRG(100, 0.2)
  >>> g.get_edgelist() == g2.get_edgelist()
  False

Una forma un poco más relajada de comprobar si los grafos son equivalentes es mediante :meth:`~Graph.isomorphic()`::

  >>> g.isomorphic(g2)
  False

Comprobar por el isomorfismo puede llevar un tiempo en el caso de grafos grandes (en este caso, la respuesta puede darse rápidamente comprobando las distribuciones de grados de los dos grafos).

Establecer y recuperar atributos
================================

Como se ha mencionado anteriormente, en |igraph| cada vértice y cada arista tienen un ID numérico de ``0`` en adelante. Por lo tanto, la eliminación de vértices o aristas puede causar la reasignación de los ID de vértices y/o aristas. Además de los IDs, los vértices y aristas pueden tener *atributos* como un nombre, coordenadas para graficar, metadatos y pesos. El propio grafo puede tener estos atributos también (por ejemplo, un nombre, que se mostrará en ``print`` o :meth:`Graph.summary`). En cierto sentido, cada :class:`Graph`, vértice y arista pueden utilizarse como un diccionario de Python para almacenar y recuperar estos atributos.

Para demostrar el uso de los atributos, creemos una red social sencilla::

  >>> g = ig.Graph([(0,1), (0,2), (2,3), (3,4), (4,2), (2,5), (5,0), (6,3), (5,6)])

Cada vértice representa una persona, por lo que queremos almacenar nombres, edades y géneros::

  >>> g.vs["name"] = ["Alice", "Bob", "Claire", "Dennis", "Esther", "Frank", "George"]
  >>> g.vs["age"] = [25, 31, 18, 47, 22, 23, 50]
  >>> g.vs["gender"] = ["f", "m", "f", "m", "f", "m", "m"]
  >>> g.es["is_formal"] = [False, False, True, True, True, False, True, False, False]

:attr:`Graph.vs` y :attr:`Graph.es` son la forma estándar de obtener una secuencia de todos los vértices y aristas respectivamente. El valor debe ser una lista con la misma longitud que los vértices (para :attr:`Graph.vs`) o aristas (para :attr:`Graph.es`). Esto asigna un atributo a *todos* los vértices/aristas a la vez.

Para asignar o modificar un atributo para un solo vértice/borde, puedes hacer lo siguiente::

 >>> g.es[0]["is_formal"] = True

De hecho, un solo vértice se representa mediante la clase :class:`Vertex`, y una sola arista mediante :class:`Edge`. Ambos, junto con :class:`Graph`, pueden ser tecleados como un diccionario para establecer atributos, por ejemplo, para añadir una fecha al grafo::

  >>> g["date"] = "2009-01-10"
  >>> print(g["date"])
  2009-01-10

Para recuperar un diccionario de atributos, puedes utilizar :meth:`Graph.attributes`, :meth:`Vertex.attributes` y :meth:`Edge.attributes`.

Además, cada :class:`Vertex` tiene una propiedad especial, :attr:`Vertex.index`, que se utiliza para averiguar el ID de un vértice. Cada :class:`Edge` tiene :attr:`Edge.index` más dos propiedades adicionales, :attr:`Edge.source` y :attr:`Edge.target`, que se utilizan para encontrar los IDs de los vértices conectados por esta arista. Para obtener ambas propiedades a la vez, puedes utilizar :attr:`Edge.tuple`.

Para asignar atributos a un subconjunto de vértices o aristas, puedes utilizar el corte::

  >>> g.es[:1]["is_formal"] = True

La salida de ``g.es[:1]`` es una instancia de :class:`~seq.EdgeSeq`, mientras que :class:`~seq.VertexSeq` es la clase equivalente que representa subconjuntos de vértices.

Para eliminar atributos, puedes utilizar ``del``, por ejemplo::

  >>> g.vs[3]["foo"] = "bar"
  >>> g.vs["foo"]
  [None, None, None, 'bar', None, None, None]
  >>> del g.vs["foo"]
  >>> g.vs["foo"]
  Traceback (most recent call last):
    File "<stdin>", line 25, in <module>
  KeyError: 'Attribute does not exist'

.. warning::
   Los atributos pueden ser objetos arbitrarios de Python, pero si está guardando grafos en un
   archivo, sólo se conservarán los atributos de cadena ("string") y numéricos. Consulte el
   módulo :mod:`pickle` de la biblioteca estándar de Python si busca una forma de guardar otros
   tipos de atributos. Puede hacer un pickle de sus atributos individualmente, almacenarlos como
   cadenas y guardarlos, o puedes hacer un pickle de todo el :class:`Graph` si sabes que quieres
   cargar el grafo en Python.


Propiedades estructurales de los grafos
=======================================

Además de las funciones simples de manipulación de grafos y atributos descritas anteriormente, |igraph| proporciona un amplio conjunto de métodos para calcular varias propiedades estructurales de los grafos. Está más allá del alcance de este tutorial documentar todos ellos, por lo que esta sección sólo presentará algunos de ellos con fines ilustrativos. Trabajaremos con la pequeña red social que construimos en la sección anterior.

Probablemente, la propiedad más sencilla en la que se puede pensar es el "grado del vértice" (:dfn:`vertex degree`). El grado de un vértice es igual al número de aristas incidentes a él. En el caso de los grafos dirigidos, también podemos definir el ``grado de entrada`` (:dfn:`in-degree`, el número de aristas que apuntan hacia el vértice) y el ``grado de salida`` (:dfn:`out-degree`, el número de aristas que se originan en el vértice)::

  >>> g.degree()
  [3, 1, 4, 3, 2, 3, 2]

Si el grafo fuera dirigido, habríamos podido calcular los grados de entrada y salida por separado utilizando ``g.degree(mode="in")`` y ``g.degree(mode="out")``. También puedes usar un único ID de un vértice o una lista de ID de los vértices a :meth:`~Graph.degree` si quieres calcular los grados sólo para un subconjunto de vértices::

  >>> g.degree(6)
  2
  >>> g.degree([2,3,4])
  [4, 3, 2]

Este procedimiento se aplica a la mayoría de las propiedades estructurales que |igraph| puede calcular. Para las propiedades de los vértices, los métodos aceptan un ID o una lista de IDs de los vértices (y si se omiten, el valor predeterminado es el conjunto de todos los vértices). Para las propiedades de las aristas, los métodos también aceptan un único ID de o una lista de IDs de aristas. En lugar de una lista de IDs, también puedes proporcionar una instancia :class:`VertexSeq` o una instancia :class:`EdgeSeq` apropiadamente. Más adelante, en el próximo capítulo "consulta de vértices y aristas", aprenderás a restringirlos exactamente a los vértices o aristas que quieras.

.. note::

   Para algunos casos, no tiene sentido realizar el calculo sólo para unos pocos vértices o
   aristas en lugar de todo el grafo, ya que de todas formas se tardaría el mismo tiempo. En
   este caso, los métodos no aceptan IDs de vértices o aristas, pero se puede restringir la
   lista resultante más tarde usando operadores estándar de indexación y de corte. Un ejemplo de
   ello es la centralidad de los vectores propios (:meth:`Graph.evcent()`)

Además de los grados, |igraph| incluye rutinas integradas para calcular muchas otras propiedades de centralidad, como la intermediación de vértices y aristas o el PageRank de Google (:meth:`Graph.pagerank`), por nombrar algunas. Aquí sólo ilustramos la interrelación de aristas::

  >>> g.edge_betweenness()
  [6.0, 6.0, 4.0, 2.0, 4.0, 3.0, 4.0, 3.0. 4.0]

Ahora también podemos averiguar qué conexiones tienen la mayor centralidad de intermediación
con un poco de magia de Python::

  >>> ebs = g.edge_betweenness()
  >>> max_eb = max(ebs)
  >>> [g.es[idx].tuple for idx, eb in enumerate(ebs) if eb == max_eb]
  [(0, 1), (0, 2)]

La mayoría de las propiedades estructurales también pueden ser obtenidas para un subconjunto de vértices o aristas o para un solo vértice o arista llamando al método apropiado de la clase :class:`VertexSeq` o :class:`EdgeSeq` de interés::

  >>> g.vs.degree()
  [3, 1, 4, 3, 2, 3, 2]
  >>> g.es.edge_betweenness()
  [6.0, 6.0, 4.0, 2.0, 4.0, 3.0, 4.0, 3.0. 4.0]
  >>> g.vs[2].degree()
  4

Busqueda de vértices y aristas basada en atributos
==================================================

Selección de vértices y aristas
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Tomando como ejemplo la red social anterirormente creada, te gustaría averiguar quién tiene el mayor grado o centralidad de intermediación. Puedes hacerlo con las herramientas presentadas hasta ahora y conocimientos básicos de Python, pero como es una tarea común seleccionar vértices y aristas basándose en atributos o propiedades estructurales, |igraph| te ofrece una forma más fácil de hacerlo::

  >>> g.vs.select(_degree=g.maxdegree())["name"]
  ['Claire']

La sintaxis puede parecer un poco rara a primera vista, así que vamos a tratar de interpretarla paso a paso. meth:`~VertexSeq.select` es un método de :class:`VertexSeq` y su único propósito es filtrar un :class:`VertexSeq` basándose en las propiedades de los vértices individuales. La forma en que filtra los vértices depende de sus argumentos posicionales y de palabras clave. Los argumentos posicionales (los que no tienen un nombre explícito como ``_degree`` siempre se procesan antes que los argumentos de palabra clave de la siguiente manera:

- Si el primer argumento posicional es ``None``, se devuelve una secuencia vacía (que no contiene vértices)::

    >>> seq = g.vs.select(None)
    >>> len(seq)
    0

- Si el primer argumento posicional es un objeto invocable (es decir, una función, un método vinculado o cualquier cosa que se comporte como una función), el objeto será llamado para cada vértice que esté actualmente en la secuencia. Si la función devuelve ``True``, el vértice será incluido, en caso contrario será excluido::

    >>> graph = ig.Graph.Full(10)
    >>> only_odd_vertices = graph.vs.select(lambda vertex: vertex.index % 2 == 1)
    >>> len(only_odd_vertices)
    5

- Si el primer argumento posicional es un iterable (es decir, una lista, un generador o cualquier cosa sobre la que se pueda iterar), *debe* devolver enteros y estos enteros se considerarán como índices del conjunto de vértices actual (que *no* es necesariamente todo el grafo). Sólo se incluirán en el conjunto de vértices filtrados los vértices que coincidan con los índices dados. Los numero flotantes, las cadenas y los ID de vértices no válidos seran omitidos::

    >>> seq = graph.vs.select([2, 3, 7])
    >>> len(seq)
    3
    >>> [v.index for v in seq]
    [2, 3, 7]
    >>> seq = seq.select([0, 2])         # filtering an existing vertex set
    >>> [v.index for v in seq]
    [2, 7]
    >>> seq = graph.vs.select([2, 3, 7, "foo", 3.5])
    >>> len(seq)
    3

- Si el primer argumento posicional es un número entero, se espera que todos los demás argumentos sean también números enteros y se interpretan como índices del conjunto de vértices actual. Esto solo es "azucar sintáctica", se podría conseguir un efecto equivalente pasando una lista como primer argumento posicional, de esta forma se pueden omitir los corchetes::

    >>> seq = graph.vs.select(2, 3, 7)
    >>> len(seq)
    3

Los argumentos clave ("keyword argument") pueden utilizarse para filtrar los vértices en función de sus atributos o sus propiedades estructurales. El nombre de cada argumento clave consiste como máximo de dos partes: el nombre del atributo o propiedad estructural y el operador de filtrado. El operador puede omitirse; en ese caso, automáticamente se asume el operador de igualdad. Las posibilidades son las siguientes (donde *name* indica el nombre del atributo o propiedad):

================ ================================================================
Keyword argument Significado
================ ================================================================
``name_eq``      El valor del atributo/propiedad debe ser *igual* a
---------------- ----------------------------------------------------------------
``name_ne``      El valor del atributo/propiedad debe *no ser igual* a
---------------- ----------------------------------------------------------------
``name_lt``      El valor del atributo/propiedad debe ser *menos* que
---------------- ----------------------------------------------------------------
``name_le``      El valor del atributo/propiedad debe ser *inferior o igual a*
---------------- ----------------------------------------------------------------
``name_gt``      El valor del atributo/propiedad debe ser *mayor que*
---------------- ----------------------------------------------------------------
``name_ge``      El valor del atributo/propiedad debe ser *mayor o igual a*
---------------- ----------------------------------------------------------------
``name_in``      El valor del atributo/propiedad debe estar *incluido en*, el cual tiene que ser
                 una secuencia en este caso
---------------- ----------------------------------------------------------------
``name_notin``   El valor del atributo/propiedad debe *no estar incluido en* ,
                 el cual tiene que ser una secuencia en este caso
================ ================================================================

Por ejemplo, el siguiente comando te da las personas menores de 30 años en nuestra red social imaginaria::

  >>> g.vs.select(age_lt=30)

.. note::
   Debido a las restricciones sintácticas de Python, no se puede utilizar la sintaxis más
   sencilla de ``g.vs.select(edad < 30)``, ya que en Python sólo se permite que aparezca el
   operador de igualdad en una lista de argumentos.

Para ahorrarte algo de tecleo, puedes incluso omitir el método :meth:`~VertexSeq.select` si
desea::

  >>> g.vs(age_lt=30)

También hay algunas propiedades estructurales especiales para seleccionar los aristas:

- Utilizando ``_source`` or ``_from`` en función de los vértices de donde se originan las aristas. Por ejemplo, para seleccionar todas las aristas procedentes de Claire (que tiene el índice de vértice 2)::

    >>> g.es.select(_source=2)

- Usar los filtros ``_target`` o ``_to`` en base a los vértices de destino. Esto es diferente de ``_source`` and ``_from`` si el grafo es dirigido.

- ``_within`` toma un objeto :class:`VertexSeq` o un set de vértices y selecciona todos los aristas que se originan y terminan en un determinado set de vértices. Por ejemplo, la siguiente expresión selecciona todos los aristas entre Claire (índice 2), Dennis (índice 3) y Esther (índice 4)::

    >>> g.es.select(_within=[2,3,4])

- ``_between`` toma una tupla que consiste en dos objetos :class:`VertexSeq` o una listas que contienen los indices de los vértices o un objeto :class:`Vertex` y selecciona todas las aristas que se originan en uno de los conjuntos y terminan en el otro. Por ejemplo, para seleccionar todas las aristas que conectan a los hombres con las mujeres::

    >>> men = g.vs.select(gender="m")
    >>> women = g.vs.select(gender="f")
    >>> g.es.select(_between=(men, women))

Encontrar un solo vértice o arista con algunas propiedades
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

En muchos casos buscamos un solo vértice o arista de un grafo con algunas propiedades, sin importar cuál de las coincidencias se devuelve, ya sea si éxiste mútliples coincidencias, o bien sabemos de antemano que sólo habrá una coincidencia. Un ejemplo típico es buscar vértices por su nombre en la propiedad ``name``. Los objetos :class:`VertexSeq` y :class:`EdgeSeq` proveen el método :meth:`~VertexSeq.find` para esos casos. Esté método funciona de manera similar a :meth:`~VertexSeq.select`, pero devuelve solo la primer coincidencia si hay multiples resultados, y señala una excepción si no se encuentra ninguna coincidencia. Por ejemplo, para buscar el vértice correspondiente a Claire, se puede hacer lo siguiente::

  >>> claire = g.vs.find(name="Claire")
  >>> type(claire)
  igraph.Vertex
  >>> claire.index
  2

La búsqueda de un nombre desconocido dará lugar a una excepción::

  >>> g.vs.find(name="Joe")
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  ValueError: no such vertex

Búsqueda de vértices por nombres
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Buscar vértices por su nombre es una operación muy común, y normalmente es mucho más fácil recordar los nombres de los vértices de un grafo que sus IDs. Para ello, |igraph| trata el atributo ``name`` de los vértices de forma especial; se indexan de forma que los vértices se pueden buscar por sus nombre. Para hacer las cosas incluso más fácil, |igraph| acepta nombres de vértices (casi) en cualquier lugar dónde se espere especificar un ID de un vérice, e incluso, acepta colecciones (tuplas,listas,etc.) de nombres de vértices dónde sea que se esperé una lista de IDs de vértices. Por ejemplo, puedes buscar el grado (número de conexiones) de Dennis de la siguiente manera::

  >>> g.degree("Dennis")
  3

o alternativamente::

  >>> g.vs.find("Dennis").degree()
  3

El mapeo entre los nombres de los vértices y los IDs es mantenido de forma transparente por |igraph| en segundo plano; cada vez que el grafo cambia, |igraph| también actualiza el mapeo interno. Sin embargo, la singularidad de los nombres de los vértices *no* se impone; puedes crear fácilmente un grafo en el que dos vértices tengan el mismo nombre, pero igraph sólo devolverá uno de ellos cuando los busques por nombres, el otro sólo estará disponible por su índice.

Tratar un grafo como una matriz de adyacencia
=============================================

La matriz de adyacencia es otra forma de formar un grafo. En la matriz de adyacencia, las filas y columnas están etiquetadas por los vértices del grafo: los elementos de la matriz indican si los vértices *i* y *j* tienen una arista común (*i, j*). La matriz de adyacencia del grafo de nuestra red social imaginaria es::

  >>> g.get_adjacency()
  Matrix([
    [0, 1, 1, 0, 0, 1, 0],
    [1, 0, 0, 0, 0, 0, 0],
    [1, 0, 0, 1, 1, 1, 0],
    [0, 0, 1, 0, 1, 0, 1],
    [0, 0, 1, 1, 0, 0, 0],
    [1, 0, 1, 0, 0, 0, 1],
    [0, 0, 0, 1, 0, 1, 0]
  ])

Por ejemplo, Claire (``[1, 0, 0, 1, 1, 1, 0]``) está directamente conectada con Alice (que tiene el índice 0), Dennis (índice 3), Esther (índice 4) y Frank (índice 5), pero no con Bob (índice 1) ni con George (índice 6).

Diseños ("layouts") y graficar
==============================

Un grafo es un objeto matemático abstracto sin una representación específica en el espacio 2D o 3D. Esto significa que cuando queremos visualizar un grafo, tenemos que encontrar primero un trazado de los vértices a las coordenadas en el espacio bidimensional o tridimensional, preferiblemente de una manera que sea agradable a la vista. Una rama separada de la teoría de grafos, denominada dibujo de grafos, trata de resolver este problema mediante varios algoritmos de disposición de grafos. igraph implementa varios algoritmos de diseño y también es capaz de dibujarlos en la pantalla o en un archivo PDF, PNG o SVG utilizando la `libreria Cairo <https://www.cairographics.org>`_.

.. important::

   Para seguir los ejemplos de esta sección, se requieren de la librería Cairo en Python o
   matplotlib.

Algoritmos de diseños ("layouts")
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Los métodos de diseño en |igraph| se encuentran en el objeto :class:`Graph`, y siempre comienzan con ``layout_``. La siguiente tabla los resume:

==================================== =============== =============================================
Method name                          Short name      Algorithm description
==================================== =============== =============================================
``layout_circle``                    ``circle``,     Disposición determinista que coloca los
                                     ``circular``    vértices en un círculo
------------------------------------ --------------- ---------------------------------------------
``layout_drl``                       ``drl``         El algoritmo [Distributed Recursive Layout]
                                                     para grafos grandes
------------------------------------ --------------- ---------------------------------------------
``layout_fruchterman_reingold``      ``fr``          El algoritmo dirigido Fruchterman-Reingold
------------------------------------ --------------- ---------------------------------------------
``layout_fruchterman_reingold_3d``   ``fr3d``,       El algoritmo dirigido Fruchterman-Reingold
                                     ``fr_3d``       en tres dimensiones
------------------------------------ --------------- ---------------------------------------------
``layout_kamada_kawai``              ``kk``          El algoritmo dirigido Kamada-Kawai
------------------------------------ --------------- ---------------------------------------------
``layout_kamada_kawai_3d``           ``kk3d``,       El algoritmo dirigido Kamada-Kawai
                                     ``kk_3d``       en tres dimensiones
------------------------------------ --------------- ---------------------------------------------
``layout_lgl``                       ``large``,      El algoritmo [Large Graph Layout] para
                                     ``lgl``,        grafos grandes
                                     ``large_graph``
------------------------------------ --------------- ---------------------------------------------
``layout_random``                    ``random``      Coloca los vértices de forma totalmente aleatoria
------------------------------------ --------------- ---------------------------------------------
``layout_random_3d``                 ``random_3d``   Coloca los vértices de forma totalmente aleatoria en 3D
------------------------------------ --------------- ---------------------------------------------
``layout_reingold_tilford``          ``rt``,         Diseño de árbol de Reingold-Tilford, útil
                                     ``tree``        para grafos (casi) arbóreos
------------------------------------ --------------- ---------------------------------------------
``layout_reingold_tilford_circular`` ``rt_circular`` Diseño de árbol de Reingold-Tilford con una
                                                     post-transformación de coordenadas polares,
                                     ``tree``        útil para grafos (casi) arbóreos
------------------------------------ --------------- ---------------------------------------------
``layout_sphere``                    ``sphere``,     Disposición determinista que coloca los vértices
                                     ``spherical``,  de manera uniforme en la superficie de una esfera
                                     ``circular_3d``
==================================== =============== =============================================

.. _Distributed Recursive Layout: https://www.osti.gov/doecode/biblio/54626
.. _Large Graph Layout: https://sourceforge.net/projects/lgl/

Los algoritmos de diseño pueden ser llamados directamente o utilizando :meth:`~Graph.layout`::

  >>> layout = g.layout_kamada_kawai()
  >>> layout = g.layout("kamada_kawai")

El primer argumento del método :meth:`~Graph.layout` debe ser el nombre corto del algoritmo de diseño (mirar la tabla anterior). Todos los demás argumentos posicionales y de palabra clave se pasan intactos al método de diseño elegido. Por ejemplo, las dos llamadas siguientes son completamente equivalentes::

  >>> layout = g.layout_reingold_tilford(root=[2])
  >>> layout = g.layout("rt", [2])

Los métodos de diseño devuelven un objeto :class:`~layout.Layout` que se comporta principalmente como una lista de listas. Cada entrada de la lista en un objeto :class:`~layout.Layout` corresponde a un vértice en el grafo original y contiene las coordenadas del vértice en el espacio 2D o 3D. Los objetos :class:`~layout.Layout` también contienen algunos métodos útiles para traducir, escalar o rotar las coordenadas en un lote. Sin embargo, la principal utilidad de los objetos :class:`~layout.Layout` es que puedes pasarlos a la función :func:`~drawing.plot` junto con el grafo para obtener un dibujo en 2D.

Dibujar un grafo utilizando un diseño ("layout")
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Por ejemplo, podemos trazar nuestra red social imaginaria con el algoritmo de distribución Kamada-Kawai de la siguiente manera::

  >>> layout = g.layout("kk")
  >>> ig.plot(g, layout=layout)

Esto debería abrir un visor de imágenes externo que muestre una representación visual de la red, algo parecido a lo que aparece en la siguiente figura (aunque la colocación exacta de los nodos puede ser diferente en su máquina, ya que la disposición no es determinista):

.. figure:: figures/tutorial_social_network_1.png
   :alt: The visual representation of our social network (Cairo backend)
   :align: center

Nuestra red social con el algoritmo de distribución Kamada-Kawai

Si prefiere utilizar `matplotlib`_ como motor de trazado, cree un eje y utilice el argumento ``target``::

  >>> import matplotlib.pyplot as plt
  >>> fig, ax = plt.subplots()
  >>> ig.plot(g, layout=layout, target=ax)

.. figure:: figures/tutorial_social_network_1_mpl.png
   :alt: The visual representation of our social network (matplotlib backend)
   :align: center

Hmm, esto no es demasiado bonito hasta ahora. Una adición trivial sería usar los nombres como etiquetas de los vértices y colorear los vértices según el género. Las etiquetas de los vértices se toman del atributo ``label`` por defecto y los colores de los vértices se determinan por el atributo ``color``::

  >>> g.vs["label"] = g.vs["name"]
  >>> color_dict = {"m": "blue", "f": "pink"}
  >>> g.vs["color"] = [color_dict[gender] for gender in g.vs["gender"]]
  >>> ig.plot(g, layout=layout, bbox=(300, 300), margin=20)  # Cairo backend
  >>> ig.plot(g, layout=layout, target=ax)  # matplotlib backend

Tenga en cuenta que aquí simplemente estamos reutilizando el objeto de diseño anterior, pero también hemos especificado que necesitamos un gráfico más pequeño (300 x 300 píxeles) y un margen mayor alrededor del grafo para que quepan las etiquetas (20 píxeles). El resultado es:

.. figure:: figures/tutorial_social_network_2.png
   :alt: The visual representation of our social network - with names and genders
   :align: center

Nuestra red social - con nombres como etiquetas y géneros como colores

y para matplotlib:

.. figure:: figures/tutorial_social_network_2_mpl.png
   :alt: The visual representation of our social network - with names and genders
   :align: center

En lugar de especificar las propiedades visuales como atributos de vértices y aristas, también puedes darlas como argumentos a :func:`~drawing.plot`::

  >>> color_dict = {"m": "blue", "f": "pink"}
  >>> ig.plot(g, layout=layout, vertex_color=[color_dict[gender] for gender in g.vs["gender"]])

Este último enfoque es preferible si quiere mantener las propiedades de la representación visual de su gráfico separadas del propio gráfico. Puedes simplemente crear un diccionario de Python que contenga los argumentos que contenga las palabras clave que pasarias a la función :func:`~drawing.plot` y luego usar el doble asterisco (``**``) para pasar tus atributos de estilo específicos a :func:`~drawing.plot`::

  >>> visual_style = {}
  >>> visual_style["vertex_size"] = 20
  >>> visual_style["vertex_color"] = [color_dict[gender] for gender in g.vs["gender"]]
  >>> visual_style["vertex_label"] = g.vs["name"]
  >>> visual_style["edge_width"] = [1 + 2 * int(is_formal) for is_formal in g.es["is_formal"]]
  >>> visual_style["layout"] = layout
  >>> visual_style["bbox"] = (300, 300)
  >>> visual_style["margin"] = 20
  >>> ig.plot(g, **visual_style)

El gráfico final muestra los vínculos formales con líneas gruesas y los informales con líneas finas:

.. figure:: figures/tutorial_social_network_3.png
   :alt: The visual representation of our social network - with names, genders and formal ties
   :align: center

   Nuestra red social - también muestra qué vínculos son formales

Para resumirlo todo: hay propiedades especiales de vértices y aristas que corresponden a la representación visual del grafo. Estos atributos anulan la configuración por defecto de |igraph| (es decir, el color, el peso, el nombre, la forma, el diseño, etc.). Las dos tablas siguientes resumen los atributos visuales más utilizados para los vértices y las aristas, respectivamente:

Atributos de los vértices que controlan los gráficos
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

=============== ====================== ==========================================
Attribute name  Keyword argument       Purpose
=============== ====================== ==========================================
``color``       ``vertex_color``       Color del vertice
--------------- ---------------------- ------------------------------------------
``font``        ``vertex_font``        Familia tipográfica del vértice
--------------- ---------------------- ------------------------------------------
``label``       ``vertex_label``       Etiqueta del vértice.
--------------- ---------------------- ------------------------------------------
``label_angle`` ``vertex_label_angle`` Define la posición de las etiquetas de los
                                       vértices, en relación con el centro de los
                                       mismos. Se interpreta como un ángulo en
                                       radianes, cero significa 'a la derecha'.
--------------- ---------------------- ------------------------------------------
``label_color`` ``vertex_label_color`` Color de la etiqueta del vértice
--------------- ---------------------- ------------------------------------------
``label_dist``  ``vertex_label_dist``  Distancia de la etiqueta del vértice,
                                       en relación con el tamaño del vértice
--------------- ---------------------- ------------------------------------------
``label_size``  ``vertex_label_size``  Tamaño de letra de la etiqueta de vértice
--------------- ---------------------- ------------------------------------------
``order``       ``vertex_order``       Orden de dibujo de los vértices. Vértices
                                       con un parámetro de orden menor se
                                       dibujarán primero.
--------------- ---------------------- ------------------------------------------
``shape``       ``vertex_shape``       La forma del vértice,. Algunas formas:
                                       ``rectangle``, ``circle``, ``hidden``,
                                       ``triangle-up``, ``triangle-down``. Ver
                                       :data:`drawing.known_shapes`.
--------------- ---------------------- ------------------------------------------
``size``        ``vertex_size``        El tamaño del vértice en pixels
=============== ====================== ==========================================

Atributos de las aristas que controlan los gráficos
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

=============== ====================== ==========================================
Attribute name  Keyword argument       Purpose
=============== ====================== ==========================================
``color``       ``edge_color``         Color de la arista.
--------------- ---------------------- ------------------------------------------
``curved``      ``edge_curved``        La curvatura de la arista. Valores positivos
                                       corresponden a aristas curvadas en sentido
                                       contrario a las manecillas del reloj, valores
                                       negativos lo contrario. Una curvatura cero
                                       representa aristas rectas. ``True`` significa
                                       una curvatura de 0.5, ``False`` es una
                                       curvatura de cero.
--------------- ---------------------- ------------------------------------------
``font``        ``edge_font``          Familia tipográfica del arista.
--------------- ---------------------- ------------------------------------------
``arrow_size``  ``edge_arrow_size``    Tamaño (longitud)  de la punta de flecha del
                                       arista si el grafo es dirigido, relativo a
                                       15 pixels.
--------------- ---------------------- ------------------------------------------
``arrow_width`` ``edge_arrow_width``   El ancho de las flechas. Relativo a 10
                                       pixels.
--------------- ---------------------- ------------------------------------------
``loop_size``   ``edge_loop_size``     Tamaño de los bucles. Puede ser negativo
                                       para escalar con el tamaño del vertice
                                       correspondiente. Este atributo no
                                       es utilizado para otras aristas. Este
                                       atributo sólo existe en el backend
                                       matplotlib.
--------------- ---------------------- ------------------------------------------
``width``       ``edge_width``         Anchura del borde en píxeles.
--------------- ---------------------- ------------------------------------------
``label``       ``edge_label``         Si se especifica, añade una etiqueta al borde.
--------------- ---------------------- ------------------------------------------
``background``  ``edge_background``    Si se especifica, añade una caja rectangular
                                       alrededor de la etiqueta de borde (solo en
                                       matplotlib).
--------------- ---------------------- ------------------------------------------
``align_label`` ``edge_align_label``   Si es verdadero, gira la etiqueta de la
                                       arista de forma que se alinee con la
                                       dirección de la arista. Las etiquetas que
                                       estarían al revés se voltean (sólo matplotlib).
=============== ====================== ==========================================

Argumentos genéricos de ``plot()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Estos ajustes se pueden especificar como argumentos de palabra clave a la función ``plot`` para controlar la apariencia general del gráfico.

================ ================================================================
Keyword argument Purpose
================ ================================================================
``autocurve``    Determinación automática de la curvatura de las aristas en grafos
                 con múltiples aristas. El estandar es ``True`` para grafos
                 con menos de 10000 aristas y  ``False`` para el caso contrario.
---------------- ----------------------------------------------------------------
``bbox``         La caja delimitadora del gráfico. Debe ser una tupla que contenga
                 la anchura y la altura deseadas del gráfico. Por default el gráfico
                 tiene 600 pixels de ancho y 600 pixels de largo.
---------------- ----------------------------------------------------------------
``layout``       El diseño que se va a utilizar. Puede ser una instancia de ``layout``
                 una lista de tuplas que contengan coordenadas X-Y, o el nombre
                 un algoritmo de diseño. El valor por defecto es ``auto``, que
                 selecciona un algoritmo de diseño automáticamente basado en el tamaño
                 y la conectividad del grafo.
---------------- ----------------------------------------------------------------
``margin``       La cantidad de espacio vacío debajo, encima, a la izquierda y
                 a la derecha del gráfico.
================ ================================================================

Especificación de colores en los gráficos
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

|igraph| entiende las siguientes especificaciones de color siempre que espera un color (por ejemplo, colores de aristas, vértices o etiquetas en los respectivos atributos):

***Nombres de colores X11***

Consulta la `lista de nombres de colores X11 <https://en.wikipedia.org/wiki/X11_color_names>`_ en Wikipedia para ver la lista completa. Los nombres de los colores no distinguen entre mayúsculas y minúsculas en |igraph|, por lo que ``"DarkBLue"`` puede escribirse también como ``"darkblue"``.

***Especificación del color en la sintaxis CSS***

Se trata de una cadena según uno de los siguientes formatos (donde *R*, *G* y *B* denotan los componentes rojo, verde y azul, respectivamente):

-   ``#RRGGBB``, los componentes van de 0 a 255 en formato hexadecimal. Ejemplo: ``"#0088ff"``
-   ``#RGB``, los componentes van de 0 a 15 en formato hexadecimal. Ejemplo: ``"#08f"``
-   ``rgb(R, G, B)``, los componentes van de 0 a 255 o de 0% a 100%. Ejemplo: ``"rgb(0, 127, 255)"`` o ``"rgb(0%, 50%, 100%)"``.

Guardar gráficos
^^^^^^^^^^^^^^^^

|igraph| puede usarse para crear gráficos de calidad de publicación solicitando  la función :func:`~drawing.plot` que guarde el gráfico en un archivo en lugar de mostrarlo en pantalla. Para ello, basta con pasar el nombre del archivo destino como argumento adicional después del grafo mismo. El formato preferido se deduce de la extensión. |igraph| puede guardar en cualquier cosa que soporte Cairo, incluyendo archivos SVG, PDF y PNG. Los archivos SVG o PDF pueden ser convertidos posteriormente al formato PostScript (``.ps``) o PostScript encapsulado (``.eps``) si lo prefieres, mientras que los archivos PNG pueden ser convertidos a TIF (``.tif``)::

  >>> ig.plot(g, "social_network.pdf", **visual_style)

Si estas usando matplotlib, puedes guardar el gŕafico como de costumbre::

  >>> fig, ax = plt.subplots()
  >>> ig.plot(g, **visual_style)
  >>> fig.savefig("social_network.pdf")

Muchos formatos de archivos son admitidos por matplotlib.

|igraph| y el mundo exterior
============================

Ningún módulo de grafos estaría completo sin algún tipo de funcionalidad de importación/exportación que permita al paquete comunicarse con programas y kits de herramientas externos. |igraph| no es una excepción: proporciona funciones para leer los formatos de grafos más comunes y para guardar objetos :class:`Graph` en archivos que obedezcan estas especificaciones de formato. La siguiente tabla resume los formatos que igraph puede leer o escribir:

================ ============= ============================ =============================
Format           Short name    Reader method                Writer method
================ ============= ============================ =============================
Adjacency list   ``lgl``       :meth:`Graph.Read_Lgl`       :meth:`Graph.write_lgl`
(a.k.a. `LGL`_)
---------------- ------------- ---------------------------- -----------------------------
Adjacency matrix ``adjacency`` :meth:`Graph.Read_Adjacency` :meth:`Graph.write_adjacency`
---------------- ------------- ---------------------------- -----------------------------
DIMACS           ``dimacs``    :meth:`Graph.Read_DIMACS`    :meth:`Graph.write_dimacs`
---------------- ------------- ---------------------------- -----------------------------
DL               ``dl``        :meth:`Graph.Read_DL`        not supported yet
---------------- ------------- ---------------------------- -----------------------------
Edge list        ``edgelist``, :meth:`Graph.Read_Edgelist`  :meth:`Graph.write_edgelist`
                 ``edges``,
                 ``edge``
---------------- ------------- ---------------------------- -----------------------------
`GraphViz`_      ``graphviz``, not supported yet            :meth:`Graph.write_dot`
                 ``dot``
---------------- ------------- ---------------------------- -----------------------------
GML              ``gml``       :meth:`Graph.Read_GML`       :meth:`Graph.write_gml`
---------------- ------------- ---------------------------- -----------------------------
GraphML          ``graphml``   :meth:`Graph.Read_GraphML`   :meth:`Graph.write_graphml`
---------------- ------------- ---------------------------- -----------------------------
Gzipped GraphML  ``graphmlz``  :meth:`Graph.Read_GraphMLz`  :meth:`Graph.write_graphmlz`
---------------- ------------- ---------------------------- -----------------------------
LEDA             ``leda``      not supported yet            :meth:`Graph.write_leda`
---------------- ------------- ---------------------------- -----------------------------
Labeled edgelist ``ncol``      :meth:`Graph.Read_Ncol`      :meth:`Graph.write_ncol`
(a.k.a. `NCOL`_)
---------------- ------------- ---------------------------- -----------------------------
`Pajek`_ format  ``pajek``,    :meth:`Graph.Read_Pajek`     :meth:`Graph.write_pajek`
                 ``net``
---------------- ------------- ---------------------------- -----------------------------
Pickled graph    ``pickle``    :meth:`Graph.Read_Pickle`    :meth:`Graph.write_pickle`
================ ============= ============================ =============================

.. _GraphViz: https://www.graphviz.org
.. _LGL: https://lgl.sourceforge.net/#FileFormat
.. _NCOL: https://lgl.sourceforge.net/#FileFormat
.. _Pajek: http://mrvar.fdv.uni-lj.si/pajek/

Como ejercicio, descarga la representación gráfica del conocido `Estudio del club de karate de Zacarías <https://en.wikipedia.org/wiki/Zachary%27s_karate_club>`_ en formato graphml. Dado que se trata de un archivo GraphML, debe utilizar el método de lectura GraphML de la tabla anterior (asegúrese de utilizar la ruta adecuada al archivo descargado)::

  >>> karate = ig.Graph.Read_GraphML("zachary.graphml")
  >>> ig.summary(karate)
  IGRAPH UNW- 34 78 -- Zachary's karate club network

Si quieres convertir el mismo grafo a, digamos, el formato de Pajek, puedes hacerlo con el método de la tabla anterior::

  >>> karate.write_pajek("zachary.net")

.. note::
   La mayoría de los formatos tienen sus propias limitaciones; por ejemplo, no todos pueden
   almacenar atributos. Tu mejor opción es probablemente GraphML o GML si quieres guardar los
   grafos de |igraph| en un formato que pueda ser leído desde un paquete externo y quieres
   preservar los atributos numéricos y de cadena. La lista de aristas y NCOL también están bien
   si no tienes atributos (aunque NCOL soporta nombres de vértices y pesos de aristas). Si no
   quieres utilizar grafos fuera de |igraph|, pero quieres almacenarlos para una sesión
   posterior, el formato de grafos ``pickled`` te garantza que obtendras exactamente el mismo
   grafo. El formato de grafos ``pickled`` usa el modulo ``pickle`` de Python para guardar y
   leer grafos.

También existen dos métodos de ayuda: :func:`read` es un punto de entrada genérico para los métodos de lectura que intenta deducir el formato adecuado a partir de la extensión del archivo. :meth:`Graph.write` es lo contrario de :func:`read`: permite guardar un grafo en el que el formato preferido se deduce de nuevo de la extensión. La detección del formato de :func:`read` y :meth:`Graph.write` se puede anular mediante el argumento ``format`` de la palabra clave ("keyword"), la cual acepta los nombres cortos de los otros formatos de la tabla anterior::

  >>> karate = ig.load("zachary.graphml")
  >>> karate.write("zachary.net")
  >>> karate.write("zachary.my_extension", format="gml")

Dónde ir a continuación
=======================

Este tutorial sólo ha arañado la superficie de lo que |igraph| puede hacer. Los planes a largo plazo son ampliar este tutorial para convertirlo en una documentación adecuada de estilo manual para igraph en los próximos capítulos. Un buen punto de partida es la documentación de la clase `Graph`. Si te quedas atascado, intenta preguntar primero en nuestro `Discourse group`_ - quizás haya alguien que pueda ayudarte inmediatamente.

.. _Discourse group: https://igraph.discourse.group
.. _matplotlib: https://matplotlib.org/
.. _IPython: https://ipython.readthedocs.io/en/stable/
.. _Jupyter: https://jupyter.org/


---

## <a name="doc-source-tutorial-rst"></a>File: doc\source\tutorial.rst

.. include:: include/global.rst

.. _tutorial:

.. currentmodule:: igraph

========
Tutorial
========

This page is a detailed tutorial of |igraph|'s Python capabilities. To get an quick impression of what |igraph| can do, check out the :doc:`tutorials/quickstart`. If you have not installed |igraph| yet, follow the section titled :doc:`install`.

.. note::
   For the impatient reader, see the :doc:`tutorials/index` page for short, self-contained examples.

Starting |igraph|
=================

The most common way to use |igraph| is as a named import within a Python environment (e.g. a bare Python shell, an `IPython`_ shell, a `Jupyter`_ notebook or JupyterLab instance, `Google Colab <https://colab.research.google.com/>`_, or an `IDE <https://www.spyder-ide.org/>`_)::

  $ python
  Python 3.9.6 (default, Jun 29 2021, 05:25:02)
  [Clang 12.0.5 (clang-1205.0.22.9)] on darwin
  Type "help", "copyright", "credits" or "license" for more information.
  >>> import igraph as ig

To call functions, you need to prefix them with ``ig`` (or whatever name you chose)::

  >>> import igraph as ig
  >>> print(ig.__version__)
  0.9.8

.. note::
   It is possible to use *star imports* for |igraph|::

    >>> from igraph import *

   but it is `generally discouraged <https://stackoverflow.com/questions/2386714/why-is-import-bad>`_.

There is a second way to start |igraph|, which is to call the script :command:`igraph` from your terminal::

  $ igraph
  No configuration file, using defaults
  igraph 0.9.6 running inside Python 3.9.6 (default, Jun 29 2021, 05:25:02)
  Type "copyright", "credits" or "license" for more information.
  >>>

.. note::
  Windows users will find the script inside the :file:`scripts` subdirectory of Python
  and might have to add it manually to their path.

This script starts an appropriate shell (`IPython`_ or `IDLE <https://docs.python.org/3/library/idle.html>`_ if found, otherwise a pure Python shell) and uses star imports (see above). This is sometimes convenient to play with |igraph|'s functions.

.. note::
   You can specify which shell should be used by this script via |igraph|'s
   :doc:`configuration` file.

This tutorial will assume you have imported igraph using the namespace ``ig``.

Creating a graph
================

The simplest way to create a graph is the :class:`Graph` constructor. To make an empty graph::

  >>> g = ig.Graph()

To make a graph with 10 nodes (numbered ``0`` to ``9``) and two edges connecting nodes ``0-1`` and ``0-5``::

  >>> g = ig.Graph(n=10, edges=[[0, 1], [0, 5]])

We can print the graph to get a summary of its nodes and edges::

  >>> print(g)
  IGRAPH U--- 10 2 --
  + edges:
  0--1 0--5

This means: **U** ndirected graph with **10** vertices and **2** edges, with the exact edges listed out. If the graph has a `name` attribute, it is printed as well.

.. note::

   |igraph| also has a :func:`igraph.summary()` function, which is similar to ``print()`` but it does not list the edges. This is convenient for large graphs with millions of edges::

     >>> ig.summary(g)
     IGRAPH U--- 10 2 --


Adding/deleting vertices and edges
==================================
Let's start from the empty graph again. To add vertices to an existing graph, use :meth:`Graph.add_vertices`::

  >>> g = ig.Graph()
  >>> g.add_vertices(3)

In |igraph|, vertices are always numbered up from zero. The number of a vertex is called the *vertex ID*. A vertex might or might not have a name.

Similarly, to add edges use :meth:`Graph.add_edges`::

  >>> g.add_edges([(0, 1), (1, 2)])

Edges are added by specifying the source and target vertex for each edge. This call added two edges, one connecting vertices ``0`` and ``1``, and one connecting vertices ``1`` and ``2``. Edges are also numbered up from zero (the *edge ID*) and have an optional name.

.. warning::

   Creating an empty graph and adding vertices and edges as shown here can be much slower
   than creating a graph with its vertices and edges as demonstrated earlier. If speed is
   of concern, you should especially avoid adding vertices and edges *one at a time*. If you
   need to do it anyway, you can use :meth:`Graph.add_vertex` and :meth:`Graph.add_edge`.


If you try to add edges to vertices with invalid IDs (i.e., you try to add an edge to vertex ``5`` when the graph has only three vertices), you get an :exc:`igraph.InternalError` exception::

  >>> g.add_edges([(5, 4)])
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "/usr/lib/python3.10/site-packages/igraph/__init__.py", line 376, in add_edges
      res = GraphBase.add_edges(self, es)
  igraph._igraph.InternalError: Error at src/graph/type_indexededgelist.c:270: cannot add edges. -- Invalid vertex id

The message tries to explain what went wrong (``cannot add edges. -- Invalid
vertex id``) along with the corresponding line in the source code where the error
occurred.

.. note::
   The whole traceback, including info on the source code, is useful when
   reporting bugs on our
   `GitHub issue page <https://github.com/igraph/python-igraph/issues>`_. Please include it
   if you create a new issue!


Let us add some more vertices and edges to our graph::

  >>> g.add_edges([(2, 0)])
  >>> g.add_vertices(3)
  >>> g.add_edges([(2, 3), (3, 4), (4, 5), (5, 3)])
  >>> print(g)
  IGRAPH U---- 6 7 --
  + edges:
  0--1 1--2 0--2 2--3 3--4 4--5 3--5

We now have an undirected graph with 6 vertices and 7 edges. Vertex and edge IDs are
always *continuous*, so if you delete a vertex all subsequent vertices will be renumbered.
When a vertex is renumbered, edges are **not** renumbered, but their source and target
vertices will. Use :meth:`Graph.delete_vertices` and :meth:`Graph.delete_edges` to perform
these operations. For instance, to delete the edge connecting vertices ``2-3``, get its
ID and then delete it::

  >>> g.get_eid(2, 3)
  3
  >>> g.delete_edges(3)

Generating graphs
=================

|igraph| includes both deterministic and stochastic graph generators (see :doc:`generation`).
*Deterministic* generators produce the same graph every time you call the fuction, e.g.::

  >>> g = ig.Graph.Tree(127, 2)
  >>> summary(g)
  IGRAPH U--- 127 126 --

uses :meth:`Graph.Tree` to generate a regular tree graph with 127 vertices, each vertex
having two children (and one parent, of course). No matter how many times you call
:meth:`Graph.Tree`, the generated graph will always be the same if you use the same
parameters::

  >>> g2 = ig.Graph.Tree(127, 2)
  >>> g2.get_edgelist() == g.get_edgelist()
  True

The above code snippet also shows you that the :meth:`~Graph.get_edgelist()` method,
which returns a list of source and target vertices for all edges, sorted by edge ID.
If you print the first 10 elements, you get::

  >>> g2.get_edgelist()[:10]
  [(0, 1), (0, 2), (1, 3), (1, 4), (2, 5), (2, 6), (3, 7), (3, 8), (4, 9), (4, 10)]

*Stochastic* generators produce a different graph each time, e.g. :meth:`Graph.GRG`::

  >>> g = ig.Graph.GRG(100, 0.2)
  >>> summary(g)
  IGRAPH U---- 100 516 --
  + attr: x (v), y (v)

.. note:: ``+ attr`` shows attributes for vertices (v) and edges (e), in this case two
   vertex attributes and no edge attributes.

This generates a geometric random graph: *n* points are chosen randomly and
uniformly inside the unit square and pairs of points closer to each other than a predefined
distance *d* are connected by an edge. If you generate GRGs with the same parameters, they
will be different::

  >>> g2 = ig.Graph.GRG(100, 0.2)
  >>> g.get_edgelist() == g2.get_edgelist()
  False

A slightly looser way to check if the graphs are equivalent is via :meth:`~Graph.isomorphic()`::

  >>> g.isomorphic(g2)
  False

Checking for isomorphism can take a while for large graphs (in this case, the
answer can quickly be given by checking the degree distributions of the two graphs).


Setting and retrieving attributes
=================================
As mentioned above, vertices and edges of a graph in |igraph| have numeric IDs from ``0`` upwards. Deleting vertices or edges can therefore cause reassignments of vertex and/or edge IDs. In addition to IDs, vertices and edges can have *attributes* such as a name, coordinates for plotting, metadata, and weights. The graph itself can have such attributes too (e.g. a name, which will show in ``print`` or :meth:`Graph.summary`). In a sense, every :class:`Graph`, vertex and edge can be used as a Python dictionary to store and retrieve these attributes.

To demonstrate the use of attributes, let us create a simple social network::

  >>> g = ig.Graph([(0,1), (0,2), (2,3), (3,4), (4,2), (2,5), (5,0), (6,3), (5,6)])

Each vertex represents a person, so we want to store names, ages and genders::

  >>> g.vs["name"] = ["Alice", "Bob", "Claire", "Dennis", "Esther", "Frank", "George"]
  >>> g.vs["age"] = [25, 31, 18, 47, 22, 23, 50]
  >>> g.vs["gender"] = ["f", "m", "f", "m", "f", "m", "m"]
  >>> g.es["is_formal"] = [False, False, True, True, True, False, True, False, False]

:attr:`Graph.vs` and :attr:`Graph.es` are the standard way to obtain a sequence of all
vertices and edges, respectively. Just like a Python dictionary, we can set each property
using square brackets. The value must be a list with the same length as the vertices (for
:attr:`Graph.vs`) or edges (for :attr:`Graph.es`). This assigns an attribute to *all* vertices/edges at once.

To assign or modify an attribute for a single vertex/edge, you can use indexing::

  >>> g.es[0]["is_formal"] = True

In fact, a single vertex is represented via the class :class:`Vertex`, and a single edge via :class:`Edge`. Both of them plus :class:`Graph` can all be keyed like a dictionary to set attributes, e.g. to add a date to the graph::

  >>> g["date"] = "2009-01-10"
  >>> print(g["date"])
  2009-01-10

To retrieve a dictionary of attributes, you can use :meth:`Graph.attributes`, :meth:`Vertex.attributes`, and :meth:`Edge.attributes`.

Furthermore, each :class:`Vertex` has a special property, :attr:`Vertex.index`, that is used to find out the ID of a vertex. Each :class:`Edge` has :attr:`Edge.index` plus two additional properties, :attr:`Edge.source` and :attr:`Edge.target`, that are used to find the IDs of the vertices connected by this edge. To get both at once as a tuple, you can use :attr:`Edge.tuple`.

To assign attributes to a subset of vertices or edges, you can use slicing::

  >>> g.es[:1]["is_formal"] = True

The output of ``g.es[:1]`` is an instance of :class:`~seq.EdgeSeq`, whereas :class:`~seq.VertexSeq` is the equivalent class representing subsets of vertices.

To delete attributes, use the Python keyword ``del``, e.g.::

  >>> g.vs[3]["foo"] = "bar"
  >>> g.vs["foo"]
  [None, None, None, 'bar', None, None, None]
  >>> del g.vs["foo"]
  >>> g.vs["foo"]
  Traceback (most recent call last):
    File "<stdin>", line 25, in <module>
  KeyError: 'Attribute does not exist'


.. warning::
   Attributes can be arbitrary Python objects, but if you are saving graphs to a
   file, only string and numeric attributes will be kept. See the :mod:`pickle` module in
   the standard Python library if you are looking for a way to save other attribute types.
   You can either pickle your attributes individually, store them as strings and save them,
   or you can pickle the whole :class:`Graph` if you know that you want to load the graph
   back into Python only.



Structural properties of graphs
===============================

Besides the simple graph and attribute manipulation routines described above,
|igraph| provides a large set of methods to calculate various structural properties
of graphs. It is beyond the scope of this tutorial to document all of them, hence
this section will only introduce a few of them for illustrative purposes.
We will work on the small social network we built in the previous section.

Probably the simplest property one can think of is the :dfn:`vertex degree`. The
degree of a vertex equals the number of edges adjacent to it. In case of directed
networks, we can also define :dfn:`in-degree` (the number of edges pointing towards
the vertex) and :dfn:`out-degree` (the number of edges originating from the vertex).
|igraph| is able to calculate all of them using a simple syntax::

  >>> g.degree()
  [3, 1, 4, 3, 2, 3, 2]

If the graph was directed, we would have been able to calculate the in- and out-degrees
separately using ``g.degree(mode="in")`` and ``g.degree(mode="out")``. You can
also pass a single vertex ID or a list of vertex IDs to :meth:`~Graph.degree` if you
want to calculate the degrees for only a subset of vertices::

  >>> g.degree(6)
  2
  >>> g.degree([2,3,4])
  [4, 3, 2]

This calling convention applies to most of the structural properties |igraph| can
calculate. For vertex properties, the methods accept a vertex ID or a list of vertex IDs
(and if they are omitted, the default is the set of all vertices). For edge properties,
the methods accept a single edge ID or a list of edge IDs. Instead of a list of IDs,
you can also supply a :class:`VertexSeq` or an :class:`EdgeSeq` instance appropriately.
Later in the :ref:`next chapter <querying_vertices_and_edges>`, you will learn how to
restrict them to exactly the vertices or edges you want.

.. note::

  For some measures, it does not make sense to calculate them only for a few vertices
  or edges instead of the whole graph, as it would take the same time anyway. In this
  case, the methods won't accept vertex or edge IDs, but you can still restrict the
  resulting list later using standard list indexing and slicing operators. One such
  example is eigenvector centrality (:meth:`Graph.evcent()`).

Besides degree, |igraph| includes built-in routines to calculate many other centrality
properties, including vertex and edge betweenness
(:meth:`Graph.betweenness <GraphBase.betweenness>`,
:meth:`Graph.edge_betweenness <GraphBase.edge_betweenness>`) or Google's PageRank
(:meth:`Graph.pagerank`) just to name a few. Here we just illustrate edge betweenness::

  >>> g.edge_betweenness()
  [6.0, 6.0, 4.0, 2.0, 4.0, 3.0, 4.0, 3.0. 4.0]

Now we can also figure out which connections have the highest betweenness centrality
with some Python magic::

  >>> ebs = g.edge_betweenness()
  >>> max_eb = max(ebs)
  >>> [g.es[idx].tuple for idx, eb in enumerate(ebs) if eb == max_eb]
  [(0, 1), (0, 2)]

Most structural properties can also be retrieved for a subset of vertices or edges
or for a single vertex or edge by calling the appropriate method on the
:class:`VertexSeq`, :class:`EdgeSeq`, :class:`Vertex` or :class:`Edge` object of
interest::

  >>> g.vs.degree()
  [3, 1, 4, 3, 2, 3, 2]
  >>> g.es.edge_betweenness()
  [6.0, 6.0, 4.0, 2.0, 4.0, 3.0, 4.0, 3.0. 4.0]
  >>> g.vs[2].degree()
  4

.. _querying_vertices_and_edges:

Querying vertices and edges based on attributes
===============================================

Selecting vertices and edges
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Imagine that in a given social network, you would like to find out who has the largest
degree or betweenness centrality. You can do that with the tools presented so far and
some basic Python knowledge, but since it is a common task to select vertices and edges
based on attributes or structural properties, |igraph| gives you an easier way to do that::

  >>> g.vs.select(_degree=g.maxdegree())["name"]
  ['Claire']

The syntax may seem a little bit awkward for the first sight, so let's try to interpret
it step by step. :meth:`~VertexSeq.select` is a method of :class:`VertexSeq` and its
sole purpose is to filter a :class:`VertexSeq` based on the properties of individual
vertices. The way it filters the vertices depends on its positional and keyword
arguments. Positional arguments (the ones without an explicit name like
``_degree`` above) are always processed before keyword arguments as follows:

- If the first positional argument is ``None``, an empty sequence (containing no
  vertices) is returned::

    >>> seq = g.vs.select(None)
    >>> len(seq)
    0

- If the first positional argument is a callable object (i.e., a function, a bound
  method or anything that behaves like a function), the object will be called for
  every vertex that's currently in the sequence. If the function returns ``True``,
  the vertex will be included, otherwise it will be excluded::

    >>> graph = ig.Graph.Full(10)
    >>> only_odd_vertices = graph.vs.select(lambda vertex: vertex.index % 2 == 1)
    >>> len(only_odd_vertices)
    5

- If the first positional argument is an iterable (i.e., a list, a generator or
  anything that can be iterated over), it *must* return integers and these integers
  will be considered as indices into the current vertex set (which is *not* necessarily
  the whole graph). Only those vertices that match the given indices will be included
  in the filtered vertex set. Floats, strings, invalid vertex IDs will silently be
  ignored::

    >>> seq = graph.vs.select([2, 3, 7])
    >>> len(seq)
    3
    >>> [v.index for v in seq]
    [2, 3, 7]
    >>> seq = seq.select([0, 2])         # filtering an existing vertex set
    >>> [v.index for v in seq]
    [2, 7]
    >>> seq = graph.vs.select([2, 3, 7, "foo", 3.5])
    >>> len(seq)
    3

- If the first positional argument is an integer, all remaining arguments are also
  expected to be integers and they are interpreted as indices into the current vertex
  set. This is just syntactic sugar, you could achieve an equivalent effect by
  passing a list as the first positional argument, but this way you can omit the
  square brackets::

    >>> seq = graph.vs.select(2, 3, 7)
    >>> len(seq)
    3

Keyword arguments can be used to filter the vertices based on their attributes
or their structural properties. The name of each keyword argument should consist
of at most two parts: the name of the attribute or structural property and the
filtering operator. The operator can be omitted; in that case, we automatically
assume the equality operator. The possibilities are as follows (where
*name* denotes the name of the attribute or property):

================ ================================================================
Keyword argument Meaning
================ ================================================================
``name_eq``      The attribute/property value must be *equal to* the value of the
                 keyword argument
---------------- ----------------------------------------------------------------
``name_ne``      The attribute/property value must *not be equal to* the value of
                 the keyword argument
---------------- ----------------------------------------------------------------
``name_lt``      The attribute/property value must be *less than* the value of
                 the keyword argument
---------------- ----------------------------------------------------------------
``name_le``      The attribute/property value must be *less than or equal to* the
                 value of the keyword argument
---------------- ----------------------------------------------------------------
``name_gt``      The attribute/property value must be *greater than* the value of
                 the keyword argument
---------------- ----------------------------------------------------------------
``name_ge``      The attribute/property value must be *greater than or equal to*
                 the value of the keyword argument
---------------- ----------------------------------------------------------------
``name_in``      The attribute/property value must be *included in* the value of
                 the keyword argument, which must be a sequence in this case
---------------- ----------------------------------------------------------------
``name_notin``   The attribute/property value must *not be included in* the value
                 of the the keyword argument, which must be a sequence in this
                 case
================ ================================================================

For instance, the following command gives you people younger than 30 years in
our imaginary social network::

  >>> g.vs.select(age_lt=30)

.. note::
   Due to the syntactical constraints of Python, you cannot use the admittedly
   simpler syntax of ``g.vs.select(age < 30)`` as only the equality operator is
   allowed to appear in an argument list in Python.

To save you some typing, you can even omit the :meth:`~VertexSeq.select` method if
you wish::

  >>> g.vs(age_lt=30)

Theoretically, it can happen that there exists an attribute and a structural property
with the same name (e.g., you could have a vertex attribute named ``degree``). In that
case, we would not be able to decide whether the user meant ``degree`` as a structural
property or as a vertex attribute. To resolve this ambiguity, structural property names
*must* always be preceded by an underscore (``_``) when used for filtering. For example, to
find vertices with degree larger than 2::

  >>> g.vs(_degree_gt=2)

There are also a few special structural properties for selecting edges:

- Using ``_source`` or ``_from`` in the keyword argument list of :meth:`EdgeSeq.select`
  filters based on the source vertices of the edges. E.g., to select all the edges
  originating from Claire (who has vertex index 2)::

    >>> g.es.select(_source=2)

- Using ``_target`` or ``_to`` filters based on the target vertices. This is different
  from ``_source`` and ``_from`` if the graph is directed.

- ``_within`` takes a :class:`VertexSeq` object or a list or set of vertex indices
  and selects all the edges that originate and terminate in the given vertex
  set. For instance, the following expression selects all the edges between
  Claire (vertex index 2), Dennis (vertex index 3) and Esther (vertex index 4)::

    >>> g.es.select(_within=[2,3,4])

  We could also have used a :class:`VertexSeq` object::

    >>> g.es.select(_within=g.vs[2:5])

- ``_between`` takes a tuple consisting of two :class:`VertexSeq` objects or lists
  containing vertex indices or :class:`Vertex` objects and selects all the edges that
  originate in one of the sets and terminate in the other. E.g., to select all the
  edges that connect men to women::

    >>> men = g.vs.select(gender="m")
    >>> women = g.vs.select(gender="f")
    >>> g.es.select(_between=(men, women))

Finding a single vertex or edge with some properties
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In many cases we are looking for a single vertex or edge of a graph with some properties,
and either we do not care which one of the matches is returned if there are multiple
matches, or we know in advance that there will be only one match. A typical example is
looking up vertices by their names in the ``name`` property. :class:`VertexSeq` and
:class:`EdgeSeq` objects provide the :meth:`~VertexSeq.find` method for such use-cases.
:meth:`~VertexSeq.find` works similarly to :meth:`~VertexSeq.select`, but it returns
only the first match if there are multiple matches, and throws an exception if no
match is found. For instance, to look up the vertex corresponding to Claire, one can
do this::

  >>> claire = g.vs.find(name="Claire")
  >>> type(claire)
  igraph.Vertex
  >>> claire.index
  2

Looking up an unknown name will yield an exception::

  >>> g.vs.find(name="Joe")
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  ValueError: no such vertex

Looking up vertices by names
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Looking up vertices by names is a very common operation, and it is usually much easier
to remember the names of the vertices in a graph than their IDs. To this end, |igraph|
treats the ``name`` attribute of vertices specially; they are indexed such that vertices
can be looked up by their names in amortized constant time. To make things even easier,
|igraph| accepts vertex names (almost) anywhere where it expects vertex IDs, and also
accepts collections (list, tuples etc) of vertex names anywhere where it expects lists
of vertex IDs or :class:`VertexSeq` instances. E.g, you can simply look up the degree
(number of connections) of Dennis as follows::

  >>> g.degree("Dennis")
  3

or, alternatively::

  >>> g.vs.find("Dennis").degree()
  3

The mapping between vertex names and IDs is maintained transparently by |igraph| in
the background; whenever the graph changes, |igraph| also updates the internal mapping.
However, uniqueness of vertex names is *not* enforced; you can easily create a graph
where two vertices have the same name, but |igraph| will return only one of them when
you look them up by names, the other one will be available only by its index.

Treating a graph as an adjacency matrix
=======================================

Adjacency matrix is another way to form a graph. In adjacency matrix, rows and columns are labeled by graph vertices: the elements of the matrix indicate whether the vertices *i* and *j* have a common edge (*i, j*).
The adjacency matrix for the example graph is

::

  >>> g.get_adjacency()
  Matrix([
    [0, 1, 1, 0, 0, 1, 0],
    [1, 0, 0, 0, 0, 0, 0],
    [1, 0, 0, 1, 1, 1, 0],
    [0, 0, 1, 0, 1, 0, 1],
    [0, 0, 1, 1, 0, 0, 0],
    [1, 0, 1, 0, 0, 0, 1],
    [0, 0, 0, 1, 0, 1, 0]
  ])

For example, Claire (``[1, 0, 0, 1, 1, 1, 0]``) is directly connected to Alice (who has vertex index 0), Dennis (index 3),
Esther (index 4), and Frank (index 5), but not to Bob (index 1) nor George (index 6).

.. _tutorial-layouts-plotting:

Layouts and plotting
====================

A graph is an abstract mathematical object without a specific representation in 2D or
3D space. This means that whenever we want to visualise a graph, we have to find a
mapping from vertices to coordinates in two- or three-dimensional space first,
preferably in a way that is pleasing for the eye. A separate branch of graph theory,
namely graph drawing, tries to solve this problem via several graph layout algorithms.
|igraph| implements quite a few layout algorithms and is also able to draw them onto
the screen or to a PDF, PNG or SVG file using the `Cairo library <https://www.cairographics.org>`_.

.. important::
   To follow the examples of this subsection, you need the Python bindings of the
   Cairo library or matplotlib (depending on what backend is selected). The previous
   chapter (:ref:`installing-igraph`) tells you more about how to install Cairo's Python
   bindings.

Layout algorithms
^^^^^^^^^^^^^^^^^

The layout methods in |igraph| are to be found in the :class:`Graph` object, and they
always start with ``layout_``. The following table summarises them:

==================================== =============== =============================================
Method name                          Short name      Algorithm description
==================================== =============== =============================================
``layout_circle``                    ``circle``,     Deterministic layout that places the
                                     ``circular``    vertices on a circle
------------------------------------ --------------- ---------------------------------------------
``layout_davidson_harel``            ``dh``          Davidson-Harel simulated annealing algorithm
------------------------------------ --------------- ---------------------------------------------
``layout_drl``                       ``drl``         The `Distributed Recursive Layout`_ algorithm
                                                     for large graphs
------------------------------------ --------------- ---------------------------------------------
``layout_fruchterman_reingold``      ``fr``          Fruchterman-Reingold force-directed algorithm
------------------------------------ --------------- ---------------------------------------------
``layout_fruchterman_reingold_3d``   ``fr3d``,       Fruchterman-Reingold force-directed algorithm
                                     ``fr_3d``       in three dimensions
------------------------------------ --------------- ---------------------------------------------
``layout_graphopt``                  ``graphopt``    The GraphOpt algorithm for large graphs
------------------------------------ --------------- ---------------------------------------------
``layout_grid``                      ``grid``        Regular grid layout
------------------------------------ --------------- ---------------------------------------------
``layout_kamada_kawai``              ``kk``          Kamada-Kawai force-directed algorithm
------------------------------------ --------------- ---------------------------------------------
``layout_kamada_kawai_3d``           ``kk3d``,       Kamada-Kawai force-directed algorithm
                                     ``kk_3d``       in three dimensions
------------------------------------ --------------- ---------------------------------------------
``layout_lgl``                       ``large``,      The `Large Graph Layout`_ algorithm for
                                     ``lgl``,        large graphs
                                     ``large_graph``
------------------------------------ --------------- ---------------------------------------------
``layout_mds``                       ``mds``         Multidimensional scaling layout
------------------------------------ --------------- ---------------------------------------------
``layout_random``                    ``random``      Places the vertices completely randomly
------------------------------------ --------------- ---------------------------------------------
``layout_random_3d``                 ``random_3d``   Places the vertices completely randomly in 3D
------------------------------------ --------------- ---------------------------------------------
``layout_reingold_tilford``          ``rt``,         Reingold-Tilford tree layout, useful for
                                     ``tree``        (almost) tree-like graphs
------------------------------------ --------------- ---------------------------------------------
``layout_reingold_tilford_circular`` ``rt_circular`` Reingold-Tilford tree layout with a polar
                                                     coordinate post-transformation, useful for
                                                     (almost) tree-like graphs
------------------------------------ --------------- ---------------------------------------------
``layout_sphere``                    ``sphere``,     Deterministic layout that places the vertices
                                     ``spherical``,  evenly on the surface of a sphere
                                     ``circular_3d``
==================================== =============== =============================================

.. _Distributed Recursive Layout: https://www.osti.gov/doecode/biblio/54626
.. _Large Graph Layout: https://sourceforge.net/projects/lgl/

Layout algorithms can either be called directly or using the common layout method called
:meth:`~Graph.layout`::

  >>> layout = g.layout_kamada_kawai()
  >>> layout = g.layout("kamada_kawai")

The first argument of the :meth:`~Graph.layout` method must be the short name of the
layout algorithm (see the table above). All the remaining positional and keyword arguments
are passed intact to the chosen layout method. For instance, the following two calls are
completely equivalent::

  >>> layout = g.layout_reingold_tilford(root=[2])
  >>> layout = g.layout("rt", root=[2])

Layout methods return a :class:`~layout.Layout` object, which behaves mostly like a list of lists.
Each list entry in a :class:`~layout.Layout` object corresponds to a vertex in the original graph
and contains the vertex coordinates in the 2D or 3D space. :class:`~layout.Layout` objects also
contain some useful methods to translate, scale or rotate the coordinates in a batch.
However, the primary utility of :class:`~layout.Layout` objects is that you can pass them to the
:func:`~drawing.plot` function along with the graph to obtain a 2D drawing.

Drawing a graph using a layout
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For instance, we can plot our imaginary social network with the Kamada-Kawai
layout algorithm as follows::

  >>> layout = g.layout("kk")
  >>> ig.plot(g, layout=layout)

This should open an external image viewer showing a visual representation of the network,
something like the one on the following figure (although the exact placement of
nodes may be different on your machine since the layout is not deterministic):

.. figure:: figures/tutorial_social_network_1.png
   :alt: The visual representation of our social network (Cairo backend)
   :align: center

   Our social network with the Kamada-Kawai layout algorithm

If you prefer to use `matplotlib`_ as a plotting engine, create an axes and use the
``target`` argument::

  >>> import matplotlib.pyplot as plt
  >>> fig, ax = plt.subplots()
  >>> ig.plot(g, layout=layout, target=ax)

.. figure:: figures/tutorial_social_network_1_mpl.png
   :alt: The visual representation of our social network (matplotlib backend)
   :align: center

.. note::
   When plotting rooted trees, Cairo automatically puts the root on top of the image and
   the leaves at the bottom. For `matplotlib`_, the root is usually at the bottom instead.
   You can easily place the root on top by calling `ax.invert_yaxis()`.

Hmm, this is not too pretty so far. A trivial addition would be to use the names as the
vertex labels and to color the vertices according to the gender. Vertex labels are taken
from the ``label`` attribute by default and vertex colors are determined by the
``color`` attribute, so we can simply create these attributes and re-plot the graph::

  >>> g.vs["label"] = g.vs["name"]
  >>> color_dict = {"m": "blue", "f": "pink"}
  >>> g.vs["color"] = [color_dict[gender] for gender in g.vs["gender"]]
  >>> ig.plot(g, layout=layout, bbox=(300, 300), margin=20)  # Cairo backend
  >>> ig.plot(g, layout=layout, target=ax)  # matplotlib backend

Note that we are simply re-using the previous layout object here, but for the Cairo backend
we also specified that we need a smaller plot (300 x 300 pixels) and a larger margin around
the graph to fit the labels (20 pixels). These settings would be ignored for the Matplotlib
backend. The result is:

.. figure:: figures/tutorial_social_network_2.png
   :alt: The visual representation of our social network - with names and genders
   :align: center

   Our social network - with names as labels and genders as colors

and for matplotlib:

.. figure:: figures/tutorial_social_network_2_mpl.png
   :alt: The visual representation of our social network - with names and genders
   :align: center

   Our social network - with names as labels and genders as colors

Instead of specifying the visual properties as vertex and edge attributes, you can
also give them as keyword arguments to :func:`~drawing.plot`::

  >>> color_dict = {"m": "blue", "f": "pink"}
  >>> ig.plot(g, layout=layout, vertex_color=[color_dict[gender] for gender in g.vs["gender"]])

This latter approach is preferred if you want to keep the properties of the visual
representation of your graph separate from the graph itself. You can simply set up
a Python dictionary containing the keyword arguments you would pass to :func:`~drawing.plot`
and then use the double asterisk (``**``) operator to pass your specific styling
attributes to :func:`~drawing.plot`::

  >>> visual_style = {}
  >>> visual_style["vertex_size"] = 20
  >>> visual_style["vertex_color"] = [color_dict[gender] for gender in g.vs["gender"]]
  >>> visual_style["vertex_label"] = g.vs["name"]
  >>> visual_style["edge_width"] = [1 + 2 * int(is_formal) for is_formal in g.es["is_formal"]]
  >>> visual_style["layout"] = layout
  >>> visual_style["bbox"] = (300, 300)
  >>> visual_style["margin"] = 20
  >>> ig.plot(g, **visual_style)

The final plot shows the formal ties with thick lines while informal ones with thin lines:

.. figure:: figures/tutorial_social_network_3.png
   :alt: The visual representation of our social network - with names, genders and formal ties
   :align: center

   Our social network - also showing which ties are formal

To sum it all up: there are special vertex and edge properties that correspond to
the visual representation of the graph. These attributes override the default settings
of |igraph| (see :doc:`configuration` for overriding the system-wide defaults).
Furthermore, appropriate keyword arguments supplied to :func:`~drawing.plot` override the
visual properties provided by the vertex and edge attributes. The following two
tables summarise the most frequently used visual attributes for vertices and edges,
respectively:

Vertex attributes controlling graph plots
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

=============== ====================== ==========================================
Attribute name  Keyword argument       Purpose
=============== ====================== ==========================================
``color``       ``vertex_color``       Color of the vertex
--------------- ---------------------- ------------------------------------------
``font``        ``vertex_font``        Font family of the vertex
--------------- ---------------------- ------------------------------------------
``label``       ``vertex_label``       Label of the vertex
--------------- ---------------------- ------------------------------------------
``label_angle`` ``vertex_label_angle`` The placement of the vertex label on the
                                       circle around the vertex. This is an angle
                                       in radians, with zero belonging to the
                                       right side of the vertex.
--------------- ---------------------- ------------------------------------------
``label_color`` ``vertex_label_color`` Color of the vertex label
--------------- ---------------------- ------------------------------------------
``label_dist``  ``vertex_label_dist``  Distance of the vertex label from the
                                       vertex itself, relative to the vertex size
--------------- ---------------------- ------------------------------------------
``label_size``  ``vertex_label_size``  Font size of the vertex label
--------------- ---------------------- ------------------------------------------
``order``       ``vertex_order``       Drawing order of the vertices. Vertices
                                       with a smaller order parameter will be
                                       drawn first.
--------------- ---------------------- ------------------------------------------
``shape``       ``vertex_shape``       Shape of the vertex. Known shapes are:
                                       ``rectangle``, ``circle``, ``diamond``,
                                       ``hidden``, ``triangle-up``,
                                       ``triangle-down``.
                                       Several aliases are also accepted, see
                                       :data:`drawing.known_shapes`.
--------------- ---------------------- ------------------------------------------
``size``        ``vertex_size``        Size of the vertex in pixels
=============== ====================== ==========================================


Edge attributes controlling graph plots
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

=============== ====================== ==========================================
Attribute name  Keyword argument       Purpose
=============== ====================== ==========================================
``color``       ``edge_color``         Color of the edge
--------------- ---------------------- ------------------------------------------
``curved``      ``edge_curved``        The curvature of the edge. Positive values
                                       correspond to edges curved in CCW
                                       direction, negative numbers correspond to
                                       edges curved in clockwise (CW) direction.
                                       Zero represents straight edges. ``True``
                                       is interpreted as 0.5, ``False`` is
                                       interpreted as zero. This is useful to
                                       make multiple edges visible. See also the
                                       ``autocurve`` keyword argument to
                                       :func:`~drawing.plot`.
--------------- ---------------------- ------------------------------------------
``font``        ``edge_font``          Font family of the edge
--------------- ---------------------- ------------------------------------------
``arrow_size``  ``edge_arrow_size``    Size (length) of the arrowhead on the edge
                                       if the graph is directed, relative to 15
                                       pixels.
--------------- ---------------------- ------------------------------------------
``arrow_width`` ``edge_arrow_width``   Width of the arrowhead on the edge if the
                                       graph is directed, relative to 10 pixels.
--------------- ---------------------- ------------------------------------------
``loop_size``   ``edge_loop_size``     Size of self-loops. It can be set as a
                                       negative number, in which case it scales
                                       with the size of the corresponding vertex
                                       (e.g. -1.0 means the loop has the same size
                                       as the vertex). This attribute is
                                       ignored by edges that are not loops.
                                       This attribute is available only in the
                                       matplotlib backend.
--------------- ---------------------- ------------------------------------------
``width``       ``edge_width``         Width of the edge in pixels.
--------------- ---------------------- ------------------------------------------
``label``       ``edge_label``         If specified, it adds a label to the edge.
--------------- ---------------------- ------------------------------------------
``background``  ``edge_background``    If specified, it adds a rectangular box
                                       around the edge label, of the specified
                                       color (matplotlib only).
--------------- ---------------------- ------------------------------------------
``align_label`` ``edge_align_label``   If True, rotate the edge label such that
                                       it aligns with the edge direction. Labels
                                       that would be upside-down are flipped
                                       (matplotlib only).
=============== ====================== ==========================================


Generic keyword arguments of ``plot()``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

These settings can be specified as keyword arguments to the :func:`~drawing.plot` function
to control the overall appearance of the plot.

================ ================================================================
Keyword argument Purpose
================ ================================================================
``autocurve``    Whether to determine the curvature of the edges automatically in
                 graphs with multiple edges. The default is ``True`` for graphs
                 with less than 10.000 edges and ``False`` otherwise.
---------------- ----------------------------------------------------------------
``bbox``         The bounding box of the plot. This must be a tuple containing
                 the desired width and height of the plot. The default plot is
                 600 pixels wide and 600 pixels high. Ignored for the
                 Matplotlib backend.
---------------- ----------------------------------------------------------------
``layout``       The layout to be used. It can be an instance of
                 :class:`~layout.Layout`,
                 a list of tuples containing X-Y coordinates, or the name of a
                 layout algorithm. The default is ``auto``, which selects a
                 layout algorithm automatically based on the size and
                 connectedness of the graph.
---------------- ----------------------------------------------------------------
``margin``       The top, right, bottom and left margins of the plot in pixels.
                 This argument must be a list or tuple and its elements will be
                 re-used if you specify a list or tuple with less than four
                 elements. Ignored for the Matplotlib backend.
================ ================================================================

Specifying colors in plots
^^^^^^^^^^^^^^^^^^^^^^^^^^

|igraph| understands the following color specifications wherever it expects a
color (e.g., edge, vertex or label colors in the respective attributes):

X11 color names
    See the `list of X11 color names <https://en.wikipedia.org/wiki/X11_color_names>`_
    in Wikipedia for the complete list. Alternatively you can see the
    keys of the ``igraph.drawing.colors.known_colors`` dictionary. Color
    names are case insensitive in igraph so ``"DarkBlue"`` can be written as
    ``"darkblue"`` as well.

Color specification in CSS syntax
    This is a string according to one of the following formats (where *R*, *G* and
    *B* denote the red, green and blue components, respectively):

    - ``#RRGGBB``, components range from 0 to 255 in hexadecimal format.
      Example: ``"#0088ff"``.
    - ``#RGB``, components range from 0 to 15 in hexadecimal format. Example:
      ``"#08f"``.
    - ``rgb(R, G, B)``, components range from 0 to 255 or from 0% to
      100%. Example: ``"rgb(0, 127, 255)"`` or ``"rgb(0%, 50%, 100%)"``.

Lists or tuples of RGB values in the range 0-1
    Example: ``(1.0, 0.5, 0)`` or ``[1.0, 0.5, 0]``.

Note that when specifying the same color for all vertices or edges, you can use
a string as-is but not the tuple or list syntax as tuples or lists would be
interpreted as if the *items* in the tuple are for individual vertices or
edges. So, this would work::

  >>> ig.plot(g, vertex_color="green")

But this would not, as it would treat the items in the tuple as palette indices
for the first, second and third vertoces::

  >>> ig.plot(g, vertex_color=(1, 0, 0))

In this latter case, you need to expand the color specification for each vertex
explicitly::

  >>> ig.plot(g, vertex_color=[(1, 0, 0)] * g.vcount())


Saving plots
^^^^^^^^^^^^

|igraph| can be used to create publication-quality plots by asking the :func:`~drawing.plot`
function to save the plot into a file instead of showing it on a screen. This can
be done simply by passing the target filename as an additional argument after the
graph itself. The preferred format is inferred from the extension. |igraph| can
save to anything that is supported by Cairo, including SVG, PDF and PNG files.
SVG or PDF files can then later be converted to PostScript (``.ps``) or Encapsulated
PostScript (``.eps``) format if you prefer that, while PNG files can be converted to
TIF (``.tif``)::

  >>> ig.plot(g, "social_network.pdf", **visual_style)

If you are using the matplotlib backend, you can save your plot as usual::

  >>> fig, ax = plt.subplots()
  >>> ig.plot(g, **visual_style)
  >>> fig.savefig("social_network.pdf")

Many file formats are supported by matplotlib.

|igraph| and the outside world
==============================

No graph module would be complete without some kind of import/export functionality
that enables the package to communicate with external programs and toolkits. |igraph|
is no exception: it provides functions to read the most common graph formats and
to save :class:`Graph` objects into files obeying these format specifications.
The following table summarises the formats |igraph| can read or write:

================ ============= ============================ =============================
Format           Short name    Reader method                Writer method
================ ============= ============================ =============================
Adjacency list   ``lgl``       :meth:`Graph.Read_Lgl`       :meth:`Graph.write_lgl`
(a.k.a. `LGL`_)
---------------- ------------- ---------------------------- -----------------------------
Adjacency matrix ``adjacency`` :meth:`Graph.Read_Adjacency` :meth:`Graph.write_adjacency`
---------------- ------------- ---------------------------- -----------------------------
DIMACS           ``dimacs``    :meth:`Graph.Read_DIMACS`    :meth:`Graph.write_dimacs`
---------------- ------------- ---------------------------- -----------------------------
DL               ``dl``        :meth:`Graph.Read_DL`        not supported yet
---------------- ------------- ---------------------------- -----------------------------
Edge list        ``edgelist``, :meth:`Graph.Read_Edgelist`  :meth:`Graph.write_edgelist`
                 ``edges``,
                 ``edge``
---------------- ------------- ---------------------------- -----------------------------
`GraphViz`_      ``graphviz``, not supported yet            :meth:`Graph.write_dot`
                 ``dot``
---------------- ------------- ---------------------------- -----------------------------
GML              ``gml``       :meth:`Graph.Read_GML`       :meth:`Graph.write_gml`
---------------- ------------- ---------------------------- -----------------------------
GraphML          ``graphml``   :meth:`Graph.Read_GraphML`   :meth:`Graph.write_graphml`
---------------- ------------- ---------------------------- -----------------------------
Gzipped GraphML  ``graphmlz``  :meth:`Graph.Read_GraphMLz`  :meth:`Graph.write_graphmlz`
---------------- ------------- ---------------------------- -----------------------------
LEDA             ``leda``      not supported yet            :meth:`Graph.write_leda`
---------------- ------------- ---------------------------- -----------------------------
Labeled edgelist ``ncol``      :meth:`Graph.Read_Ncol`      :meth:`Graph.write_ncol`
(a.k.a. `NCOL`_)
---------------- ------------- ---------------------------- -----------------------------
`Pajek`_ format  ``pajek``,    :meth:`Graph.Read_Pajek`     :meth:`Graph.write_pajek`
                 ``net``
---------------- ------------- ---------------------------- -----------------------------
Pickled graph    ``pickle``    :meth:`Graph.Read_Pickle`    :meth:`Graph.write_pickle`
================ ============= ============================ =============================

.. _GraphViz: https://www.graphviz.org
.. _LGL: https://lgl.sourceforge.net/#FileFormat
.. _NCOL: https://lgl.sourceforge.net/#FileFormat
.. _Pajek: http://mrvar.fdv.uni-lj.si/pajek/

As an exercise, download the graph representation of the well-known
`Zachary karate club study <https://en.wikipedia.org/wiki/Zachary%27s_karate_club>`_
from :download:`this file </assets/zachary.zip>`, unzip it and try to load it into
|igraph|. Since it is a GraphML file, you must use the GraphML reader method from
the table above (make sure you use the appropriate path to the downloaded file)::

  >>> karate = ig.Graph.Read_GraphML("zachary.graphml")
  >>> ig.summary(karate)
  IGRAPH UNW- 34 78 -- Zachary's karate club network

If you want to convert the very same graph into, say, Pajek's format, you can do it
with the Pajek writer method from the table above::

  >>> karate.write_pajek("zachary.net")

.. note:: Most of the formats have their own limitations; for instance, not all of
   them can store attributes. Your best bet is probably GraphML or GML if you
   want to save |igraph| graphs in a format that can be read from an external
   package and you want to preserve numeric and string attributes. Edge list and
   NCOL is also fine if you don't have attributes (NCOL supports vertex names and
   edge weights, though). If you don't want to use your graphs outside |igraph|
   but you want to store them for a later session, the pickled graph format
   ensures that you get exactly the same graph back. The pickled graph format
   uses Python's ``pickle`` module to store and read graphs.

There are two helper methods as well: :func:`read` is a generic entry point for
reader methods which tries to infer the appropriate format from the file extension.
:meth:`Graph.write` is the opposite of :func:`read`: it lets you save a graph where
the preferred format is again inferred from the extension. The format detection of
:func:`read` and :meth:`Graph.write` can be overridden by the ``format`` keyword
argument which accepts the short names of the formats from the above table::

  >>> karate = ig.load("zachary.graphml")
  >>> karate.write("zachary.net")
  >>> karate.write("zachary.my_extension", format="gml")


Where to go next
================

This tutorial was only scratching the surface of what |igraph| can do.  My
long-term plans are to extend this tutorial into a proper manual-style
documentation to |igraph| in the next chapters. In the meanwhile, check out the
:doc:`api/index` which should provide information about almost every
|igraph| class, function or method. A good starting point is the documentation
of the :class:`Graph` class. Should you get stuck, try asking in our
`Discourse group`_ first - maybe there is someone out there who can help you
out immediately.

.. _Discourse group: https://igraph.discourse.group
.. _matplotlib: https://matplotlib.org/
.. _IPython: https://ipython.readthedocs.io/en/stable/
.. _Jupyter: https://jupyter.org/


---

## <a name="doc-source-visualisation-rst"></a>File: doc\source\visualisation.rst

.. include:: include/global.rst

=======================
Visualisation of graphs
=======================
|igraph| includes functionality to visualize graphs. There are two main components: graph layouts and graph plotting.

In the following examples, we will assume |igraph| is imported as `ig` and a
:class:`Graph` object has been previously created, e.g.::

   >>> import igraph as ig
   >>> g = ig.Graph(edges=[[0, 1], [2, 3]])

Read the :doc:`api/index` for details on each function and class. See the :ref:`tutorial <tutorial-layouts-plotting>` and
the :doc:`tutorials/index` for examples.

Graph layouts
=============
A graph *layout* is a low-dimensional (usually: 2 dimensional) representation of a graph. Different layouts for the same
graph can be computed and typically preserve or highlight distinct properties of the graph itself. Some layouts only make
sense for specific kinds of graphs, such as trees.

|igraph| offers several graph layouts. The general function to compute a graph layout is :meth:`Graph.layout`::

   >>> layout = g.layout(layout='auto')

See below for a list of supported layouts. The resulting object is an instance of `igraph.layout.Layout` and has some
useful properties:

- :attr:`Layout.coords`: the coordinates of the vertices in the layout (each row is a vertex)
- :attr:`Layout.dim`: the number of dimensions of the embedding (usually 2)

and methods:

- :meth:`Layout.boundaries` the rectangle with the extreme coordinates of the layout
- :meth:`Layout.bounding_box` the boundary, but as an `igraph.drawing.utils.BoundingBox` (see below)
- :meth:`Layout.centroid` the coordinates of the centroid of the graph layout

Indexing and slicing can be performed and returns the coordinates of the requested vertices::

   >>> coords_subgraph = layout[:2]  # Coordinates of the first two vertices

.. note:: The returned object is a list of lists with the coordinates, not an `igraph.layout.Layout`
   object. You can wrap the result into such an object easily:

      >>> layout_subgraph = ig.Layout(coords=layout[:2])

It is possible to perform linear transformations to the layout:

- :meth:`Layout.translate`
- :meth:`Layout.center`
- :meth:`Layout.scale`
- :meth:`Layout.fit_into`
- :meth:`Layout.rotate`
- :meth:`Layout.mirror`

as well as a generic nonlinear transformation via:

- :meth:`Layout.transform`

The following regular layouts are supported:

- `Graph.layout_star`: star layout
- `Graph.layout_circle`: circular/spherical layout
- `Graph.layout_grid`: regular grid layout in 2D
- `Graph.layout_grid_3d`: regular grid layout in 3D
- `Graph.layout_random`: random layout (2D and 3D)

The following algorithms produce nice layouts for general graphs:

- `Graph.layout_davidson_harel`: Davidson-Harel layout, based on simulated annealing optimization including edge crossings
- `Graph.layout_drl`: DrL layout for large graphs (2D and 3D), a scalable force-directed layout
- `Graph.layout_fruchterman_reingold`: Fruchterman-Reingold layout (2D and 3D), a "spring-electric" layout based on classical physics
- `Graph.layout_graphopt`: the graphopt algorithm, another force-directed layout
- `Graph.layout_kamada_kawai`: Kamada-Kawai layout (2D and 3D), a "spring" layout based on classical physics
- `Graph.layout_lgl`: Large Graph Layout
- `Graph.layout_mds`: multidimensional scaling layout
- `Graph.layout_umap`: Uniform Manifold Approximation and Projection (2D and 3D). UMAP works especially well when the graph is composed
  by "clusters" that are loosely connected to each other.

The following algorithms are useful for *trees* (and for Sugiyama *directed acyclic graphs* or *DAGs*):

- `Graph.layout_reingold_tilford`: Reingold-Tilford layout
- `Graph.layout_reingold_tilford_circular`: circular Reingold-Tilford layout
- `Graph.layout_sugiyama`: Sugiyama layout, a hierarchical layout

For *bipartite graphs*, there is a dedicated function:

- `Graph.layout_bipartite`: bipartite layout

More might be added in the future, based on request.

Graph plotting
==============
Once the layout of a graph has been computed, |igraph| can assist with the plotting itself. Plotting happens within a single
function, `igraph.plot`.

Plotting with the default image viewer
++++++++++++++++++++++++++++++++++++++

A naked call to `igraph.plot` generates a temporary file and opens it with the default image viewer::

   >>> ig.plot(g)

(see below if you are using this in a `Jupyter`_ notebook). This uses the `Cairo`_ library behind the scenes.

Saving a plot to a file
+++++++++++++++++++++++

A call to `igraph.plot` with a `target` argument stores the graph image in the specified file and does *not*
open it automatically. Based on the filename extension, any of the following output formats can be chosen:
PNG, PDF, SVG and PostScript::

   >>> ig.plot(g, target='myfile.pdf')

.. note:: PNG is a raster image format while PDF, SVG, and Postscript are vector image formats. Choose one of the last three
   formats if you are planning on refining the image with a vector image editor such as Inkscape or Illustrator.

Plotting graphs within Matplotlib figures
+++++++++++++++++++++++++++++++++++++++++

If the target argument is a `matplotlib`_ axes, the graph will be plotted inside that axes::

   >>> import matplotlib.pyplot as plt
   >>> fig, ax = plt.subplots()
   >>> ig.plot(g, target=ax)

You can then further manipulate the axes and figure however you like via the `ax` and `fig` variables (or whatever you
called them). This variant does not use `Cairo`_ directly and might be lacking some features that are available in the
`Cairo`_ backend: please open an issue on Github to request specific features.

.. note::
   When plotting rooted trees, Cairo automatically puts the root on top of the image and
   the leaves at the bottom. For `matplotlib`_, the root is usually at the bottom instead.
   You can easily place the root on top by calling `ax.invert_yaxis()`.

Plotting via `matplotlib`_ makes it easy to combine igraph with other plots. For instance, if you want to have a figure
with two panels showing different aspects of some data set, say a graph and a bar plot, you can easily do that::

   >>> import matplotlib.pyplot as plt
   >>> fig, axs = plt.subplots(1, 2, figsize=(8, 4))
   >>> ig.plot(g, target=axs[0])
   >>> axs[1].bar(x=[0, 1, 2], height=[1, 5, 3], color='tomato')

Another common situation is modifying the graph plot after the fact, to achieve some kind of customization. For instance,
you might want to change the size and color of the vertices::

   >>> import matplotlib.pyplot as plt
   >>> fig, ax = plt.subplots()
   >>> ig.plot(g, target=ax)
   >>> artist = ax.get_children()[0] # This is a GraphArtist
   >>> dots = artist.get_vertices()
   >>> dot.set_facecolors(['tomato'] * g.vcount())
   >>> dot.set_sizes(dot.get_sizes() * 2) # double the default radius

That also helps as a workaround if you cannot figure out how to use the plotting options below: just use the defaults and
then customize the appearance of your graph via standard `matplotlib`_ tools.

.. note:: The order of `artist.get_children()` is the following: (i) one artist for clustering hulls if requested;
   (ii) one artist for edges; (iii) one artist for vertices; (iv) one artist for **each** edge label; (v) one
   artist for **each** vertex label.

To use `matplotlib_` as your default plotting backend, you can set:

>>> ig.config['plotting.backend'] = 'matplotlib'

Then you don't have to specify an `Axes` anymore:

>>> ig.plot(g)

will automatically make a new `Axes` for you and return it.


Plotting graphs in Jupyter notebooks
++++++++++++++++++++++++++++++++++++

|igraph| supports inline plots within a `Jupyter`_ notebook via both the `Cairo`_ and `matplotlib`_ backend. If you are
calling `igraph.plot` from a notebook cell without a `matplotlib`_ axes, the image will be shown inline in the corresponding
output cell. Use the `bbox` argument to scale the image while preserving the size of the vertices, text, and other artists.
For instance, to get a compact plot (using the Cairo backend only)::

   >>> ig.plot(g, bbox=(0, 0, 100, 100))

These inline plots can be either in PNG or SVG format. There is currently an open bug that makes SVG fail if more than one graph
per notebook is plotted: we are working on a fix for that. In the meanwhile, you can use PNG representation.

If you want to use the `matplotlib`_ engine in a Jupyter notebook, you can use the recipe above. First create an axes, then
tell `igraph.plot` about it via the `target` argument::

   >>> import matplotlib.pyplot as plt
   >>> fig, ax = plt.subplots()
   >>> ig.plot(g, target=ax)

Exporting to other graph formats
++++++++++++++++++++++++++++++++++
If igraph is missing a certain plotting feature and you cannot wait for us to include it, you can always export your graph
to a number of formats and use an external graph plotting tool. We support both conversion to file (e.g. DOT format used by
`graphviz`_) and to popular graph libraries such as `networkx`_ and `graph-tool`_::

   >>> dot = g.write('/myfolder/myfile.dot')
   >>> n = g.to_networkx()
   >>> gt = g.to_graph_tool()

You do not need to have any libraries installed if you export to file, but you do need them to convert directly to external
Python objects (`networkx`_, `graph-tool`_).

Plotting options
================

You can add an argument `layout` to the `plot` function to specify a precomputed layout, e.g.::

   >>> layout = g.layout("kamada_kawai")
   >>> ig.plot(g, layout=layout)

It is also possible to use the name of the layout algorithm directly::

   >>> ig.plot(g, layout="kamada_kawai")

If the layout is left unspecified, igraph uses the dedicated `layout_auto()` function, which chooses between one of several
possible layouts based on the number of vertices and edges.

You can also specify vertex and edge color, size, and labels - and more - via additional arguments, e.g.::

   >>> ig.plot(g,
   ...         vertex_size=20,
   ...         vertex_color=['blue', 'red', 'green', 'yellow'],
   ...         vertex_label=['first', 'second', 'third', 'fourth'],
   ...         edge_width=[1, 4],
   ...         edge_color=['black', 'grey'],
   ...         )

See the :ref:`tutorial <tutorial-layouts-plotting>` for examples and a full list of options.

.. _matplotlib: https://matplotlib.org
.. _Jupyter: https://jupyter.org/
.. _Cairo: https://www.cairographics.org
.. _graphviz: https://www.graphviz.org
.. _networkx: https://networkx.org/
.. _graph-tool: https://graph-tool.skewed.de/
