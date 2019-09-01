---
layout: post
title: js二叉树遍历
categories: js
description: 使用js完成二叉树前序，中序，后序，遍历
keywords: 二叉树, 遍历
---
写之前先明确遍历的顺序。
前序 左中右  
中序 中左右 
后序 左右中 

```
     A
    / \
   B   c
  / \   \
 D   E   F
前序  DBEACF
中序  ABDECF
后序  DEBFCA
```
递归的实现比较简单，基本上就是直接对顺序进行递归就可以了。

## 前序
```js
function PreOrderTraverse(parent) {
  if(parent == null) {
    return;
  }
  console.log(parent.data); // 中
  PreOrderTraverse(parent.lchild); // 左
  PreOrderTraverse(parent.rchild); // 右
}
```

## 中序
```js
function InOrderTraverse(node) {
  if (node == null) {
    return;
  }

  InOrderTraverse(node.lchild); // 左
  console.log(node.data); // 中
  InOrderTraverse(node.rchild); // 右
}
```
## 后序
```js
function PostOrderTraverse(node) {
  if (node == null) {
    return;
  }

  PostOrderTraverse(node.lchild); // 左
  PostOrderTraverse(node.rchild); // 右
  console.log(node.data); // 中
}
```
更好的方式其实应该是使用循环来进行遍历，由于递归是不断在调用自己，会不断进入调用栈比较消耗内存。而循环去遍历的时候只调用一次，使用的内存会少很多。 

## 前序（循环）
按照顺序，一直遍历子树，知道最底部，每次遍历的时候，将当前的节点压入栈里。 
当没有左节点的时候，就可以输出自身，然后出栈。 
最后再将当前节点设置为父节点的右节点，开始遍历右子树。 
栈里就一直保存当前值就可以了。
```js
function PreOrderTraverse(root, k) {
    let stack = []  // 存储调用栈
    let curr = root // 头结点
    while (stack.length || curr) {
        if (curr) {
            stack.push(curr) // 压入当前
            curr = curr.left  // 当前节点设置为左子树
        } else {
            // 当左子树都已经遍历完，没有左子树时，就是最左端的节点
            curr = stack.pop()
            console.log(curr) // 输出当前节点
            curr = curr.right // 如果有右节点的话，遍历右节点
        }

    }
};
```
## 中序（循环）
与前面相同，但是在中序里的顺序是中左右。  
所以，当前节点存在时，直接打印当前节点。然后将右节点压入栈。 
再将当前节点赋值为左节点。当前节点为null时，就已经没有子节点了，开始遍历栈底也就是离当前节点最近的一个右节点。
```js
function inOrder(root) {
    let stack = []  // 存储调用栈
    let curr = root // 头结点
    while (stack.length || curr) {
        if (curr) {
            console.log(curr.val) // 输出当前节点
            stack.push(curr.right) // 压入右节点
            curr = curr.left  // 当前节点设置为左子树
        } else {
            // 当左子树都已经遍历完，没有左子树时，就是最左端的节点
            curr = stack.pop()
        }

    }
};
```
## 后序（循环）
后序遍历的顺序是左右中, 后序遍历反过来就是中右左，也就是说，中序遍历的左右节点互换，就可以得到倒序的后序遍历了

```javascript
function outOrder(root) {
    let stack = []  // 存储调用栈
    let curr = root // 头结点
    while (stack.length || curr) {
        if (curr) {
            console.log(curr.val) // 输出当前节点
            stack.push(curr.left) // 压入左节点
            curr = curr.right  // 当前节点设置为右子树
        } else {
            // 当左子树都已经遍历完，没有左子树时，就是最左端的节点
            curr = stack.pop()
        }

    }
};
```