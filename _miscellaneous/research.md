---
title: "Some tips about doing research"
collection: learning
categories:
    - miscellaneous
permalink: /learning/miscellaneous/research/
excerpt: 'Some tips and tricks about doing research.'
date: 2024-5-31
author_profile: true
toc: true
---

#
## Some GPT Prompt
1. 润色的prompt ：
    
    I am preparing my conference paper for submission of ICASSP, a top-tier conference in signal processing and audio processing, and require assistance in polishing each paragraph. Could you please refine my writing for academic rigor? I need you to correct any grammatical errors, improve sentence structure for academic suitability, and make the text more formal where necessary. For each paragraph we need to improve, you need to put all modified sentences in a Markdown table, each column contains the following: Full original sentence; Highlight the revised part of the sentence; Explain why made these changes. Finally, Rewrite the full, corrected paragraph. If you understand, please reply: yes, let's get started.

2. 检查语法的prompt：
    
    Can you help me ensure that the grammar and the spelling is correct? Do not try to polish the text, if no mistake is found, tell me that this paragraph is good. If you find grammar or spelling mistakes, please list mistakes you find in a two-column markdown table, put the original text the first column, put the corrected text in the second column and highlight the key words you fixed. Example: Paragraph: How is you? Do you knows what is it? | Original sentence | Corrected sentence | | :--- | :--- | | How is you? | How are you? | | Do you knows what is it? | Do you know what it is ? | Below is a paragraph from an academic paper. You need to report all grammar and spelling mistakes as the example before. Paragraph: ——————


# LaTex
## Tricks
1. 对于类似希腊字符的特殊字符是不允许使用`\textbf{}`的方式加粗的，编译会报错。对于特殊字符的加粗要使用`\pmb{}`的方法，`\mathbf{}`没有作用。`\mathrm{}`可以让字符不变为斜体
2. 如何引用公式， 
    \ref{label{equation:eq2}}
    
    \begin{align}
    
    \label{equation:eq2}
    \end{align}
    %
    
3. 向量的角标一般来说不要加粗向量加粗就可以了
4. 如何插入多行多列子图？https://smallsquare.github.io/LaTeX-insert-images/



 
4 (一) 句号换成分号 enumerate
式子2-1  ， 句号改成分号。 ：

2-7公式 逗号，
图横纵轴字体调大，用中文画图，