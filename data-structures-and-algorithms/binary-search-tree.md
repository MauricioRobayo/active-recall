# Binary Search Tree

<details>
  <summary>What is a Binary Search Tree?</summary>
  <br/>

  Is a node-based binary tree data structure which has the following properties:

  - The left subtree of a node contains only nodes with keys lesser than the node's key.
  - The right subtree of a node contains only nodes with keys greater than the node's key.
  - The left and right subtree each must also be a binary search tree.

  ![Binary Search Tree](https://upload.wikimedia.org/wikipedia/commons/thumb/d/da/Binary_search_tree.svg/1920px-Binary_search_tree.svg.png)

  The above properties of Binary Search Tree provide an ordering among keys so that the operations like search, minimum and maximum can be done fast. If there is no ordering, then we may have to compare every key to search a given key.

</details>
<details>
  <summary>What's depth-first search traversal?</summary>
  <br/>

  Searches referred as _depth-first_ search (DFS) deepen as much as possible on each child before going to the next sibling. For a binary tree, they are defined as access operations at each node, starting with the current node.

  ![Depth-first search of a BST](https://upload.wikimedia.org/wikipedia/commons/thumb/d/dc/Sorted_binary_tree_ALL.svg/1920px-Sorted_binary_tree_ALL.svg.png)

  Depth-first traversal of an example tree: pre-order (red): F, B, A, D, C, E, G, I, H; in-order (yellow): A, B, C, D, E, F, G, H, I; post-order (green): A, C, E, D, B, H, I, G, F.

</details>
<details>
  <summary>What kind of data structure is used for depth-first traversal of a Binary Search Tree?</summary>
  <br/>

  A stack.

</details>
<details>
  <summary>Which are the Depth First ways to traverse a Binary Search Tree (BST)?</summary>
  <br/>

  1. Inorder
  2. Preorder
  3. Postorder

</details>
<details>
  <summary>How is Breadth First traversal of a Binary Search Tree (BST) also called?</summary>
  <br/>

  Level Order Traversal

</details>
<details>
  <summary>Describe the algorithm for Inorder traversal of a Binary Search Tree (BST).</summary>
  <br/>

  1. Traverse the left subtree, i.e., call Inorder(left-subtree).
  2. Visit the root.
  3. Traverse the right subtree, i.e., call Inorder(right-subtree).

</details>
<details>
  <summary>In case of Binary Search Tree (BST), Inorder traversal gives nodes in non-decreasing order. How can we get nodes in non-increasing order?</summary>
  <br/>

  We can use a variation of Inorder traversal where Inorder traversal is reversed.

</details>
<details>
  <summary>How are node ordered in a Binary Search Tree?</summary>
  <br/>

  In a Binary Search Tree each node is ordered such that the key is greater than all keys in its left subtree and less than all keys in its right subtree.

</details>
<details>
  <summary>How is the result of in-order traversal sorted in a Binary Search Tree?</summary>
  <br/>

  In-order traversal returns the keys sorted in ascending order.

</details>
<details>
  <summary>How is the result of reverse in-order traversal sorted in a Binary Search Tree?</summary>
  <br/>

  Since in-order traversal returns the keys in ascending sorted order, if we reverse the in-order traversal, we get the results in descending order.

</details>
