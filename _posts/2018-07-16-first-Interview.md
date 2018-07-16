---
layout: post
title: 第一次面试(鹅厂外包岗位)
categories: interview
description: 毕业后第一次面试
keywords: 面试
---
# 前言
记下每次面试的经历，不要再犯上一次的错误，希望可以找到一个好一点的工作。

## 经历

毕业之后在微博上找到了一个鹅厂外包的岗位（初级PHP程序员），虽然我原本比较偏向于去找一个前端的岗位，但是还是比较想去大公司学习一下。去面试的时候没有做好比较充足的准备，再加上有点紧张，好多个问题都没有回答正确。之后在坐地铁回去的路上才想到正确的答案，这次面试应该是已经凉了。

## 面试题 (有些以及记得不太清楚了)

### 写几个PHP的字符串处理函数。

strpos,strstr。我只写了两个，其他的都不太记得了。
> php有许多函数与C的string.h里的是一样的。
> strrpos、strrpos字符串第一次出现的位置、字符串最后一次出现的位置
> strstr 截取字符串首次出现到结尾的字符串。
> strchr strstr的别名
> strcmp 字符串比较，区分大小写
> str_split 字符串转数组
> str_shuffle 打乱字符串
> str_replace 字符串替换
> str_repeat 字符串重复
> str_pad 字符串填充
> [http://php.net/manual/zh/ref.strings.php]( 官网)

### 写几个PHP的数组处理函数。

array_push,array_merge。只记得这两个，不知道排序算不算数组处理函数就没有回答了。
> array_change_key_case — 将数组中的所有键名修改为全大写或小写
array_chunk — 将一个数组分割成多个
array_column — 返回数组中指定的一列
array_combine — 创建一个数组，用一个数组的值作为其键名，另一个数组的值作为其值
array_keys — 返回数组中部分的或所有的键名
array_map — 为数组的每个元素应用回调函数
array_merge — 合并一个或多个数组
array_multisort — 对多个数组或多维数组进行排序
array_pad — 以指定长度将一个值填充进数组
array_pop — 弹出数组最后一个单元（出栈）
array_replace — 使用传递的数组替换第一个数组的元素
array_reverse — 返回单元顺序相反的数组
array_search — 在数组中搜索给定的值，如果成功则返回首个相应的键名
array_shift — 将数组开头的单元移出数组
array_slice — 从数组中取出一段
[http://php.net/manual/zh/book.array.php](官网)

### MySQL统计部门人数大于2的部门

答得稀烂。。 SELECT count(id),deptid from depts where deptid>2。完全是错的。。忘记加分组了。
> ```sql
> SELECT deptid,count(id) from deptid where count(id)>2 GROUP BY deptid
> ```

### cookie和session的区别

我只答了session是加密的、cookie是不加密的。之后面试官又问了如何保存登陆状态。
> [https://www.cnblogs.com/zlw-xf/p/8001383.html](cnblog)

### 自动登陆的实现

用户登录后，服务器创建session文件，浏览器保存sessionid，再次登陆，通过sessionid获得session读取用户信息。面试官还问了session是怎么区分各个客户端的，我没答上来，面试官说是通过USER-AGENT和ip
> [http://www.freebuf.com/articles/web/10369.html](你必须了解的Session的本质)

### git、svn

对这部分不太熟悉，上一份工作并没有做太多的代码管理。面试官问了svn怎么进行创建分支以及合并。

>