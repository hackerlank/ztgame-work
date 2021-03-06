# svn 忽略文件、文件夹

svn 忽略文件、文件夹是通过设置**文件夹**的 svn:ignore 属性来实现的，所以只需要将忽略文件的列表添加至对应文件夹的 svn:ignore 属性中即可。


### 设置 svn:ignore 属性

```shell
svn propset svn:ignore "*.o
> *.a
> *.tmp
> *.log
> tmp
> " .
```

* 这里的命令是通过多行输入的，用换行符分隔各个要忽略的文件、文件夹
* 支持通配符
* 文件夹名不能带 `/`
* 命令最后的 `.` 代表设置的是当前文件夹，当然可以指定为其他文件夹

当然，还可以通过某个文件来导入这些忽略的文件、文件夹

```shell
svn propset -F .svnignore .
```

* 这里的文件 `.svnignore` 中包含了要忽略的文件清单
* 文件清单中各个文件、文件夹以换行符分隔
* 文件清单中的文件夹名不能带 `/`
* 命令最后的 `.` 代表设置的是当前文件夹，当然可以指定为其他文件夹


### 忽略文件、文件夹脚本

根据上面的特性，我们可以写一个脚本来自动完成更新 svn:ignore 的工作

```
#!/bin/bash
# @file svnignore.sh
# @brief 设置 svn:ignore 的脚本，每次更新 .svnignore 文件后请执行这个脚本

for ignorefile in $(find . -iname .svnignore)
do
    svn propset svn:ignore -F $ignorefile $(dirname $ignorefile)
done
```

现在要忽略某些文件时，只需要在对应文件夹下添加 `.svnignore` 文件，并将要忽略的文件以换行符分隔添加到 这个 `.svnignore` 文件中，再执行一下 `./svnignore.sh` 脚本就OK了，最后需要使用 `svn commit` 将需改的 `svn:ignore` 属性提交。


### svn 属性相关操作

*  获取属性列表

    ```shell
    svn proplist
    ```
    打印当前文件夹的属性清单，如果设置了 `svn:ignore` 属性会打印出来。


* 获取 svn:ignore 清单

    ```shell
    svn propget svn:ignore
    ```

* 删除 svn:ignore 属性

    ```shell
    svn propdel svn:ignore
    ```