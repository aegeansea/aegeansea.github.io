---
title: Sublime 保存时自动转换tab成空格
date: 2016-07-20 15:17:04
tags: [自动转换]
---
每次保存前手动转换实在太烦人，下面这个脚本可以帮到你。
<!-- more -->
1. 打开sublime的Preference -> Browser Packages ...

2. 新建一个目录ExpandTabsOnSave

3. 新建文件ExpandTabsOnSave.py

4. 把下面内容复制进去，保存

```python
import sublime, sublime_plugin, os

class ExpandTabsOnSave(sublime_plugin.EventListener):
  def on_pre_save(self, view):
    if view.settings().get('expand_tabs_on_save') == 1:
      view.window().run_command('expand_tabs')
```

5.如果你想只是应用于当前项目，在 .sublime-project文件下添加：
```json
"settings": {
    "expand_tabs_on_save": true
}
```

6.全局改变，打开Preferences -> Settings - User添加：
```json
"settings": {
    "expand_tabs_on_save": true
}
```

### 参考
[Sublime 保存时自动转换tab成空格](https://www.douban.com/note/305605712/)
[Convert Tabs to Spaces on file save](https://coderwall.com/p/zvyg7a/convert-tabs-to-spaces-on-file-save)
