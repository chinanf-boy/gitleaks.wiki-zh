# zricethezav/gitleaks.wiki [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 审核git存储库的密码，gitleaks 的 Wiki-维基页面 」

[中文](./readme.md) | [english](https://github.com/zricethezav/gitleaks.wiki)

---

## 校对 🀄️

<!-- doc-templite START generated -->
<!-- repo = 'zricethezav/gitleaks.wiki' -->
<!-- commit = 'a0946237020abaab87788bacfe5491f98e330f60' -->
<!-- time = '2018-10-27' -->

| 翻译的原文 | 与日期        | 最新更新 | 更多                       |
| ---------- | ------------- | -------- | -------------------------- |
| [commit]   | ⏰ 2018-10-27 | ![last]  | [中文翻译][translate-list] |

[last]: https://img.shields.io/github/last-commit/zricethezav/gitleaks.wiki.svg
[commit]: https://github.com/zricethezav/gitleaks.wiki/tree/a0946237020abaab87788bacfe5491f98e330f60

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

### 目录

<!-- START doctoc -->
<!-- END doctoc -->

<p align="center">
  <img alt="gitleaks" src="https://raw.githubusercontent.com/zricethezav/gifs/master/gitleaks5.png" height="140" />
  <p align="center">
      <a href="https://travis-ci.org/zricethezav/gitleaks"><img alt="Travis" src="https://img.shields.io/travis/zricethezav/gitleaks/master.svg?style=flat-square"></a>
  </p>
</p>

# 如何引导

这个指南将贯穿GITLACK的几个例子.

## 基本用法

Gitleaks提供了很多可调性,但是对于大多数用户来说,在默认模式下针对单个回购运行gitleaks就足够了.

### 1.与回购相抗衡

```
gitleaks --repo=https://github.com/gitleakstest/gronit
```

这个例子运行GITLACK针对我在GITHUB上托管的测试回购.Gronit包含两个AWS密钥泄露.

### 2.冗长模式

```
gitleaks --repo=https://github.com/gitleakstest/gronit -v
```

当GITLACK处理回购时,您可能希望查看审核的输出.打开冗长模式`-v`或`--verbose`

### 三.Redact模式

```
gitleaks --repo=https://github.com/gitleakstest/gronit -v --redact
```

也许你想知道哪一行包含秘密,但不想记录秘密内容.你可以使用`--redact`这将导致输出看起来像:

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

### 4.报告

```
gitleaks --repo=https://github.com/gitleakstest/gronit --report=gronit_results.csv
```

也许你想一个一个地对一批回购进行审计,并保存每个回购的报告.您可以通过使用`--report=`选择权.你的报告必须结束.`.csv`或`.json`.

### 5对抗GITLAB回购

```
gitleaks --repo=https://gitlab.com/relaxeaza/twoverflow.git 
```

GITLACKE不仅适用于GITHUB RePOS,而且适用于所有Git RePOS,只要您有一个有效的地址.

### 6.目标特定分支

```
gitleaks --repo=https://github.com/gitleakstest/gronit --branch=dev
```

### 7.螺纹加工

```
gitleaks --repo=https://github.com/gitleakstest/gronit --threads=8
```

这个`--threads=`选项指定生成的线程的最大数目.

## 吉图布

GITLACKS确实提供一些GITHUB专有功能,如所有者(ORG/用户)扫描以及PR扫描.这两个特征都需要用户设置.`GITHUB_TOKEN`在这些环境中,这些特性依赖于GITHUB API.在这里生成GITHUB API令牌:[HTTPS://Help.GiTHUBCOM/Toels/CalaTeN-Acto Access ToKE-For命令行](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)

### 8.与GITHUB组织竞争

```
gitleaks --github-org=gitleakstestorg
```

注意:您可能想使用`--disk`如果您正在审核的组织很大

### 9.与GITHUB组织竞争

```
gitleaks --github-org=gitleakstestorg
```

注1:您可能想使用`--disk`如果您正在审核的组织很大

注2:您可能想使用`--log=debug`看看你会审核哪一份.

### 10.与GITHUB用户一起运行

```
gitleaks --github-org=gitleakstest
```

注意:您可能想使用`--disk`如果您正在审核的用户是大的

注2:您可能想使用`--log=debug`看看你会审核哪一份.

### 11.逆袭公关

```
gitleaks --github-pr=https://github.com/gitleakstest/gronit/pull/1
```

这可以很容易地挂钩到CI过程与DOCKER…

```
docker run --rm --name=gitleaks -e GITHUB_TOKEN={your github token dont hardcode this!!!}  zricethezav/gitleaks --github-pr=https://github.com/gitleakstest/gronit/pull/1
```

设置`--github-pr=`选项不克隆整个回购.GITLACKE使用GITHUB API为PR中的每个提交生成补丁并审核这些提交.

### 12.与Github org运行,不包括叉

```
gitleaks --github-org=gitleakstestorg --exclude-forks
```

这将排除对Github Orgs和用户的叉上的审计.

## "高级"配置特征

让我们看看GITLACK提供的一些更先进的特性…我们将编辑GITLACK配置`.toml`文件.为了让GITLACK读取自定义配置,必须使用GITLACKE来运行`config=`选择或拥有`GITLEAKS_CONFIG`EVAR设置为您的路径`.toml`配置.

```
gitleaks --config=gitleaks.toml
```

### 13.配置——扫描X

GITLIKAX附带了一些扫描的默认秘密,但您可能需要添加更多的.您可以通过添加到`.toml`配置文件.

```toml
# gitleaks.toml
title = "gitleaks config"
[[regexes]]
description = "AWS"
regex = '''AKIA[0-9A-Z]{16}'''

# adding your own
[[regexes]]
description = "zachs secret"
regex = '''terces shcaz'''

# another one!
[[regexes]]
description = "1024-bit hexadecimal string (possible hash, key or token)"
regex = '''['\"][0-9a-fA-F]{256}['\"]'''
```

### 14.配置——Whitelisting文件

假设我们正在审核包含一些音频编辑软件的回购协议.如果我是一个博彩者,我敢打赌,回购有一些音频文件.我们希望忽略这些文件,因为我们可以安全地(不是真的)假设它们不包含秘密.我们该怎么做?只需提供GITLACK配置即可.

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

这个配置告诉GITLACK搜索AWS密钥而不包括`wav, wma, mp3, m4a, flac`审核中的文件.

### 15.配置-白名单提交

也许有一些你有意想要忽略的回购,不管出于什么原因.你可以这样做

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

此配置设置GITLACK搜索脸谱网密钥,但忽略提交.`21b...`和`927...`如果扫描进入这些SASS.

### 16.配置——Whitelisting regexes

你可能想搜索所有的AWS键,但是忽略它们的一个子集(我不知道你为什么要这么做……但你可以)

```toml
[[regexes]]
description = "AWS"
regex = '''AKIA[0-9A-Z]{16}'''

[whitelist]
regexes = [
  "AKAIMYFAKEAWKKEY",
]
```

使用此配置文件的审计将看到一行包含AWS密钥,然后检查是否存在任何正则表达式白名单.如果有正则表达式白名单,则忽略该行.在这种情况下,如果我们有一行包含`AKAIMYFAKEAWKKEY`然后,它将被忽略,而所有其他AWS密钥仍然会被拾起.

### 17.配置-白名单分支

如果您正在使用所有分支进行审计,那么您也可以使用白名单分支.`--all-ref`.

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
