---
layout: post
title:  "bower 和 npm的区别"
date:   2017-03-01
excerpt: "bower和npm的区别"
tag:
- "bower"
- "npm"
comments: true
---

> 本文的目的不是为了详细解释两种包管理器的功能，而是大致描述一下两者的区别。

  * bower完全是为前端开发的，目标是**最小化下载文件**，如果有不同的内容依赖相同的模块，这个**模块不会重复下载**。
  * npm(node package manager)主要用于node模块管理，当然通过browserify也可用于前端。
     * npm2.x采用嵌套的方式安装所有依赖，每个内容依赖的模块都是单独下载的，不同内容之间不会互相影响，所以**同一个模块可能被下载很多次**，在整个项目中会出现很多冗余的代码。
     
        ```
        app----->A v1.0--->B v1.0
            |--->C v1.0--->B v2.0
            |--->D v1.0--->B v2.0
            |--->E v1.0--->B v1.0
        ```
        
     * npm3.x减少了结构树的深度和嵌套方式带来的冗余。如下结构树所示，A和E都依赖B v1.0版本，将B v1.0放到顶级依赖层，A和E不需要再安装B v1.0，公用顶级依赖层中的B v1.0就可以了。因为B v1.0处于顶级依赖层，所以C和D依赖的B v2.0不能再提升到顶级依赖层，C和D各自安装自己需要的B v2.0模块。**结构树中模块的层级取决于模块的安装顺序**，具体看[官方文档](https://docs.npmjs.com/how-npm-works/npm3-dupe)

        ```
        app----->A v1.0
            |--->B v1.0
            |--->C v1.0--->B v2.0
            |--->D v1.0--->B v2.0
            |--->E v1.0
        ```

一般项目中会同时使用不同的包管理工具，具体使用什么管理器需要看具体的应用场景。

