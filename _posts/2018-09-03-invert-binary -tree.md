---
layout: post
title: 使用js翻转二叉树
categories: algorithm
description: 使用js翻转二叉树
keywords: 数据结构,算法,翻转二叉树
---
# 二叉树

## 生成二叉树

可以用js对象实现二叉树，一个节点为一个对象，拥有左右子节点的应用，和当前节点的值.
```js
function Node(val, left, right) {
    this.left = left;
    this.right = right;
    this.val = val;
}
```
定义一个二叉树对象来描述这颗二叉树
```js

class BST {
    constructor() {
        this.root = null;
    }
    show() {
        console.log(this.root)
    }
    insert(val) {
        // insert node
    }
}
```

插入节点，首先判断节点的值是否比父节点的小，是的话在判断是否已经存在左节点，存在则将父节点设为那个左节点继续判断，不存在则将父节点的左节点设为当前节点。

```js
insert(val) {
        let n = new Node(val, null, null);
        if (this.root === null) {
            this.root = n;
        } else {
            let current = this.root;
            while (current) {
                if (val <= current.val) {
                    if (current.left) {
                        current = current.left;
                    } else {
                        current.left = n;
                        break;
                    }
                } else {
                    if (current.right) {
                        current = current.right;
                    } else {
                        current.right = n;
                        break;
                    }
                }
            }
        }

    }
```

## 翻转二叉树
递归的将左右节点互换即可
``` javascript
function invertTree(root) {
    if (root !== null) {
        [root.left, root.right] = [root.right, root.left];
        invertTree(root.left)
        invertTree(root.right);
    }
}
```
