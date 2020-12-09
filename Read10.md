# Stacks & Queues

## What is a Stack

stack is a data structure that consists of Nodes.

Each Node references the next Node in the stack, but **does not reference its previous**.

terminology for a stack:

- **Push** Nodes or items that are put into the stack are pushed
- **Pop** Nodes or items that are removed from the stack are popped. When you attempt to pop an empty stack an exception will be raised.
- **Top** This is the top of the stack.
- **Peek** When you peek you will view the value of the top Node in the stack. When you attempt to peek an empty stack an exception will be raised.
- **IsEmpty** returns true when stack is empty otherwise returns false.

Stacks follow these concepts:

FILO ===>  First In Last Out ===> First Item added to all Last to Pop

LIFO ===>  Last In First Out ===> Last Item added First to pop

## Stack Visualization

When you push something to the stack, it becomes the new top.

![image](assets/Read10.png)

### Push O(1)

Pushing a Node onto a stack will always be an O(1) operation. This is because it takes the same amount of time no matter how many Nodes (n) you have in the stack.

When adding a Node, you push it into the stack by assigning it as the new top, with its next property equal to the original top.

- First, you should have the Node that you want to add. Here is an example of a Node that we want to add to the stack.

- Next, you need to assign the next property of Node 5 to reference the same Node that top is referencing: Node 4

- Technically at this point, your new Node is added to your stack, but there is no indication that it is the first Node in the stack. To make this happen, you have to re-assign our reference top to the newly added Node, Node 5.

### Pop O(1)

Popping a Node off a stack is the action of removing a Node from the top. When conducting a pop, the top Node will be re-assigned to the Node that lives below and the top Node is returned to the user.

- The first step of removing Node 5 from the stack is to create a reference named temp that points to the same Node that top points to.

- Once you have created the new reference type, you now need to re-assign top to the value that the next property is referencing. In our visual, we can see that the next property is pointing to Node 4. We will re-assign top to be Node 4.

- We can now remove Node 5 safely without it affecting the rest of the stack. Before we do that though you may want to make sure that you clear out the next property in your current temp reference.

## What is a Queue

- **Enqueue** Nodes or items that are added to the queue.
- **Dequeue** Nodes or items that are removed from the queue. If called when the queue is empty an exception will be raised.
- **Front** This is the front/first Node of the queue.
- **Rear** This is the rear/last Node of the queue.
- **Peek** When you peek you will view the value of the front Node in the queue. If called when the queue is empty an exception will be raised.
- **IsEmpty** returns true when queue is empty otherwise returns false.

### Enqueue O(1)

When you add an item to a queue, you use the enqueue action. This is done with an O(1) operation in time because it does not matter how many other items live in the queue (n); it takes the same amount of time to perform the operation.

### Dequeue O(1)

When you remove an item from a queue, you use the dequeue action. This is done with an O(1) operation in time because it doesn’t matter how many other items are in the queue, you are always just removing the front Node of the queue.

