# Ubuntu无法解析gitee的问题

## 问题描述

在ubuntu虚拟机下克隆gitee仓库时报错，无法解析gitee.com。

```bash
git clone --recursive https://gitee.com/blueskybones/riscv-gnu-toolchain.git
```


```bash
fatal: unable to access 'https://gitee.com/blueskybones/riscv-gnu-toolchain.git/': Could not resolve host: gitee.com
```

## 解决方法

先打开ip查询网站：[https://ipaddress.com](https://ipaddress.com)，查询gitee的IP地址。

修改 `/etc/hosts`文件中的内容，在其中添加gitee的ip地址。

```bash
sudo vim /etc/hosts
```

```bash
[ip地址] [域名]
154.213.2.253 gitee.com
```

然后可能需要重启网络服务，但是我没有重启就可以可以克隆gitee仓库了。

github的连接方案也是类似的，将github的ip地址和域名添加进hosts文件中即可。

## 参考

[Ubuntu下解决github无法解析的问题](https://blog.csdn.net/weixin_47266712/article/details/124760778)
