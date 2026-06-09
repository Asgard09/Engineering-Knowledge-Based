A Red-Black Tree is a self-balancing binary search tree with a height limit of O(logN), enabling efficient search, insertion, and deletion operations in O(logN) time, unlike standard binary search trees which can take O(N) time.
- Each node has an additional attribute: a color, which can be either red or black.
- These colors are used to maintain balance during insertions and deletions, ensuring efficient data retrieval and manipulation.
## Properties
1. Node Color: Each node is either red or black.
2. Root Property: The root of the tree is always black.
3. Red Node Property: Red nodes cannot have red children (Red nodes cannot be adjacent).
4. Black Node Property: Every path from a node to its descendant leaves must have the same number of black nodes.
5. Leaf Property: All leaves (NIL nodes) are black.