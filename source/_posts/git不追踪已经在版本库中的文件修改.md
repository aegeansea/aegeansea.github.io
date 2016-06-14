---
title: git不追踪已经在版本库中的文件修改
date: 2016-03-23 09:09:33
tags:
    - git
---
### 取消追踪命令
``` bash
    git update-index --assume-unchanged filePath
```

### 还原追踪命令
``` bash
    git update-index --no-assume-unchanged filePath
```