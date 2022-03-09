# Lerna

是一个用来优化托管在 git/npm 上的多 package 代码库的工作流的一个管理工具，可以让你在主项目下管理多个子项目，从而解决了多个包互相依赖，且发布时需要手动维护多个包的问题。

# 基本结构

可以见到项目里面有多个 package.json，这些子项目的 package.json 全部由 lerna.json 管理起来

```js
lerna-project/
    |-- packages/
        |-- packages-a/
            |-- ...
            |-- package.json
        |-- packages-b/
            |-- ...
            |-- package.json
    |-- ...
    |-- lerna.json
    |-- package.json
```

# 父项目构建

全局安装 lerna 命令

```bash
npm i -g lerna
```

将仓库转变为一个 lerna 仓库

```bash
lerna init
```

# 子项目构建

增加 2 个 packages

```bash
lerna create @eno/view
lerna create @eno/utils
```

# 父项目增加依赖

```bash
npm i chalk
```

# 子项目增加依赖

我们可以使用 lerna，分别给相应的 package 增加依赖模块，就不需要手动在子项目里面一个一个的安装

```bash
lerna add chalk
```

以前我们在根目录下 npm i 完之后，只会安装好依赖生成 node_modules 在根目录，有了 lerna 之后可以配合子项目的 package.json 统一管理子项目下的所有依赖 node_modules 的安装和删除

以前需要删除一个个子项目的依赖，现在只需要 lerna clean 一个命令即可

| 命令  | 描述                                                            |
| ----- | --------------------------------------------------------------- |
| lerna | bootstrap 安装依赖                                              |
| lerna | clean 删除各个包下的 node_modules                               |
| lerna | init 创建新的 lerna 库                                          |
| lerna | list 显示 package 列表                                          |
| lerna | changed 显示自上次 relase tag 以来有修改的包， 选项通 list      |
| lerna | diff 显示自上次 relase tag 以来有修改的包的差异， 执行 git diff |
| lerna | exec 在每个包目录下执行任意命令                                 |
| lerna | run 执行每个包 package.json 中的脚本命令                        |
| lerna | add 添加一个包的版本为各个包的依赖                              |
| lerna | import 引入 package                                             |
| lerna | link 链接互相引用的库                                           |
| lerna | create 新建 package                                             |
| lerna | publish 发布                                                    |

# 开发

以后别人拿到项目之后，只需要运行以下命令就可以把整个项目的依赖安装好

```bash
# 安装父/根项目依赖
npm i
# 安装子项目依赖
lerna bootstrap
```

# pnpm 代替

上面这种方式还能用 pnpm 的方案代替

先在项目根目录建立一个 pnpm 的 workspcae 配置文件 pnpm-workspace.yaml，意味着这是一个 monorepo：

```js
packages:
  # 所有在 packages/ 和 components/ 子目录下的 package
  - 'packages/**'
  - 'components/**'
  # 不包括在 test 文件夹下的 package
  - '!**/test/**'
```

然后直接在根目录 pnpm i 就可以代替原来的 npm i + lerna bootstrap
