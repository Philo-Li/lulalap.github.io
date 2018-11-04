---
title: Atom下如何让光标自动移动到配对符号外
date: 2018-03-12 22:28:32
tags: Atom
categories: 日常折腾
---
Atom作为一款简洁优美的编辑器深受广大用户的喜爱，丰富的插件极大地提高了它的易用性。日常敲代码的人都知道，编辑器提供的符号自动配对（即括号成对输出：“”）虽然降低了出错率，但是最令人头疼的还是在输入完成后，需要再敲一个方向键才能将光标移动到括号外，而单纯移动手指是够不着方向键→的。

那么如何在Atom环境下解决光标自动移动问题？

<!--more-->

### 环境

* Windows 10 + 64位
* Atom

### 修改init script文件

* 在 Atom 编辑器中，用 Ctrl + Shift + P 呼出 Command Palette 窗口

* 在窗口搜索框输入 init script , 点击 Application: Open Your Init Script

* 在打开的文件里粘贴以下代码：
```bash
# move cursor across the ending symbols...
EndingSymbolRegex = /\s*[)}>\]/'"`;:=-]/
atom.commands.add 'atom-text-editor', 'custom:jump-over-symbol': (event) ->
  editor = atom.workspace.getActiveTextEditor()
  cursorMoved = false
  for cursor in editor.getCursors()
    range = cursor.getCurrentWordBufferRange(wordRegex: EndingSymbolRegex)
    unless range.isEmpty()
      cursor.setBufferPosition(range.end)
      cursorMoved = true
  event.abortKeyBinding() unless cursorMoved
```

### 修改快捷键键位

* 在 Atom 编辑器中，用 Ctrl + Shift + P 呼出 Command Palette 窗口
* 在窗口搜索框输入 keymap , 点击 Application: Open Your keymap
* 在打开的文件底部增加如下代码：
```bash
"atom-text-editor:not([mini])":
  "tab": "custom:jump-over-symbol"
```
* "tab" 可更换成自己习惯的其他快捷键，如"shift", "enter", 或者"shift-enter"

### 更新设置

* 在 Atom 编辑器中，用 Ctrl + Shift + P 呼出 Command Palette 窗口
* 输入 reload, 选择 Window: Reload

接下来就可以愉快地玩耍啦！

### 参考文章

[技巧卡：怎样让 Atom 将光标自动移动到配对符号外](https://jetorz.github.io/2017-02-07-Skill-Atom-auto-pair.html)
