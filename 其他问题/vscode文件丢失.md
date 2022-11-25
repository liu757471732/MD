## vscode文件丢失

出现场景提交时不知道点击了什么导致文件丢失

### 第一种解决方案

1. 打开vscode控制台切换到输出你会看到类似的数据 

```
2022-11-25 02:19:29.649 [info] > git cat-file -s 974bfb7476b7f0266bff0072502089af0f7390f1 [30ms]
2022-11-25 02:19:30.336 [info] > git ls-files --stage -- C:\Users\刘\Desktop\Vue\vue3_admin\src\router\modules\components.ts [28ms]
2022-11-25 02:19:30.339 [info] > git show --textconv :src/router/modules/components.ts [34ms]
2022-11-25 02:19:30.373 [info] > git cat-file -s b85d20a630d2574432fe209f53b3bd024133521d [34ms]
```

2. 类似于**b85d20a630d2574432fe209f53b3bd024133521d**这个东西叫做blob是vscode用来记录当前文件的状态,我们可以通过查看文件然后把代码复制出来

```
git cat-file -p e84218b0d0c2ccc93b909cc7d72623a62d493e36
```

### 第二种解决方案

1. 打开vscode控制台查看你最近所操作的文件

```
2022-11-25 02:19:29.616 [info] > git ls-files --stage -- C:\Users\刘\Desktop\Vue\vue3_admin\src\router\modules\others.ts [27ms]
2022-11-25 02:19:29.619 [info] > git show --textconv :src/router/modules/others.ts [32ms]
2022-11-25 02:19:29.649 [info] > git cat-file -s 974bfb7476b7f0266bff0072502089af0f7390f1 [30ms]
2022-11-25 02:19:30.336 [info] > git ls-files --stage -- C:\Users\刘\Desktop\Vue\vue3_admin\src\router\modules\components.ts [28ms]
```

**1C:\Users\刘\Desktop\Vue\vue3_admin\src\router\modules\others.ts**这个就是最近操作的文件，然后再目录中新建他

2. 新建出来的文件夹要与之前的文件同名，创建完以后文件下面会有一个**时间线**时间线会记录最近这个文件修改的内容复制出来文件即可