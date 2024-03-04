# fankwang98_wiki
Wiki of frankwang98 for guys（内含C++修炼合集）

[Mkdocs Wiki搭建参考](http://t.csdnimg.cn/UsbyW)

[格式与排版参考](https://oi-wiki.org/)

## Build
```shell
# 安装mkdocs
pip install mkdocs
pip install mkdocs-awesome-pages-plugin python-markdown-math # 安装对应插件
pip install mkdocs-material # 可通过添加子模块安装主题

# 其他
git submodule add <URL of the repository> <local directory> # 添加子模块
git submodule update --remote # 更新子模块
git clone --recursive <URL of the repository> # 递归克隆
```

main与gh-pages分支是独立的，分别维护源码和wiki。