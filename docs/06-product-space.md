# The Product Space

This section is adapted from [@atlas2014].

To visualize the product space we use some simple design criteria. First, we want the visualization of the product space to be a connected network. By this, we mean avoiding islands of isolated products. the second criteria is that we want the network visualization to be relatively sparse. trying to visualize too many links can create unnecessary visual complexity where the most relevant connections will be occluded. this criteria is achieved by creating a visualization in which the average number of links per node is not larger than 5 and results in a representation that can summarize the structure of the product space using the strongest 1% of the links.

To make sure the visualization of the product space is connected, we calculate the Maximum Spanning Tree (MST) of the proximity matrix. MST is the set of links that connects all the nodes in the network using a minimum number of connections and the maximum possible sum of proximities. We calculated the MST using Kruskal's algorithm. Basically the algorithm sorts the values of the proximity matrix in descending order and then includes links in the MST if and only if they connect an isolated product. By definition, the MST includes all products, but the number of links is the minimum possible.

The second step is to add the strongest connections that were not selected for the MST. By definition a spanning tree for 200 nodes contains 199 edges. Suppose we add 100 connections satisfying our criterion. With the additional 100 connections we end up with 300 edges.

After selecting the links using the above mentioned criteria we build a visualization using a Force-Directed layout algorithm. in this algorithm nodes repel each other, just like electric charges, while edges act as spring trying to bring connected nodes together. this helps to create a visualization in which densely connected sets of nodes are put together while nodes that are not connected are pushed apart.

Finally, we manually clean up the layout to minimize edge crossings and provide the most clearly representation possible.
