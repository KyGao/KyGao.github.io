---
title: 记一次vim优化
date: 2020-01-29 10:50:43
tags: [tools,vim]
categories: tools
abbrlink: tools/vim
search: [vim,vundle,YouCompleteMe] 
description: "初次尝试修改vim插件，效果一般，想到暂时对vim的需求不大，故先搁置于此"
---

**起因**：因为实在受不了默认的hjkl上下左右，想要修改vim的快捷键，才看到vim竟然还有插件和配置这种东西，原来和zsh一样，看来是我才疏学浅了，于是临时兴起又随便照着别人的配置优化了一下，之后有需要再回来完善。


### 安装vundle

vundle是vim的插件管理器，参考官网安装即可：https://github.com/VundleVim/Vundle.vim

这里放一点vundle的常见操作：

- 安装插件
  1. vim中`:PluginInstall`
  2. 命令行`vim +PluginInstall +qall`
- 移除插件
  在.vimrc中注释掉对应的Plugin，保存退出后输入命令`:BundleClean`
- 更新插件
  `:BundleUpdate`
- 列出所有插件
  `:BundleList`
- 查找插件
  `:BundleSearch`



### 安装YouCompleteMe

官网地址：https://github.com/ycm-core/YouCompleteMe

官网杂七杂八的说太多了，我是参考这个博文安装的：

[YouCompleteMe 安装配置方法](https://zhuanlan.zhihu.com/p/35626421)

> 这里在安装（./install.py --all）的时候报错
>
> ```cmd
> File ~/.vim/bundle/YouCompleteMe/third_party/ycmd/build.py does not exist; you probably forgot to run:
>         git submodule update --init --recursive
> ```
>
> 按照他的提示执行
>
> `git submodule update --init --recursive`
>
> 即可
>

emmm因为安装太久了实在受不了，就参考官网说的只安装C的全家福了

`python3 install.py --clangd-completer`



### 配置.vimrc和其他插件

为了方便我直接用的一个博主的配置（**NERDTree**什么的直接通过这里就可以安装好了）

[Vim的终极配置方案，完美的写代码界面](https://blog.csdn.net/amoscykl/article/details/80616688)



### 主题选择

安装方式：在.vimrc中添加

```
Plugin 'jnurmine/Zenburn'
Plugin 'altercation/vim-colors-solarized'
```

诸如此类的字符串就当插件一样的安装了，但是在安装完成之后需要

```cmd
mkidr ~/.vim/colors
cp ~/.vim/bundle/Zenburn/colors/zenburn.vim ~/.vim/colors/
cp ~/.vim/bundle/vim-colors-solarized/colors/solarized.vim ~/.vim/colors/
```

这样才可以通过命令在控制台修改主题

**常见命令**

- 查看当前主题 
  `:colorscheme`
- 列出所有主题
  `:colorscheme [space] [tab]`

- 切换主题
  `:colorscheme [your-theme]`

  > 参考[Vim安装插件](https://juejin.im/entry/5b62cdc5e51d4519885635f0)


### Vim之用到哪学到哪
1. 键位映射
2. 复制粘贴
   `v` 进入view模式相当于 shift，可以选择ROI，然后选择完毕后按 `y` ，再在insert模式下按 `p` 就是粘贴
3. 撤销
   insert模式下 `:u`


> **注**：最后还是因为vim似乎不支持python2依赖所以YCM还是装不上，哎，反正暂时也不用vim写脚本，以后再说吧