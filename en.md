<p align="center">
  <img alt="gitleaks" src="https://raw.githubusercontent.com/zricethezav/gifs/master/gitleaks5.png" height="140" />
  <p align="center">
      <a href="https://travis-ci.org/zricethezav/gitleaks"><img alt="Travis" src="https://img.shields.io/travis/zricethezav/gitleaks/master.svg?style=flat-square"></a>
  </p>
</p>

# How To Guide
This guide will run through a few examples of gitleaks.

## Basic Usage
Gitleaks comes packed lots of tunability, but for the majority of users running gitleaks in default mode against a single repo is enough.
### 1. Running against a repo
```
gitleaks --repo=https://github.com/gitleakstest/gronit
```
This example runs gitleaks against a test repo I set up hosted on github. Gronit contains two AWS keys leaked.

### 2. Verbose mode
```
gitleaks --repo=https://github.com/gitleakstest/gronit -v
```
You may want to view the output of the audit as gitleaks processes the repo(s). Turn on verbose mode with `-v` or `--verbose`
### 3. Redact mode
```
gitleaks --repo=https://github.com/gitleakstest/gronit -v --redact
```
Maybe you want to know which lines contain secrets but don't want the secret content logged. You can use `--redact` which will result in output looking like: 
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
### 4. Reports
```
gitleaks --repo=https://github.com/gitleakstest/gronit --report=gronit_results.csv
```
Perhaps you want to run an audit on a bunch of repos one by one and save reports for each repo. You can accomplish this by using the `--report=` option. Your report must end in `.csv` or `.json`.

### 5 Running against a gitlab repo
```
gitleaks --repo=https://gitlab.com/relaxeaza/twoverflow.git 
```
Gitleaks works not only for github repos but for all git repos so long as you have a valid address.

### 6. Target specific branches
```
gitleaks --repo=https://github.com/gitleakstest/gronit --branch=dev
```

### 7. Threading
```
gitleaks --repo=https://github.com/gitleakstest/gronit --threads=8
```
The `--threads=` option specifies the max number of threads spawned.


## Github
Gitleaks does offer some Github exclusive features such as owner (org/user) scanning as well as PR scanning. Both of these features require the user to set `GITHUB_TOKEN` in their environment as these features depend on the Github API. Generate a github api token here: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/

### 8. Running against a Github Organization
```
gitleaks --github-org=gitleakstestorg
```
NOTE: you may want to use `--disk` if the organization you are auditing is large 

### 9. Running against a Github Organization
```
gitleaks --github-org=gitleakstestorg
```
NOTE1: you may want to use `--disk` if the organization you are auditing is large 

NOTE2: you may want to use `--log=debug` to see which repos you will be auditing.

### 10. Running against a Github User
```
gitleaks --github-org=gitleakstest
```
NOTE: you may want to use `--disk` if the user you are auditing is large 

NOTE2: you may want to use `--log=debug` to see which repos you will be auditing.

### 11. Running against a Github PR
```
gitleaks --github-pr=https://github.com/gitleakstest/gronit/pull/1
```
This could be easily hooked into a CI process with docker... 
```
docker run --rm --name=gitleaks -e GITHUB_TOKEN={your github token dont hardcode this!!!}  zricethezav/gitleaks --github-pr=https://github.com/gitleakstest/gronit/pull/1
```
Setting the `--github-pr=` option does not clone the entire repo. Gitleaks uses the github API to generate patches for each commit in the PR and audits those commits.

### 12. Running against a Github Org and Excluding forks
```
gitleaks --github-org=gitleakstestorg --exclude-forks
```
This will exclude audits on forks for github orgs and users

## "Advanced" Config Features
Let's take a look at some of the more advanced features gitleaks has to offer... We will be editing a gitleaks config `.toml` file. In order for gitleaks to read the custom config you must run gitleaks with the `config=` option or have `GITLEAKS_CONFIG` env var set to the path of your `.toml` config.
```
gitleaks --config=gitleaks.toml
```

### 13. Config -- Scanning for X
Gitleaks comes loaded with a few default secrets to scan for, but you might want to add more. You can do this by adding to the regexes in a `.toml` config file.
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

### 14. Config -- Whitelisting files
Say we are auditing a repo containing some audio editing software. If I were a betting man, I'd bet that the repo has some audio files. We want to ignore these files as we can safely (not really) assume they do not contain secrets. How do we do this? Simply supply a gitleaks config.

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
This config tells gitleaks to search for AWS keys and not to include `wav, wma, mp3, m4a, flac` files in the audit. 

### 15. Config -- Whitelisting commits 
Maybe there is some repo that you purposely want to ignore some commits for whatever reason. You can do this with 
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
This config sets gitleaks to search for facebook keys but ignore commits `21b...` and `927...` if the scan runs into those SHAs.


### 16. Config -- Whitelisting regexes
You might want to search for all AWS keys but ignore a subset of them (im not sure why you would do this... but you might)
```toml
[[regexes]]
description = "AWS"
regex = '''AKIA[0-9A-Z]{16}'''

[whitelist]
regexes = [
  "AKAIMYFAKEAWKKEY",
]
```
An audit with this config would see that a line contains an AWS key, then check if there are any regex whitelists. If there are regex whitelists, then ignore that line. In this case, if we have a line containing `AKAIMYFAKEAWKKEY`, then it would be ignored while all other AWS keys would still be picked up.


### 17. Config -- Whitelisting branches
You can also whitelist branches if you are running an audit against all branches by using `--all-ref`.
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