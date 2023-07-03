# 先瞭解Vue2 里的内容，怎么用 Vue3 的方式写出来
## data——唯一需要注意的地方
整个 data 这一部分的内容，你只需要记住下面这一点。
以前在 data 中创建的属性，现在全都用 ref() 声明。
在 template 中直接用，在 script 中记得加 .value 。
