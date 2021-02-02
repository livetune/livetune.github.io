---
layout: post
title: egg 使用 curl 上传 buffer 时修改文件名
categories: [node, egg]
description: egg 使用 curl 上传 buffer 时修改文件名
keywords: 后端, curl, upload
---

最近在折腾企业微信机器人的时候遇到了点问题，记录一下。

### 问题描述

- 需要使用企业微信机器人发送告警信息，由于数据过多，不能直接发送文本消息。
- 使用了 xlsx 这个库生成了一个表格。
- 企业微信机器人发文件需要先发送文件到企业微信的服务器上，再通过上传返回的 media_id 去发送文件。
- 但是由于没有设置文件名，导致接受到的文件名显示为 bufferfile0
- 代码如下
```js
    const { ctx } = this;
    // 上传文件
    const result = await ctx.curl('upload url', {
      method: 'POST',
      dataType: 'json',
      // 单文件上传
      files: buffer,
    });
    // 发送文件
    await ctx.curl('send url', {
      method: 'POST',
      dataType: 'json',
      contentType: 'json',
      data: {
        msgtype: 'file',
        file: {
          media_id: result.data.media_id,
        },
      },
    });
```
![企业微信截图_16122344315746.png](https://i.loli.net/2021/02/02/li2X79E5dmWyevO.png)
### 尝试解决

根据文档，可以设置请求头来更换文件名
``` http
Content-Disposition: form-data; name="media";filename="wework.txt"; filelength=6
```
```js
const result = await ctx.curl('upload url', {
      method: 'POST',
      dataType: 'json',
      headers: {
        'Content-Disposition': 'form-data; name="media";filename="wework.txt"; filelength=6'
      }
      // 单文件上传
      files: buffer,
});
```
设置完之后发送的文件名仍然还是 bufferfile0

### 解决方案

![企业微信截图_16122349592076.png](https://i.loli.net/2021/02/02/WGedun9x8N3PZTm.png)

之后去查了一下 egg 的 curl 库 urllib 的 issue，发现有人遇到了相似的问题，对应的解决方案是使用 formstream 重新构建一个提交的表单和请求头，下面贴出对应代码。

```js
const form = new formstream();
form.buffer('media', new Buffer('This is file2 content.'), 'foo.txt');
const res = await ctx.curl('upload url', {
    method: 'POST',
    dataType: 'json',
    headers: form.headers(),
    // 单文件上传
    stream: form,
});
```
![企业微信截图_1612234851549.png](https://i.loli.net/2021/02/02/zP53C9OZeaKfYq1.png)

成功发送。