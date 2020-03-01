---
title: Emacs tutorial 笔记
tags:
  - Emacs
categories:
  - tools
description: 最早是看yhm同学大力使用，然而自己学习主要是因为Coursera上面一门课程的要求，便也开始尝试一下传说中的编辑器（操作系统）
date: 2020-02-14 13:12:43
---

​		个人感觉Emacs官网的 [guide](https://www.gnu.org/software/emacs/tour/) 比emacs自带的tutorial清晰很多，可能主要是因为富文本的原因。这里我仅放上可能常用的操作作为索引。有更多需要的小伙伴请移步官网guide。

### Above all

扩展符C-x 跟单个character，M-x 跟单词


- **tutorial**

  `C-h t`
  
- **Abortion**
  `C-g`: cmd中的ctrl+z
  
- **CloseAndMinimize**
  `C-x C-c`: Close
  `C-z`: Minimize

### Basic editing commands

- **moving around in buffers**
crtl跟着的是文章结构上的跳动（逐行逐字母），meta跟着的是内容结构上的跳动（逐句逐单词）

|   操作    |                目的                 | 操作 |    目的    |
| :-------: | :---------------------------------: | :--: | :--------: |
|    C-f    |             下一个字母              | C-b  | 上一个字母 |
|    C-n    |               下一行                | C-e  |   上一行   |
|    C-a    |              回到行首               | C-e  |  回到行尾  |
|    M-f    |             下一个单词              | M-b  | 上一个单词 |
|    M-a    |               上一句                | M-e  |   下一句   |
|    C-v    |              page down              | M-v  |  page up   |
|    M-<    |                Home                 | M->  |    End     |
| C-l (L not 1) | 将光标位置移到Top/Bot/Cen |   M-g g 3   | 前往第3行           |
| C-s | 向后查询 | C-r | 向前查询         |
> 注：C-l 一般用两次即 C-l C-l，将当前行移到窗口顶端

- **Mark**
Mark几乎始终存在，在进行页面内跳转时跨度过大emacs都会自动标记原位置，只需要C-x C-x即可回去 

  <table style="text-align:center">
    <tr>
        <td>C-SPC</td> 
        <td>Set mark to the current location</td> 
   </tr>
   <tr>
        <td>C-x C-x</td> 
        <td>Swap point and mark</td> 
   </tr>


  </table>

- **Region**
  <table style="text-align:center">
    <tr>
        <td>C-x h</td> 
        <td>全选</td> 
        <td>M-h</td> 
        <td>选中该段</td> 
   </tr>
   <tr>
        <td>C-x n n</td> 
        <td>单独拎出目标区域</td> 
        <td>C-x n w</td> 
        <td>恢复原样</td> 
   </tr>
  </table>

- **Killing, Copying and Yanking**
  <table style="text-align:center">
    <tr>
        <td>C-k</td> 
        <td>删除该行</td> 
        <td>M-k</td> 
        <td>删除该句</td> 
   </tr>
   <tr>
        <td>C-w</td> 
        <td>剪切选中区域</td> 
        <td>M-w</td> 
        <td>复制选中区域</td> 
   </tr>
    <tr>
        <td>C-y</td> 
        <td>粘贴</td> 
        <td>M-y</td> 
        <td>粘贴剪贴板之前内容</td> 
   </tr>
  </table>

- **Undo**
  3种方式：
  `C-/`
  `C-_`
  `C-x u`
  
- **Incremental Search**
  <table style="text-align:center">
    <tr>
        <td>C-s</td> 
        <td>向后搜索</td> 
        <td>C-r</td> 
        <td>向前搜索</td> 
   </tr>
  </table>
  搜索之后再按 C-s 跳到下一项， Backspace 跳到前一项

  
  

### Buffers

正文部分是buffer，最下面一行是minibuffer
是Buffer不一定是文件，但是文件就一定是Buffer

- **FIles**
  <table style="text-align:center">
    <tr>
        <td>C-x C-f</td> 
        <td>Find 查找 (创建) 文件</td> 
    </tr>
    <tr>
        <td>C-x C-s</td> 
        <td>保存文件</td> 
    </tr>

</table>


- **Buffers**
  
  <table style="text-align:center">
    <tr>
        <td>C-x C-b</td> 
        <td>List Buffers</td> 
        <td>C-x b [Buffer]</td> 
        <td>前往指定Buffer</td> 
    </tr>
    <tr>
        <td>C-x 1</td> 
        <td>Kill All other Windows</td> 
        <td>C-x s</td> 
        <td>逐一保存所有Buffer</td> 
    </tr>
    <tr>
        <td>C-x k</td> 
        <td>关闭当前Buffer</td> 
    </tr>

</table>

- **Extending**
  
  - 替换
    M-x repl s<Return> (支持补全)
  - 格式化输入
    M-x text-mode
    M-x auto-fill-mode
    C-x f <digit>
  M-q :re-fill
  
- **Multiple Windows**
  <table style="text-align:center">
    <tr>
        <td>C-x 2</td> 
        <td>复制当前buffer建立新对话 (上下)</td> 
        <td>C-M-v</td> 
        <td>scroll 另一个窗口</td> 
    </tr>
    <tr>
        <td>C-x 4 C-f <Filename></td> 
        <td>新建对话打开文件 (左右)</td> 
        <td>C-x o</td> 
        <td>Jump to the other window</td> 
    </tr>
    <tr>
        <td>C-x 2 <Filename></td> 
        <td>上下扩展</td> 
        <td>C-x 3</td> 
        <td>左右扩展</td> 
    </tr>

</table>

- **Multiple Frames**
  Window是在同一个GUI里的，Frames是新建了一个GUI
  <table style="text-align:center">
    <tr>
        <td>C-x 5 2</td> 
        <td>复制当前buffer建立新窗口 (上下)</td> 
    </tr>
    <tr>
        <td>C-x 5 0 <Filename></td> 
        <td>新建窗口打开文件 (左右)</td> 
    </tr>

</table>

