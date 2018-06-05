---
layout: post
title: 使用js实现堆排
categories: algorithm
description: 根据堆排原理自己实现堆排算法
keywords: 算法,堆排
---
# 堆排

## 原理

堆可以当作是一个不完整的二叉树，实现堆排并不用构建一个新的数据结构，只需要将数据当作完全二叉树处理就好了。例如：
数组[4,5,1,7,3,15,9,11,2,55,10,12]可以表示为
![TIM图片20180605180830.jpg](https://i.loli.net/2018/06/05/5b16614c7fe68.jpg)
可以从n/2开始处理每一个节点,大于n/2的节点是没有子节点的，例子中数组中有12个元素，可以看到从第7个开始便没有子节点了。
堆排序的循环比较简单。

1.第n/2的节点开始，如果子节点大于父节点，就将数据中两个值互换。
![图片2.png](https://i.loli.net/2018/06/05/5b16634d9d910.png)
  
2.循环完之后，可得到这个数组中的最大值，也就是数组的第一个值，将其从数组中移去并加入到另一个数组中，开始第二次的循环。

## 代码

``` javascript
let arr = [4,5,1,7,3,15,9,11,2,55,10,12];
let sort_arr = [];
function heapOrder(arr) {
    for(let i=parseInt(arr.length/2);i>=0;i--) {
        max_heap({value:arr[i],index:i});
    }
    change_value(0,arr.length-1);
    sort_arr.push(arr.pop());
    if(arr.length>0) {
        return heapOrder(arr);
    }
}

function max_heap(node) {
    let left = {index:node.index*2+1,value:arr[node.index*2+1]}; // left children
    let right = {index:node.index*2+2,value:arr[node.index*2+2]}; //right children
    if(left.value===undefined) {  // no children 
        return;
    } else if(right.value===undefined&&left.value!==undefined) { // own left children
        left.value>node.value?change_value(left.index,node.index):'';
    } else {
        if(node.value<left.value&&left.value>=right.value) { // own left and right children
            change_value(left.index,node.index)
        } else if(node.value<right.value&&right.value>=left.value) {
            change_value(right.index,node.index)
        }
    }
}

function change_value(a,b) {
    let tmp = arr[a];
    arr[a]=arr[b];
    arr[b]=tmp;
}
heapOrder(arr);
console.log(sort_arr);
```
