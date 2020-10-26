# LaTeX 个人经验 
一直以来只用word，第一次接触LaTeX是写了个英文简历，也是套用的模板，稍作修改。这次因为论文投稿的期刊推荐用LaTeX而对用Word编辑有诸多限制要求，便开始了小白学LaTeX的升级打怪之路。
## 1. Word 转 LaTeX
推荐很好的一篇文章
[Latex小白入门——如何迅速完成论文从word到Latex的移植](https://blog.csdn.net/tmylzq187/article/details/51361886?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~first_rank_v2~rank_v25-4-51361886.nonecase&utm_term=latex%E5%8F%AF%E4%BB%A5%E5%BF%AB%E9%80%9F%E5%B0%86%E8%AE%BA%E6%96%87%E5%AF%BC%E5%85%A5%E5%90%97&spm=1000.2123.3001.4430)。
不过由于我投的期刊要求图片单独上传而非嵌入文本中，改图片格式和插入这个步骤就省略了。而最耗费时间的则是表格的绘制，简直是我用LaTeX以来最糟糕的体验，后文也会着重介绍绘制表格的各种注意事项和技巧经验。
## 2. 基本
### 2.1 文件格式

扩展名  | 文件类型      | 用途
---     |---            |---
.tex    | 主文件        | 正文文本及设置
.cls    | 格式文件      | 定义格式
.bib    | 引用文件      | 储存引用信息
.bst    | 引用格式文件  | 定义引用格式

### 2.2 tex结构
我个人把tex文件结构分为两部分：

（1）头部

头部里定义了模板，使用的包和其它一些设置信息
```
\documentclass[review]{elsarticle}
\usepackage{lineno,hyperref}
\modulolinenumbers[5]
\journal{Composite Structures}
\bibliographystyle{elsarticle-num}
```


（2）主体

主体部分以\begin{document}开始，并以\end{document}结束。以\section{},\subsection{}及\subsubsection定义各个分段并显示各级标题，我使用的模板会对各级标题自动编号
```
\begin{document}
\section{Introduction}
...
\section{Experimental Program and Results}
\subsection{Coupon-level Static Tests}
...
\end{document}
```
## 3. 数学公式
数学公式是LaTeX的一大亮点，可分为两类：
### 3.1 文内公式
文内公式用一对$包括起来，如
```math
E = mc^2
```
可用

```
$E = mc^2$
```
详细函数符号参见： [LaTex命令汇总](https://blog.csdn.net/young951023/article/details/79601664)
### 3.2 块公式
块公式可用一对$$包括起来，和文内公式不同的是，它会单独成行并居中对齐。而使用\begin{equation}和\end{equation}可使该公式在右侧自动生成编号

## 4. 参考文献
参考文献部分总体体验还算是比较友好的，相比于之前用EndNote错误百出，LaTeX对文献信息的识别还是十分精准的，没有发现错误。
方法也十分简单：
1. 首先创建一个空白的参考文献库mybibfile.bib(文件名可自己定)
2. 在[Google Scholar](http://scholar.google.com/)上搜到你需要的文献，点引用（下面第二个引号的图标），再点击弹出来的对话框里底下第一个BibTeX的连接，就会出来参考文献的详细的信息，全部拷贝到mybibfile.bib里，建议自己在每条文献信息前自己手动加入编号注解，比如：
```
% 1
@article{manual2007version,
  title={Version 971},
  author={Manual, LS-DYNA Keyword User’s and Volume, I},
  journal={Livermore Software Technology Corporation},
  volume={7374},
  pages={354},
  year={2007}
}
```

3. 引用的时候在正文中加入
```
\cite{manual2007version}
```
就能生成引用：

（效果图）
![image](1.jpeg)

4. 具体的引用格式在.bst模板中定义并能够在\bibliographystyle{}中选择不同的模板

## 5. 表格

说完了最基本的，对于复杂的表格，首先推荐两个辅助工具，在线的[tablesgenerator](https://www.tablesgenerator.com/latex_tables) 和Excel插件
### 5.1 基本


```
\textbf{Table xx.}~~TableCaption\\
```


详见[Latex表格样式参数命令](https://blog.csdn.net/niekai01/article/details/54618916?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.edu_weight)
### 5.2 列宽度设置
以设置为2cm为例
扩展名              | 命令      
---                 | ---            
强制设置宽度        | p{2cm}        
强制设置宽度并居中  | p{2cm} < {\centering}
注：使用<{\centering}需引入宏包\usepackage{makecell}。

还有一种更高级自定义格式的方法：

自定义命令： \newcolumntype{C}[1]{>{\centering\arraybackslash}p{#1}}

这样输入C{2cm}后自动居中
详细参见：[LATEX固定表格列宽](https://blog.csdn.net/lqhbupt/article/details/47107801)

### 5.3 表格整体宽度调整
目标: 调整表格宽度, 效果为”按页面宽度调整表格”.
命令: \setlength{\tabcolsep}{7mm}{XXXX}
该方法可配合5.2节提到的控制列宽的方法，使表格整体更为美观

如表格过宽，可整体缩小表格宽度
命令: \resizebox{\textwidth}{15mm}{XXXX}
但并不是很推荐，仅适用于略微过宽的表格，否则表格内的字会被缩的很小。用5.2节提到的控制列宽的方法更为合适

详细参见：[Latex 表格过大(或过小)的调整方法](https://blog.csdn.net/wbl90/article/details/52597429)

还有一种扩展表格宽度的方法：

\begin{tabular*}{\hsize}{@{}@{\extracolsep{\fill}}lll@{}}

详细参见： [latex 让表格宽度与文本宽度相同](https://blog.csdn.net/robert_chen1988/article/details/79505794)

关于{tabular}和{tabular*}的区别可参见：

[Latex设置表格的宽度](https://blog.csdn.net/LaineGates/article/details/54928898?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight)


### 5.4 合并单元格
可使用\multirow和\multicolumn
其中要使用\multirow，必须在前头先行加入\usepackage{multirow}

两篇非常好的参考文章：

[latex中不规则表格合并multirow的使用](http://blog.sina.com.cn/s/blog_5e16f1770100u40t.html)

[Latex 表格 多行多列](https://blog.csdn.net/happygogf/article/details/50963275)

# 附录
## 附1：我画表格用的包

```
% packages for table
\usepackage{graphicx}
\usepackage{multirow}
\usepackage{booktabs}
\usepackage{bigstrut}
\usepackage{hyperref}
\usepackage[table]{xcolor}
\usepackage{makecell}
```

## 附2：我的表格
[LaTeX表格](https://cn.overleaf.com/project/5f942dda9b6dd00001766734)
