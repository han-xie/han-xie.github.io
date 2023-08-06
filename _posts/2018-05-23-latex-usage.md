---
layout: post
title: 【笔记】使用LaTex撰写毕业论文
tags: [note]
---

# 笔记

1. 当bibliographystyle是chicago而引用命令\cite用的宏包是natbib的时候，可能会出现Multiple citation on page xx: same authors and year(natbib) without distinguishing extra letter,(natbib) appears as question mark.报错。
比较方便的办法是最后修改bbl文件
但是人工去找修改bbl文件中哪些需要修改也是一件很麻烦的事情，可以将chicago.bst文件复制成mychicago.bst文件，然后第982行原来的`{ author my.full.label }`修改成`{ "AUTHOR_natbib" }`，这样生成的bbl文件中前面full author names会变成AUTHOR_natbib，可以在这个文件中搜索有多少个年份添加了a，然后就可以在原来的那个上面手动添加了。<br>
另外，有人建议使用biblatex包，参考[这里](https://tex.stackexchange.com/questions/95185/bibtex-chicago-style-citation-with-same-first-authors)。


2. 各种不同的cite最后输出的样式：查看[这里](https://en.wikibooks.org/wiki/LaTeX/Bibliography_Management)。

3. 大的bra ket怎么弄：查看[这里](https://tex.stackexchange.com/questions/108767/big-angle-brackets)。

4. nomenclature怎么弄：查看[这里](https://tex.stackexchange.com/questions/223376/how-to-make-section-in-nomenclature)，book或者report下怎么改header为nomenclature：查看[这里](https://tex.stackexchange.com/questions/31586/nomenclature-as-a-chapter)

5. List of figures/tables并且放入目录：查看[这里](https://texblog.org/2013/04/29/latex-table-of-contents-list-of-figurestables-and-some-customizations/)

# 后记

本文最早发表在新浪博客【[原文链接](https://blog.sina.com.cn/s/blog_86e874d30102xnga.html)】。当时我把新浪博客当作个人笔记使用，2022年4月新浪博客已关闭访问，标志着一个时代的结束。
