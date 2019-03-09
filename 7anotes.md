This is one of my favorite note taking things
---
# Lecture 7a

---

## Learning Targets

1. Given a tree, I can tell whether or not it's a valid BST.
2. I can simulate BST lookup, insert, and delete (on paper)
3. I can simulate left and right rotations (on paper)
4. I can simulate insertAtRoot (on paper)
5. I can simulate Randomized Binary Tree insertion

## Review
We often use binary trees to keep track of order. 

###### Define:
node, edge, root, leaf, tree, subtree, parent, chile, ancestor, height, balanced tree, binary tree, ordered binary tree (aka binary search tree)

**node**: an element in a tree

**edge**: connection between nodes

**root**: the top-most node in a tree

**leaf**: the final nodes in a tree

**subtree**: smaller portion of the tree, that is itself a tree

**parent**: has at least one subtree/node that branches off of it

**child**: node branching off another node/off its parent node

**height**: length of the longest path on the tree (from root to leaf), who's value is determined by the number of edges along the path. **Note** the empty tree has the height **-1**



### Exercise
What tree results from the following sequences of inserts?
* A, B, C, D, E, F, G
* D, C, A, B, E, F, G


### ```insert``` psuedocode
```C++
insert(tree, x):
    if tree is empty:
        make x its new root
        
    else if x < tree's root:
        insert(left subtree, x)
        
    else if tree's root < x:
        insert(right subtree, x)
```
### Tree Rotations
We rotate at the root of the tree that we are interested in. 

Whenever something goes up, the thing that's outside goes with it. When something goes down, the thing that's outside goes with it. Everything in the middle will remain the same. 

### ```insertAtRoot``` psuedocode
```C++
insertAtRoot(tree, x):
    if tree is empty:
        make x it's new root
    
    else if x < tree's root:
        insertAtRoot(left subtree, x)
        do right rotation at tree's root
        
    else if tree's root < x:
        insertAtRoot(right subtree, x)
        do left rotation at tree's root
```

### Suppose we have a BSR with n nodes:
What is the worst-case running time for ```find``` (and ```insert```)
* if we have a really terrible tree?
* if we have a really nice tree?
* if we have a "random" tree?

### Building better trees: Off-line algorithm
1. Take the inputs we want to put in the tree.
2. Randomly shuffle them.
3. Build tree by inserting in shuffled order

But how often to do each? 

Sometimes we will want to put it at the root, and sometimes we want to put them at the root of a subtree.

Answer: If the tree has n nodes before the insert,
* do insert-at-root with probability 1/(n+1)
* otherwise, insert randomly into the appropriate child. 

