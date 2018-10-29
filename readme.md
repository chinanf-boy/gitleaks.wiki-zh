# zricethezav/gitleaks.wiki [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 审核 git 存储库的密码，gitleaks 的 Wiki-维基页面 」

[中文](./readme.md) | [english](https://github.com/zricethezav/gitleaks/wiki)

---

## 校对 ✅

<!-- doc-templite START generated -->
<!-- repo = 'zricethezav/gitleaks.wiki' -->
<!-- commit = 'a0946237020abaab87788bacfe5491f98e330f60' -->
<!-- time = '2018-10-27' -->
翻译的原文 | 与日期 | 最新更新 | 更多
---|---|---|---
[commit] | ⏰ 2018-10-27 | ![last] | [中文翻译][translate-list]

[last]: https://img.shields.io/github/last-commit/zricethezav/gitleaks.wiki.svg
[commit]: https://github.com/zricethezav/gitleaks.wiki/tree/a0946237020abaab87788bacfe5491f98e330f60

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---


<p align="center">
  <img alt="gitleaks" src="https://raw.githubusercontent.com/zricethezav/gifs/master/gitleaks5.png" height="140" />
  <p align="center">
      <a href="https://travis-ci.org/zricethezav/gitleaks"><img alt="Travis" src="https://img.shields.io/travis/zricethezav/gitleaks/master.svg?style=flat-square"></a>
  </p>
</p>

# 指南

这个指南, 关于贯穿 Gitleaks 的几个例子.

### 目录

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [基本用法](#%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95)
  - [1.搞repo](#1%E6%90%9Erepo)
  - [2.日志模式](#2%E6%97%A5%E5%BF%97%E6%A8%A1%E5%BC%8F)
  - [3. Redact 模式](#3-redact-%E6%A8%A1%E5%BC%8F)
  - [4. 报告](#4-%E6%8A%A5%E5%91%8A)
  - [5 搞 Gitlab 的 repo](#5-%E6%90%9E-gitlab-%E7%9A%84-repo)
  - [6.目标特定分支](#6%E7%9B%AE%E6%A0%87%E7%89%B9%E5%AE%9A%E5%88%86%E6%94%AF)
  - [7. 线程数](#7-%E7%BA%BF%E7%A8%8B%E6%95%B0)
- [Github](#github)
  - [8.搞 Github 组织 （多余的）](#8%E6%90%9E-github-%E7%BB%84%E7%BB%87-%E5%A4%9A%E4%BD%99%E7%9A%84)
  - [9.搞 Github 组织](#9%E6%90%9E-github-%E7%BB%84%E7%BB%87)
  - [10.搞 Github 用户](#10%E6%90%9E-github-%E7%94%A8%E6%88%B7)
  - [11.搞 Github PR](#11%E6%90%9E-github-pr)
  - [12.搞 Github org ,不包括 forks的项目](#12%E6%90%9E-github-org-%E4%B8%8D%E5%8C%85%E6%8B%AC-forks%E7%9A%84%E9%A1%B9%E7%9B%AE)
- ["高级"配置特性](#%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE%E7%89%B9%E6%80%A7)
  - [13.配置——扫描 X](#13%E9%85%8D%E7%BD%AE%E6%89%AB%E6%8F%8F-x)
  - [14.配置—— 白名单-whitelist 文件](#14%E9%85%8D%E7%BD%AE-%E7%99%BD%E5%90%8D%E5%8D%95-whitelist-%E6%96%87%E4%BB%B6)
  - [15.配置-白名单commits](#15%E9%85%8D%E7%BD%AE-%E7%99%BD%E5%90%8D%E5%8D%95commits)
  - [16.配置——白名单 正则式](#16%E9%85%8D%E7%BD%AE%E7%99%BD%E5%90%8D%E5%8D%95-%E6%AD%A3%E5%88%99%E5%BC%8F)
  - [17.配置-白名单分支](#17%E9%85%8D%E7%BD%AE-%E7%99%BD%E5%90%8D%E5%8D%95%E5%88%86%E6%94%AF)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 基本用法

Gitleaks 提供了很多可调性,但是对于大多数用户来说,在默认模式下, 仅运行 gitleaks 搞单个repo就足够了.

### 1.搞repo

```
gitleaks --repo=https://github.com/gitleakstest/gronit
```

这个例子运行 Gitleaks 搞我在 Github 上托管的测试repo。其中gronit 包含两个 AWS 密钥泄露.

### 2.日志模式

```
gitleaks --repo=https://github.com/gitleakstest/gronit -v
```

当 Gitleaks 处理repo时,您可能希望查看审核的输出.打开日志模式`-v`或`--verbose`

### 3. Redact 模式

```
gitleaks --repo=https://github.com/gitleakstest/gronit -v --redact
```

也许你想知道哪一行包含密码,但不想记录密码内容.你可以使用`--redact`，这将导致输出看起来像:

```
{
   "line": "REDACTED",
   "commit": "cb5599aeed261b2c038aa4729e2d53ca050a4988",
   "offender": "REDACTED",
   "reason": "AWS",
   "commitMsg": "fake key",
   "author": "Zachary Rice \u003czricethezav@users.noreply.github.com\u003e",
   "file": "main.go",
   "branch": "refs/heads/master",
   "repo": "gronit"
}
```

### 4. 报告

```
gitleaks --repo=https://github.com/gitleakstest/gronit --report=gronit_results.csv
```

也许你想一个一个地对一批repo进行审计,并保存每个repo的报告.您可以通过使用`--report=`选项.你的报告必须以`.csv`或`.json`结束.

### 5 搞 Gitlab 的 repo

```
gitleaks --repo=https://gitlab.com/relaxeaza/twoverflow.git
```

Gitleakss 不仅适用于 Github repos,而且适用于所有 Git repos,只要您有一个有效的地址.

### 6.目标特定分支

```
gitleaks --repo=https://github.com/gitleakstest/gronit --branch=dev
```

### 7. 线程数

```
gitleaks --repo=https://github.com/gitleakstest/gronit --threads=8
```

这个`--threads=`选项，指定生成的线程的最大数目.

## Github

Gitleakss 确实提供一些 Github 专有功能,如所有者(org/user)扫描以及 PR 扫描.这两个功能都需要用户设置`Github_TOKEN`环境变量,这些特性依赖于 Github API. 在这里生成 Github API 令牌:[HTTPS://Help.GiTHUBCOM/Toels/CalaTeN-Acto Access ToKE-For 命令行](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)

### 8.搞 Github 组织 （多余的）

> Wiki问题，

```
gitleaks --github-org=gitleakstestorg
```

注意:您可能想使用`--disk`，如果您正在审核的组织很大

### 9.搞 Github 组织


```
gitleaks --github-org=gitleakstestorg
```

注 1:您可能想使用`--disk`，如果您正在审核的组织很大

注 2:您可能想使用`--log=debug`看看你会审核哪一份.

### 10.搞 Github 用户

```
gitleaks --github-user=gitleakstestuser
```

注 1:您可能想使用`--disk`，如果您正在审核的用户很大的

注 2:您可能想使用`--log=debug`看看你会审核哪一份.

### 11.搞 Github PR

```
gitleaks --github-pr=https://github.com/gitleakstest/gronit/pull/1
```

这个，Docker很容易地挂钩到 CI 过程

```
docker run --rm --name=gitleaks -e Github_TOKEN={your github token dont hardcode this!!!}  zricethezav/gitleaks --github-pr=https://github.com/gitleakstest/gronit/pull/1
```

设置`--github-pr=`选项,不克隆整个repo。 Gitleaks使用 Github API 为 PR 中的每个提交生成补丁-patch，并审核这些提交.

### 12.搞 Github org ,不包括 forks的项目

```
gitleaks --github-org=gitleakstestorg --exclude-forks
```

这将排除对 Github Orgs 和用户的Fork项目的审计.

## "高级"配置特性

让我们看看 Gitleaks 提供的一些更先进的特性… 我们将编辑 Gitleaks 配置`.toml`文件。为了让 Gitleaks 读取自定义配置,必须使用 Gitleaks 来运行`config=`选项或具备`GITLEAKS_CONFIG`环境设置成您`.toml`配置路径.

```
gitleaks --config=gitleaks.toml
```

### 13.配置——扫描 X

Gihleaks 附带了一些扫描的默认密码,但您可能需要添加更多的.您可以添加，到`.toml`配置文件.

```toml
# gitleaks.toml
title = "gitleaks config"
[[regexes]]
description = "AWS"
regex = '''AKIA[0-9A-Z]{16}'''

# 添加自定义
[[regexes]]
description = "zachs secret"
regex = '''terces shcaz'''

# 更多 自定义!
[[regexes]]
description = "1024-bit hexadecimal string (possible hash, key or token)"
regex = '''['\"][0-9a-fA-F]{256}['\"]'''
```

### 14.配置—— 白名单-whitelist 文件

假设我们正在审核包含一些音频编辑软件的存储库.如果我是一个赌徒,我敢打赌,这repo有一些音频文件。而我们希望忽略这些文件,因为我们可以安全地(不是真的)假设它们不包含密码。我们该怎么做? 只需提供 Gitleaks 配置即可.

```toml
# gitleaks.toml
title = "gitleaks config"
[[regexes]]
description = "AWS"
regex = '''AKIA[0-9A-Z]{16}'''

[whitelist]
files = [
  "(.*?)(wav|wma|mp3|m4a|flac)$"
]
```

这个配置告诉 Gitleaks 搜索 AWS 密，,而审核不包括`wav, wma, mp3, m4a, flac`文件.

### 15.配置-白名单commits

也许repo有一些commits，你有意想要忽略, 不管出于什么原因.你可以这样做

```toml
[[regexes]]
description = "Facebook"
regex = '''(?i)facebook.*['\"][0-9a-f]{32}['\"]'''

[whitelist]
commits = [
    "21b59fab5d01942b389fcd6573bd17c61a1077fe",
    "9272e1e556ca6a6721fedf7beb0066be5a55c6e3",
]
```

此配置设置 Gitleaks 搜索脸谱网密钥,如果扫描进入这些 SHA 码，忽略提交`21b...`和`927...`的SHA码.

### 16.配置——白名单 正则式

你可能想搜索所有的 AWS key, 但是忽略它们的一个子集(我不知道你为什么要这么做……但你可以)

```toml
[[regexes]]
description = "AWS"
regex = '''AKIA[0-9A-Z]{16}'''

[whitelist]
regexes = [
  "AKAIMYFAKEAWKKEY",
]
```

使用此配置文件的Gitleaks审计，将看到一行包含 AWS 密钥, 然后检查是否存在相关的正则表达式白名单. 如果有正则表达式白名单,则忽略该行.在这种情况下,如果我们有一行包含`AKAIMYFAKEAWKKEY`，那,它将被忽略,而所有其他 AWS 密钥仍然会被拾起.

### 17.配置-白名单分支

如果您正在对所有分支进行审计,那么您也可以使用白名单分支`--all-ref`.

```toml
[[regexes]]
description = "AWS"
regex = '''AKIA[0-9A-Z]{16}'''

[whitelist]
branches = [
  "develop",
  "preview",
]
```
