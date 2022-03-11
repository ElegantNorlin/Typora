### npm的粗略理解

乍一看的确不知道npm是做什么的，如果告诉你npm的全称：`node package manager`你一定知道这个做什么的。npm是随`node.js`一起安装的包管理工具。你可以通过`node -v`和`npm -v`来查看`node.js`和`npm`的版本。

常用命令：

```shell
# 运行服务器
npm run serve

# 本地安装项目
npm install
```

了解了npm，接下来继续了解一个与npm功能类似的工具`yarn`。



### yarn

yarn是facebook与Google等企业联合推出的新的JS的包管理工具，其文档中写道`Yarn 是为了弥补 npm 的一些缺陷而出现的`。



#### yarn安装

```shell
# 使用npm安装yarn工具
npm install -g yarn

# 查看yarn的版本号，同时查看是否安装成功
yarn --version
```



#### npm与yarn对应命令比较

| **npm**                      | yarn                 |
| ---------------------------- | -------------------- |
| npm install                  | yarn                 |
| npm install react --save     | yarn add react       |
| npm uninstall react --save   | yarn remove react    |
| npm install react --save-dev | yarn add react --dev |
| npm update --save            | yarn upgrade         |



