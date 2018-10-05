---
pandoc_args: ['--filter=pandoc-fignos', '--filter=pandoc-eqnos', '--filter=pandoc-tablenos', '--filter=pandoc-citeproc', '--bibliography=..\myref.bib', '--csl=..\chinese-gb7714-2005-numeric.csl']
title: "Word Templates"
author: "董进"
date: "Oct 5, 2019"
output: word_document
---

---
fignos-cleveref: TRUE
fignos-plus-name: 图
fignos-caption-name: 图

tablenos-cleveref: TRUE
tablenos-caption-name: 表格
tablenos-plus-name: 表
...

> This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details ...

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details ...

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details ...

# 文献引用
饮水机在家庭、办公场所市场潜力巨大，然而传统的上电后独立工作的饮水机已经不能满足信息时代的需求，智能化和网络化是其发展的必然趋势。[@夏鲲2018基于物联网的智能饮水机系统设计与实现]

对于只能饮水机的研究，其他研究者已经针对“如何让饮水机功能更多元化”做出了足够多的付出与贡献。

# 图像
LORA 调制解调器具有两种数据包模式：显示模式和隐式模式。区别在于，显示数据包模式有一个包含字节数、编码率以及数据包是否启用 CRC 校验等信息的报头。LORA 数据包(如{@fig:loradatagram})主要包含前导码、可选报头、数据有效负载和负载的 CRC校验。

![LORA 数据包结构](https://i.loli.net/2018/09/25/5ba9d0045b28e.jpg){#fig:loradatagram}

# 公式
利用公式 ({@eq:L_s}) 计算 LORA 符号速率：

$$
L_s=\frac {2^{sf}} {BW}
$$ {#eq:L_s tag="2.1"}

# 表格

|Name|B|C|
|:---|---|----|
|hw|123|1212|
|tw|123|1212|
Table: 测试. {#tbl:test tag="2.1"}

例如{@tbl:test}

## 代码

```java
public class Demo {
    public static void main(String[] args) {
      System.out.println("Demo");
    }
}
```

## 引用
