# Git打Tag相关操作

https://www.jianshu.com/p/dab7da2a0721

‌

项目的版本管理中,每当一个release版本发布时,需要做一个记录,以便以后需要的时候重新打包这个版本,这时候就用到tag这个功能.

‌

[更详细内容](https://links.jianshu.com/go?to=http%3A%2F%2Fblog.csdn.net%2Fwangjia55%2Farticle%2Fdetails%2F8793577)

‌

# **打标签**



```
git tag -a 0.1.3 -m “Release version 0.1.3″
```

‌

### **详解：**



```
git tag 是命令
-a 0.1.3是增加 名为0.1.3的标签
-m 后面跟着的是标签的注释
```

‌

打标签的操作发生在我们commit修改到本地仓库之后。

‌

# **相关操作**

‌

### **提交**



```
git add .
git commit -m “fixed some bugs”
git tag -a 0.1.3 -m “Release version 0.1.3″
```

‌

# **分享提交标签到远程服务器上**



```
git push origin master
git push origin --tags
```

‌

–tags参数表示提交所有tag至服务器端，普通的git push origin master操作不会推送标签到服务器端。

‌

# **切换到已有Tag**



```
git tag --list  // 查看已有tag列表
git checkout [tag/branch/commit]  // 切换到指定tag/branch/commit都是此命令
```

‌

# **删除标签的命令**



```
git tag -d 0.1.3
```

‌

# **删除远端服务器的标签**



```
git push origin :refs/tags/0.1.3
```
