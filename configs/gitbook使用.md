# gitbook使用

## 安装

* gitbook 安装

```
npm install gitbook-cli -g
```

* 创建目录，切换到目录下，执行

```
gitbook init
```

会发现目录下面多了2个文件，**README.md**和**SUMMARY.md**

README.md 和 SUMMARY.md 是两个必须文件

README.md 是对书籍的简单介绍

SUMMARY.md 是书籍的目录结构

* 生成目录

 ```
gitbook init
 ```

执行 gitbook init 会根据 SUMMARY.md 目录生成对应的文件夹和 md 文件，每一个 md 文件对应每一章节，每一章节的内容在对应的 md 文件里编辑

如果想要新增章节，可以在 SUMMARY.md 里面新增，然后执行 gitbook init 就会新增对应的 md 文件，原有文件不会变化；如果想要删除章节，在 SUMMARY.md 里面删除，然后执行 gitbook init 想要删除的 md 文件并不会删除，需要手动删除。

* build输出

```
gitbook build
```

```
gitbook build . ./output

//output为要输出的目录，不写默认为_book目录

```



