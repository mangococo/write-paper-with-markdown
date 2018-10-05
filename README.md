# 描述

基于 Markdown 与 Pandoc 写一篇科技型论文。

## 一些解释
由于需要使用 Pandoc 以及其他一些开源项目（鸣谢：[pandoc-fignos](https://github.com/tomduck/pandoc-fignos) 等）进行格式转换，所以文章有多处并不符合 Markdown 语法标准。

该项目作为 Git 与 GitHub 学习之用途。


## 如何编译（使用 Pandoc 进行格式转换）

```shell
$ pandoc --filter pandoc-fignos  --filter pandoc-eqnos  --filter pandoc-tablenos --filter pandoc-citeproc --bibliography=myref.bib --csl=chinese-gb7714-2005-numeric.csl  --reference-doc=./template/template.docx demo.md -o demo.docx
```
> 说明：
  - 参数顺序会影响转换后的渲染效果。
  - 想要使用模板转换文档，应该先将template.md转换为template.docx，修改template.docx的样式。

### 参数说明

文中用到了基于 [tomduck](https://github.com/tomduck/) 所制作的表格索引 `--filter pandoc-tablenos`，公式索引 `--filter pandoc-eqnos`，图片索引 `--filter pandoc-fignos`。以及文献引用 `--filter pandoc-citeproc` (见 [pandoc-citeproc](https://github.com/jgm/pandoc-citeproc))，同时文献引用需要指明参考文献数据格式 `--csl=chinese-gb7714-2005-numeric.csl` 与存放位置 `--bibliography=myref.bib` 所以运行 pandoc 时需要添加如上参数。
