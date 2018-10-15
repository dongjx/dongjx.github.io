---
layout:     post
title:      git config & credential-osxkeychain
subtitle:   
date:       2018-10-15
author:     BY annie dong
header-img: img/post-bg.jpg
catalog: true
tags:
    - git
    - credential-osxkeychain
    - two-factor authentication
---

- 确认已经安装了credential-osxkeychain  
  
```
> git credential-osxkeychain
# Test for the cred helper
Usage: git credential-osxkeychain <get|store|erase>
```
  
- 设置global的keychain    
`git config --global credential.helper osxkeychain`   

- 如果Mac系统如果不启用Keychain机制，也可以采用这种方式  
  
```
git config --global credential.helper cache
git config --global credential.helper store
```
  
- 设置git global config `git config --global -e`  

- 设置git local config  `git config --local -e`   

- 如果在GitHub中开启了2FA（two-factor authentication），那么在本地系统中输入GitHub账号密码时，不能输入原始的密码（即GitHub网站的登录密码），而是需要事先在GitHub网站中创建一个Personal access token，后续在访问代码仓库需要进行权限校验的时候，采用access token作为密码进行输入





