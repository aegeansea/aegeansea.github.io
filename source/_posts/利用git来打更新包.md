---
title: 利用git来打更新包
date: 2016-08-28 17:48:08
tags: [git,打包]
---
```bash
git archive -o update.zip NEW_COMMIT_ID_HERE $(git diff –name-only OLD_COMMIT_ID_HERE NEW_COMMIT_ID_HERE);
git archive -o update.zip 4d5baf6 $(git diff –name-only 438eac0 4d5baf6);
```

OLD_COMMIT_ID_HERE仅作为打包的起始点，但并不包含OLD_COMMIT_ID_HERE提交的内容。

当然NEW_COMMIT_ID_HERE 和OLD_COMMIT_ID_HERE之间可以间隔多个COMMIT的，这样就会打多个COMMIT的内容打包到一个压缩包内。

### 参考
[利用git来打更新包](http://www.58maisui.com/2016/08/28/206/)
