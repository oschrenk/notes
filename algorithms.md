# Algorithms #

## Computational Geometry ##

## 1-Dimensional Range Searching ##

A query asks for points inside an interval. Given a set of points on that line, we can use a binary search tree to determine points within a query range. The points are the leaves, the left subtree contains nodes smaller than the the node where the split occurs, the nodes in the right subtree are bigger.

We search for x and x'. The binary search ends two nodes, lets say a and a'. THe search result should return all leaf nodes between these two nodes.
