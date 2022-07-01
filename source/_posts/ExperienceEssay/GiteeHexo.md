---
title: Mac + Hexo + Gitee Pages 搭建在线自生成静态Blog
date: 2021-08-31 17:20:46
categories:
  - 经验随笔
tags:
  - Mac
  - Hexo
  - Gitee Pages
  - Blog
---

## 简介

以前都是在本地生成静态站点，然后提交到代码库再更新 Blog，很不方便。最近看到 Gitee 支持 Hexo 在线生成，于是全部上传到了代码库中，这下可以随时随地记录了。

<!-- more -->

## 环境配置

1. git <https://git-scm.com>
2. node.js <https://nodejs.org/zh-cn>
3. hexo <https://hexo.io/zh-cn/docs>

## 部署

具体过程与部署步骤就不在这里说了，hexo 站点中很详细，这里主要说怎么部署到 Gitee

1. 按 hexo 文档本地部署并配置好项目，测试正常

2. 将自己的主题配置文件复制到 Blog 项目根目录

   > 修改文件名为: \_config.[theme].yml (_theme:主题文件夹名，一般是主题名称_)

3. 将自己的个性化静态文件复制到 source 目录下

   > 如我个性化了部分主题中的图片，于是将这些图片放到了 source 下的 images，在 config 中 使用路径为: /images/[imageName]

4. 上传代码到 Gitee

   > 配置文件和包文件
   > \_config.yml、\_comfig.[theme].yml、package.json
   > 及文件夹
   > scaffolds、source、themes

5. 添加 Blog 文章

6. 进入代码库 -> 服务 -> Pages，点击更新

### 大功告成

🎉 现在可以随时在 Gitee 中的 WebIDE 编写文章了！脱离了本地环境的限制，感觉自由 🆓 原来是这么的美好！

> **记得要更新 Blog 内容时去 Pages 再次点击更新哦！**

___

## 初始化

```console
 [npx] hexo init [folder]
```

## 添加文章

```console
[npx] hexo new [post] <title>
```

| 参数 | 描述 |
| :--- | :--- |
| -p, --path | 自定义新文章的路径 |
| -r, --replace | 如果存在同名文章，将其替换 |
| -s, --slug | 文章的 Slug，作为新文章的文件名和发布后的 URL|

## 生成静态文件

```console
[npx] hexo g | generate
```

| 选项 | 描述 |
| :-- | :-- |
| -d, --deploy | 文件生成后立即部署网站 |
| -w, --watch | 监视文件变动 |
| -b, --bail | 生成过程中如果发生任何未处理的异常则抛出异常 |
| -f, --force | 强制重新生成文件</br>Hexo 引入了差分机制，如果 public 目录存在，那么 hexo g 只会重新生成改动的文件。</br>使用该参数的效果接近 hexo clean && hexo generate |
| -c, --concurrency | 最大同时生成文件的数量，默认无限制 |

## 运行

```console
[npx] hexo server
```

| 选项 | 描述 |
| :- | :- |
| -p, --port | 重设端口 |
| -s, --static | 只使用静态文件 |
| -l, --log | 启动日记记录，使用覆盖记录格式 |

## 部署网站

```console
[npx] hexo d | deploy
```

| 选项 | 描述 |
| :- | :- |
| -g, --generate | 部署之前预先生成静态文件 |

## 渲染文件

```console
[npx] hexo render <file1> [file2] ...
```

| 选项 | 描述 |
| :- | :- |
| -o, --output | 设置输出路径 |

## 从其他博客系统 [迁移内容](https://hexo.io/zh-cn/docs/migration)

```console
[npx] hexo migrate <file1> [file2] ...
```

## 清除缓存

```console
[npx] hexo clean
```

## 列出网站资料

```console
[npx] hexo list <type>
```

## 显示 Hexo 版本

```console
[npx] hexo version
```
