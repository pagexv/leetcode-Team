## Tree Traversals (Inorder, Preorder and Postorder)

Given the tree

![image-20220304214623161](C:\Users\48965\AppData\Roaming\Typora\typora-user-images\image-20220304214623161.png)

We have three types of ordering

### (a) **Inorder**

>  (Left, Root, Right) : 4 2 5 1 3 

```
Algorithm Inorder(tree)
   1. Traverse the left subtree, i.e., call Inorder(left-subtree)
   2. Visit the root.
   3. Traverse the right subtree, i.e., call Inorder(right-subtree)
```

### (b) **Preorder**

>  (Root, Left, Right) : 1 2 4 5 3 

```
Algorithm Preorder(tree)
   1. Visit the root.
   2. Traverse the left subtree, i.e., call Preorder(left-subtree)
   3. Traverse the right subtree, i.e., call Preorder(right-subtree) 
```

### (c) **Postorder** 

> (Left, Right, Root) : 4 5 2 3 1

```
Algorithm Postorder(tree)
   1. Traverse the left subtree, i.e., call Postorder(left-subtree)
   2. Traverse the right subtree, i.e., call Postorder(right-subtree)
   3. Visit the root.
```