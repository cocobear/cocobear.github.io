---
title: macOS 输入法状态管理
tags: [macos]
toc: true
comments: true
date: 2020-01-03 16:36:53
categories:
description: macOS 输入法状态管理
---



![尴尬](https://asset-1258390188.cos.ap-shanghai.myqcloud.com/input_python.png)

<!--more-->

## 前言

经常在多个应用之间切换，然后手速比较快，就总会遇到类似上面这样的问题：



我输入的是`python`, 因为不知道当前的输入法是中文，所以出现了上面的结果，我得按4次删除键来清除错误的输入。

为了解决这个问题，试过很多办法，目前综合了各种方案，终于有了一个比较满意的方案。

这个方案主要解决的问题有：

- 应用软件切换时提示输入法的状态

- 手动中英文切换时提示

- 自动根据运行软件切换输入法

  

基本有了以上功能，就可以无障碍的在各个应用软件之间输入文字了。为了达到上面的目的需要使用下面几个软件：

- **Karabiner**

- **Hammerspoon**

- **Squirrel**

  

## 实现方式


### 输入法配置

首先输入法使用的是：Squirrel(鼠须管)， 系统ABC，鼠须管关闭了英文模式，使用`Karabiner`将`shift`键映射为：`ctrl+space`。

本来鼠须管也有自己的英文输入，但是别的程序无法获取它的中英文状态，所以只使用了它的中文输入。修改`shift`映射是因为习惯了用它来切换中英文的状态，因为系统设置里面的输入法切换的快捷键没办法改为`shift`

鼠须管以及`Karabiner`的配置可以参考了[这篇]([https://github.com/rime/squirrel/wiki/%E7%A6%81%E7%94%A8-Squirrel-%E8%8B%B1%E6%96%87%E6%A8%A1%E5%BC%8F%EF%BC%8C%E4%BD%BF%E7%94%A8%E5%B7%A6%E4%BE%A7-Shift-%E5%88%87%E6%8D%A2%E4%B8%AD%E8%8B%B1](https://github.com/rime/squirrel/wiki/禁用-Squirrel-英文模式，使用左侧-Shift-切换中英)) 文章

### 手动中英文切换时提示

因为切换时用的是快捷键，所以可以用监听键盘按键的方式来进行提示，主要的代码如下：

```lua
  hs.eventtap.new({hs.eventtap.event.types.keyUp}, function(event)
      -- print(event:getKeyCode())
      -- print(hs.inspect.inspect(event:getFlags()))

     if event:getKeyCode() == 49  and event:getFlags().ctrl then
         showInputMethod(true)
     end
   end):start()

function showInputMethod(reverse)
   -- 用于保存当前输入法
    local currentSourceID = hs.keycodes.currentSourceID()
    local tag
    print(currentSourceID)
    hs.alert.closeSpecific(showUUID)

    if (currentSourceID == "com.apple.keylayout.ABC") then
        if reverse then
            tag = '中'
        else
            tag = 'A'
        end
    else
        if reverse then
            tag = 'A'
        else
            tag = '中'
        end
    end
    showUUID = hs.alert.show(tag, screens)
end
```

### 应用软件切换时提示输入法的状态

主要是通过`hs.application.watcher`来监听应用是否激活，然后根据应用的名字来进行处理。

```lua
function updateFocusAppInputMethod(appName, eventType, appObject)
    if (eventType == hs.application.watcher.activated) then
        local default = true
        for index, app in pairs(key2App) do
            if app[1] == appObject:path() then
                default = false
                if app[2] == 'Chinese' then
                    Chinese()
                else
                    English()
                end
            end
        end
        if default then
            English()
        end
    end
    showInputMethod()
end

appWatcher = hs.application.watcher.new(updateFocusAppInputMethod)
appWatcher:start()
```



### 自动根据运行软件切换输入法

第二步中一起做了



## 效果展示

![效果](https://asset-1258390188.cos.ap-shanghai.myqcloud.com/input.gif)



完整的代码在[github](https://github.com/cocobear/dotfiles/blob/master/.hammerspoon/init.lua), 参考了[这个](https://github.com/manateelazycat/hammerspoon-config/blob/master/init.lua)`Karabiner`配置。



## 问题

- 在iTerm中切换输入法有时候不会触发keyUp的消息，就没有状态提示了，重起后正常