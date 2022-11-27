# Trees

Trees are very fundamental data structures in computer science. They have values and a children which are different nodes of the same tree.

```
          x
        /   \
       a     b
```

### Where are trees?

Your filesystem is a tree
The dom is a tree
Trees are massively important in compilers. You have probably mininumly heard the term Abstract Syntax Tree.
and there is more...

### Terminology

root - the most parent node. The First. Adam. \
height - The longest path from the root to the most child node \
binary tree - a tree in which has at most 2 children, at least 0 children \
general tree - a tree with 0 or more children \
binary search tree - a tree in which has a specific ordering to the nodes and at most 2 children \
leaves - a node without children \
balanced - A tree is perfectly balanced when any node's left and right children have the same height. \
branching factor - the amount of children a tree has.

There are three types of traversal in tree

### Pre-order traversal

reference vide - [link](https://www.youtube.com/watch?v=Bfqd8BsPVuw&ab_channel=takeUforward)

In this we first go and visit all the left nodes (in depth left first) and then we visit all the right nodes of the tree.

**Recursive solution**

```typescript
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     val: number
 *     left: TreeNode | null
 *     right: TreeNode | null
 *     constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *     }
 * }
 */

const walk = (node: TreeNode, path: Array<number>) => {
  if (!node) return path;
  path.push(node.val);

  if (node.left) {
    walk(node.left, path);
  }
  if (node.right) {
    walk(node.right, path);
  }

  return path;
};

function preOrderTraversal(root: TreeNode | null): number[] {
  const path = [];
  return walk(root, path);
}
```

**Non recursive solution**

```typescript
// assuming the stack array as a stack.
export const preOrderIterative = (root: BinaryNode, path = []) => {
  const stack = [];
  if (!root) return path;

  stack.push(root);

  while (stack.length === 0) {
    root = stack.top();
    path.push(root.val);
    stack.pop();
    if (root.right) {
      stack.push(root.right);
    }
    if (root.left) {
      stack.push(root.left);
    }
  }

  return path;
};
```

## In order Traversal

Reference - [link](https://www.youtube.com/watch?v=lxTGsVXjwvM&ab_channel=takeUforward)

**Recursive**

```typescript
const walk = (root: TreeNode | null, path: number[]) => {
  if (root === null) return path;

  walk(root.left, path);
  path.push(root.val);
  walk(root.right, path);

  return path;
};

function inorderTraversal(root: TreeNode | null): number[] {
  const res = [];
  return walk(root, res);
}
```

**Iterative**

```typescript
function inorderTraversal(root: TreeNode | null): number[] {
  const stack = [];
  const path = [];

  while (true) {
    if (root.left) {
      stack.push(root);
      root = root.left;
    } else {
      if (stack.length === 0) {
        break;
      }
      stack.pop();
      path.push(root.val);
      root = root.right;
    }
  }
  return path;
}
```

## Post Order Traversal

**Iterative**

reference - [link](https://www.youtube.com/watch?v=NzIGLLwZBS8&ab_channel=takeUforward)

Sudo code

```typescript
Create an empty stack
Node current = root
while(current != null or !stack.isEmpty())
    if(current != null)
      stack.push(current)
      current = current->left

    else
      Node previousNode = stack.top().right

      if(previousNode == null)
          previousNode = stack.top()
          stack.pop()
          print(previousNode)

          while(!stack.isEmpty() and previousNode == stack.top().right)
            previousNode = stack.top()
            stack.pop()
            print(previousNode)

        else
            current = previousNode
```

**Recursive**

```typescript
const walk = (root: TreeNode | null, path: number[]) => {
  if (root === null) return path;

  walk(root.left, path);
  walk(root.right, path);
  path.push(root.val);

  return path;
};

function inorderTraversal(root: TreeNode | null): number[] {
  const res = [];
  return walk(root, res);
}
```

## Breath first search

Recursive

```typescript
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     val: number
 *     left: TreeNode | null
 *     right: TreeNode | null
 *     constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *     }
 * }
 */

function levelOrder(
  root: TreeNode | null,
  res: number[][] = [],
  level: number = 0
): number[][] {
  if (root === null) return res;

  if (!res[level]) {
    res[level] = [root.val];
  } else {
    res[level].push(root.val);
  }

  if (root.left) {
    levelOrder(root.left, res, level + 1);
  }
  if (root.right) {
    levelOrder(root.right, res, level + 1);
  }

  return res;
}
```

Iterative

```typescript
function levelOrder(root: TreeNode | null): number[][] {
  let result = [],
    queue = [];

  if (root) queue.push(root);

  while (queue.length > 0) {
    let current = [];
    let currentQueueLength = queue.length;
    for (let i = 0; i < currentQueueLength; i++) {
      let currentNode = queue.shift();
      current.push(currentNode.val);
      if (currentNode.left) queue.push(currentNode.left);
      if (currentNode.right) queue.push(currentNode.right);
    }
    result.push(current);
  }

  return result;
}
```

## Trees Implementation

**Insertion**

```typescript
insert(data) {
  // empty tree, insert the first node (the root node)
  if (!root) {
    root = new Node(data);
    return root;
  }

  // start from the root node
  let current = root;

  while (true) {
    if (data > current.data) {
      // search for an empty position in the right subtree
      if (current.rightNode) {
        current = current.rightNode;
      } else {
        // insert node
        current.rightNode = new Node(data);
        return current.rightNode;
      }
    } else {
      // search for an empty position in the left subtree
      if (current.leftNode) {
        current = current.leftNode;
      } else {
        // insert node
        current.leftNode = new Node(data);
        return current.leftNode;
      }
    }
  }
}
```

Delete Reference - [link](https://www.youtube.com/watch?v=kouxiP_H5WE&ab_channel=takeUforward)
