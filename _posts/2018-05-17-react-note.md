---
layout: post
title: react学习笔记
categories: react
description: 《深入浅出react和redux》的笔记
keywords: react,学习笔记
---
# 开始记录每章的学习

## Class1 React 和 jquery的不同

jQ即可的方式直观易懂，对于初学者十分适用，但是当项目逐渐变得庞
大时，用 jQuery 写出的代码往往互相纠缠，
[![1-4.png](https://i.loli.net/2018/05/17/5afd573fe55ef.png)](https://i.loli.net/2018/05/17/5afd573fe55ef.png)

使用 React 的方式，就可以避免构建这样复杂的程序结构，无论何种事件，引发的
都是 React 组件的重新渲染，至于如何只修改必要的 DOM 部分，则完全交给 React 去操
作，开发者并不需要关心.

[![1-5.png](https://i.loli.net/2018/05/17/5afd577ba4fe2.png)](https://i.loli.net/2018/05/17/5afd577ba4fe2.png)

## Class2 React的生命周期
