# Trees

## Common Terminology Used In Tress

- **Node**: A node is the individual item/data that makes up the data structure
- **Root**: The root is the first/top Node in the tree
- **Left Child**: The node that is positioned to the left of a root or node
- **Right Child**: The node that is positioned to the right of a root or node
- **Edge**: The edge in a tree is the link between a parent and child node
- **Leaf**: A leaf is a node that does not contain any children
- **Height**: The height of a tree is determined by the number of edges from the root to the bottommost node

## Sample Tree

![image](assets/Read15.png)

### Traversals

An important aspect of trees is how to traverse them. Traversing a tree allows us to search for a node:

- Depth First
- Breadth First

#### Depth First

Depth first traversal is where we prioritize going through the depth (height) of the tree first

Here are three methods for depth first traversal:

- Pre-order: root >> left >> right
- In-order: left >> root >> right
- Post-order: left >> right >> root

![image](assets/Read15-2.png)

Given the sample tree above, our traversals would result in different paths:

- Pre-order: A, B, D, E, C, F
- In-order: D, B, E, A, F, C
- Post-order: D, E, B, F, C, A

Pre-order means that the root has to be looked at first. In our case, looking at the root just means that we output its value. When we call preOrder for the first time, the root will be added to the call stack:

Next, we start reading our preOrder function’s code from top to bottom

Then, our next block of code instructs us to check if our root has a left node set. If the root does, we will then send the left node to our preOrder method recursively. This means that we make another function call, where B is our new root

This process continues until we reach a leaf node. Here’s the state of our tree when we hit our first leaf, D

It’s important to note a few things that are about to happen:

- The program will look for both a `root.left` and a `root.right`. Both will return null, so it will end the execution of that method call
- D will pop off of the call stack and the root will be reassigned back to B
- This is the heart of recursion: when we complete a function call, we pop it off the stack and are able to continue execution through the previous function call

The code block will now pick up where it left off when B was the root. Since it already looked for `root.left`, it will now look for `root.right`

E will output to the console. Since E is a leaf, it will complete the method code block, and pop E off of the call stack and make its way back up to B.

In the function call, B has already checked for root.left, and root.right. There are no further lines of code to execute, so B will be popped off the call stack, so that we can resume execution of A.

Following the same pattern as we did with the other nodes, A’s call stack frame will pick up where it left off, and check out root.right. C will be added to the call stack frame, and it will the new function’s root

C will be outputted to the console, and root.left will be evaluated. Because C has a left child, preOrder will be called, with the parameter root.left.

At this point, the program will find that F does not have any children and it will make its way back up the call stack up to C

C does not have a root.right, so it will pop off the call stack and return to A.

#### Traversal Pseudocode

Pre-order

```
ALGORITHM preOrder(root)
// INPUT <-- root node
// OUTPUT <-- pre-order output of tree node's values

    OUTPUT <-- root.value

    if root.left is not Null
        preOrder(root.left)

    if root.right is not NULL
        preOrder(root.right)
```

In-order

```
ALGORITHM inOrder(root)
// INPUT <-- root node
// OUTPUT <-- in-order output of tree node's values

    if root.left is not NULL
        inOrder(root.left)

    OUTPUT <-- root.value

    if root.right is not NULL
        inOrder(root.right)
```

Post-order

```
ALGORITHM postOrder(root)
// INPUT <-- root node
// OUTPUT <-- post-order output of tree node's values

    if root.left is not NULL
        postOrder(root.left)

    if root.right is not NULL
        postOrder(root.right)

    OUTPUT <-- root.value
```

### Breadth First

Breadth first traversal iterates through the tree by going through each level of the tree node-by-node.

Our output using breadth first traversal is now:

```
Output: A, B, C, D, E, F
```

Given our starting tree shown above, let’s start by putting the root into the queue

Now that we have one node in our queue, we can dequeue it and use that node in our code

From our dequeued node A, we can enqueue the left and right child (in that order).

This leaves us with B as the new front of our queue. We can then repeat the process we did with A: Dequeue the front node, enqueue that node’s left and right nodes, and move to the next new front of the queue.

Now our front is C, so we repeat the dequeue + enqueue children process:

And we continue onwards. When we reach a node that doesn’t have any children, we just dequeue it without any further enqueue.

#### Pseudocode

Here is the pseudocode, utilizing a built-in queue to implement a breadth first traversal.

```
ALGORITHM breadthFirst(root)
// INPUT  <-- root node
// OUTPUT <-- front node of queue to console

  Queue breadth <-- new Queue()
  breadth.enqueue(root)

  while breadth.peek()
    node front = breadth.dequeue()
    OUTPUT <-- front.value

    if front.left is not NULL
      breadth.enqueue(front.left)

    if front.right is not NULL
      breadth.enqueue(front.right)
```

### Binary Trees

In all of our examples, we’ve been using a Binary Tree. Trees can have any number of children per node, but Binary Trees restrict the number of children to two (hence our left and right children).

### Big O

The Big O time complexity for inserting a new node is O(n). Searching for a specific node will also be O(n). Because of the lack of organizational structure in a Binary Tree, the worst case for most operations will involve traversing the entire tree. If we assume that a tree has n nodes, then in the worst case we will have to look at n items, hence the O(n) complexity.

The Big O space complexity for a node insertion using breadth first insertion will be O(w), where w is the largest width of the tree. For example, in the above tree, w is 4.

>> A “perfect” binary tree is one where every non-leaf node has exactly two children. The maximum width for a perfect binary tree, is 2^(h-1), where h is the height of the tree. Height can be calculated as log n, where n is the number of nodes.

### Searching a BST

![image](assets/Read15-3.png)

#### Big O

The Big O time complexity of a Binary Search Tree’s insertion and search operations is O(h), or O(height). In