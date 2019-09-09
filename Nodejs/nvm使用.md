## nvm 安装

### windows下
卸载已安装的 NodeJS，否则会发生冲突。然后下载 (nvm-windows)[https://github.com/coreybutler/nvm-windows/releases] 最新安装包，直接安装即可。

### OS X/Linxu

安装C++编译器
Linxu(Debian)
`sudo apt-get install build-essential`

OS X
`xcode-select --install`

使用


`curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash`
或者


`wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash`
从远程下载 install.sh 脚本并执行。

(官方最新安装命令)[https://github.com/creationix/nvm#install-script]

## 安装多版本 node/npm

安装10.0.0版本
`nvm install 10.0.0`

安装10.0系列最新版本
`nvm install 10.0`

列出远程服务器上所有可用版本
`nvm ls-remote`

Windows: `nvm ls available`

## 切换版本
当我们安装了一个新版本 Node 后，全局环境会自动把这个新版本设置为默认

使用 `nvm use` 命令切换版本
切换到 4.2.2
`nvm use 4.2.2`
切换到最新的 4.2.x
`nvm use 4.2`
切换到最新版
`nvm use node`

用 nvm 给不同的版本号设置别名：

`nvm alias awesome-version 4.2.2`
给 4.2.2 这个版本号起了一个名字叫做 awesome-version，然后我们可以运行：

`nvm use awesome-version`
下面这个命令可以取消别名：

`nvm unalias awesome-version`

列出已安装实例

`nvm ls`


## 在项目中使用不同版本的 Node
我们可以通过创建项目目录中的 .nvmrc 文件来指定要使用的 Node 版本。之后在项目目录中执行 nvm use 即可

从特定版本导入到我们将要安装的新版本 Node：

`nvm install v5.0.0 --reinstall-packages-from=4.2`

直接运行特定版本的 Node

`nvm run 4.2.2 --version`
在当前终端的子进程中运行特定版本的 Node

`nvm exec 4.2.2 node --version`
确认某个版本Node的路径

`nvm which 4.2.2`
安装 Node 的其他实现，例如 iojs（一个基于 ES6 的 Node 实现，现在已经和 Node 合并）

`nvm install iojs-v3.2.0`


安装最新版 Node
`nvm install node`

安装最新版 iojs
`nvm install iojs`

安装最新不稳定版本的 Node
`nvm install unstable` 

## 使用淘宝镜像源
1.临时使用

`npm --registry https://registry.npm.taobao.org install express`

2.持久使用
`npm config set registry https://registry.npm.taobao.org`

3.通过cnpm
`npm install -g cnpm --registry=https://registry.npm.taobao.org`
