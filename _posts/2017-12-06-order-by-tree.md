---
layout: post
title: js二叉树排序
categories: js
description: 使用js完成二叉树前序，中序，后序。
keywords: 二叉树，排序
---
# 前序
```
function PreOrderTraverse(parent) {
  if(parent == null) {
    return;
  }
  console.log(parent.data);
  PreOrderTraverse(parent.lchild);
  PreOrderTraverse(parent.rchild);
}
```

#中序
```
function InOrderTraverse(node) {
  if (node == null) {
    return;
  }

  InOrderTraverse(node.lchild);
  console.log(node.data);
  InOrderTraverse(node.rchild);
}
```
# 后序
```
function PostOrderTraverse(node) {
  if (node == null) {
    return;
  }

  PostOrderTraverse(node.lchild);
  PostOrderTraverse(node.rchild);
  console.log(node.data);
}
```