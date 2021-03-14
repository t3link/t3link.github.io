+++
title = "github pages 文件夹使用波浪号"

[taxonomies]
categories = ["书签"]
tags = ["github-pages", "zola"]

[extra]
toc = false
+++

目前站点托管在 [github pages](https://docs.github.com/en/github/working-with-github-pages) 上，静态文件生成用的是 [zola](https://github.com/getzola/zola)。昨天看到一个链接中带有 `~` 波浪号，觉得挺有意思的，就想在自己的网站中也用上，所以把文字目录文件夹和图片资源文件夹都以 `~` 开头，但是发布到 `github pages` 后发现带有 `~` 波浪号这种特殊符号的链接都 `404` 。

<!-- more -->

其实 `~` 在 [RFC 3986](https://tools.ietf.org/html/rfc3986#section-2.2) 规范中并不属于预留关键字。
> Characters that are allowed in a URI but do not have a reserved
> purpose are called unreserved.  These include uppercase and lowercase
> letters, decimal digits, hyphen, period, underscore, and tilde.
> 
> unreserved  = ALPHA / DIGIT / "-" / "." / "_" / "~"

可以看到 `~` 是允许使用的，而且一般是用来标识目录。但是 `github pages` 托管就出现了问题。

几番搜索终于找到了一个解决方案

[/t/unable-to-access-resources-in-folder-with-name-starting-with/10505](https://github.community/t/unable-to-access-resources-in-folder-with-name-starting-with/10505)

> GitHub Pages uses Jekyll by default to build Pages sites. Because of the way that Jekyll works, any files or folders that start with _, ., # or end with ~ are not built:
>
> [https://help.github.com/articles/files-that-start-with-an-underscore-are-missing/](https://help.github.com/articles/files-that-start-with-an-underscore-are-missing/)
>
> If you’re not using Jekyll, you can add an empty file named `.nojekyll` to the root of your publishing source to avoid this.

所以对于 `zola` 的解决方案就是在源分支 `master` 的 `static` 文件夹里面添加一个空文件 `.nojekyll`
这样通过 `github actions` 发布到 `gh-pages` 分支的时候就会被自动复制到根目录下

我原来一直以为 `github pages` 背后就只是一个像 `nginx` 一样的 `web` 服务器，没想到默认还有 `jekyll` 这一层。`zola` 生成的静态 `html` 会再被 `jekyll` 构建部署。