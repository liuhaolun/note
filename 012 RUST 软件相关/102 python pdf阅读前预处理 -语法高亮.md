## PDF预处理--语法高亮
pip安装library: pymupdf用于pdf操作，spacy用于NLP语义分析（&而不要靠查词典）`pip install pymupdf spacy`
- spacy：使用模型 [en_core_web_sm](https://spacy.io/models/en/#en_core_web_sm) `python -m spacy download en_core_web_sm
在.py文件引入已安装library `import spacy` `import fitz` &fitz是pymupdf的附属，允许通过command line调用功能。

**（废弃）换用spacy-layout**
下载了无数依赖项。**我尝试使用过界的工具。**

1、加载模型 `nlp = spacy.load("en_core_web_sm")`
2、函数：加载.pdf---->提取词汇---->分辨为{名词||动词||形容词||连词}---->上色
	2-1、用 `enumerate(doc)` 拆页面，用 `page.get_text("words")` 的pymupdf功能获取词汇。
	2-2、重新连接被换行截断的单词。`"".join([w[4] for w in word_list])`
	2-3、`spacy_doc=nlp(text_content)` 分辨为{四种词性}。这是一个映射，我们需要逐个词代入。
	2-4、判别词性 `pos=current_token.pos_` 并根据结果 `pos in ["VERB","CONJ","PROPN","VERB"]` 分别对设置属性为预设值 `color=COLOR_NOUN`
	2-5、上色 `page.add_hightlight_annot(rect)`    ??rect是什么
	2-6、进入下一个token的处理。`if len(pdf_word_token)>=len(current_token.text)`    ??pdf_word_token和current_token等，用人类语言描述，应该是字母的什么样子？是否有空格？
3、`__main__` 函数调用。参数有‘input_path’和‘output_path’

#### 挑拣 “抄写疏漏”
1、错误：cannot save with zero pages |> fitz.open() 遗漏了input_path
2、启动后无动静 |> `_main_` 应为双下划线
3、‘words’和‘word_list’（我偏向无复数后缀的后者）混用，产生未定义变量名
4、curent_token错误拼写
5、“错误：”，然而没有返回e的结果。发现这一行无法运行：`word_list=page.get_text("word_list")` |> 修复错误3时，未经思考将参数 `"words"` 修改了。
6、产出极其错误的syntax highlight，近乎全部判断为NOUN |> `" ".join([w[4] for w in word_list])` 此前未添加空格，导致词语粘连。
7、没有CONJunction被高亮 |> 显然是‘CONJ’不够完全，经查询 [pos tagging](https://spacy.io/usage/linguistic-features/#pos-tagging)，‘ADP对应连词’而‘CONJ是AI幻觉’。

#### 数据结构
pdf_word_text=word_info[4]
	|-for word_info in word_list
		|-word_list = page.get_text("words")
- word_list: `[...,(433.02227783203125, 503.0999755859375, 440.7721862792969, 513.0625610351562, 'to', 31, 5, 6),...]`
- word_info: `(433.02227783203125, 503.0999755859375, 440.7721862792969, 513.0625610351562, 'to', 31, 5, 6)`
- pdf_word_text: 第5位置 `'to'`

rect=fitz.Rect(word_info[:4]) 取得第0~3位置，封装为Rect
	->annot=page.add_highlight_annot(rect) 将Rect格式传递为涂色区域Rect

current_token = next(spacy_iter)
	|-spacy_iter = iter(spacy_doc)
		|-spacy_doc = nlp(text_content)
			|-text_content = `" ".join([w[4] for w in word_list])`
- text_content: 取word_list的每一组(=word_info)的第5位置(=pdf_word_text)，以" "为分组标识，组合为列表
- spacy_doc: `As with the previous cir- cuit, the closed-loop gain is GCL=1 + Rf/Rg=50, with a small loop compensation capacitor Cc chosen for best re- sponse without peaking.` 打印->纯文本
	- 背地里：`nlp()` 将文本处理为 [12种属性](https://spacy.io/api/doc/#init) `text: details, pos_: NOUN, lemma_: detail, ...`
- spacy_iter: `<_cython_3_2_1.generator object at 0x000001C9B99AA210>` python的iter结构
- current_token: `'Art'

##### 处理“- ”连词换行导致词义识别错误
将spacy_doc中的断token拼合；保持word_info中的token断裂。正确识别词性，分2块上色。
1、将断token拼合会导致长度不匹配 |> 将位置(n,n+1)=(cir- ,cuit)改写为(circuit, circuit)
2、由于pdf_word_text只用于：对比和current_token是否不同，不同则跳过该token。因此知道：current_token=>词性，而Rect(word_info[:4])=>绘制位置。所以：重复词!=错误绘制。
3、word_list是交汇点，直接改写word_list

```
for page_num,page in enumerate(doc):
    word_list=page.get_text("words")
    for i,w in word_list:
        if next_word_already_copied:
            next_word_already_copied=False
            continue
        if w[4].endswith('-') and i+1<len(raw_words):
            w_next=word_list[i+1]
            copied_text=w[4][:-1]+w_next[4]
            word_list[i][4]=copied_text
            word_list[i+1][4]=copied_text
            next_word_already_copied=True
        else:
            continue
```
然而，在复制为copied_text后，仍存在词性判断相异（由于使用NLP而考虑了前后语境）（inverting inverting Verb Noun）。重复词将坏影响spacy的分析结果。
	懒得更加投入精力，用AI改了。
###### 挑拣 “抄写疏漏”
1、for i,w in word_list => 错误：too many values to unpack (expected 2) |> 需要使用enumerate(word_list)
2、错误：cannot access local variable 'next_word_already_copied' where it is not associated with a value |> 进入循环前，初始化next_word_already_copied = False
3、错误：name 'raw_words' is not defined |> 抄写时沿用了改名前的变量
4、错误：'tuple' object does not support item assignment |> 'tuple'是无法修改的。`page.get_text("words")` 返回的word_list是一个列表（用"[]"包裹），而每个word_info是元组（用"()"包裹），（都以","分隔）。 |> 新建clean_word_list，将交汇点改写：word_list->clean_word_list

![[plus/PDF预处理效果 -语法高亮.png]]



## Appendix
**pos_**
PROPN (专有名词: Q5, mA)
VERB (动词: turns, rises)
ADP (介词/方位词: on, to, from, out, across)
PUNCT (标点)
DET (限定词: the, another)
ADJ (形容词: total, emitter, base)
NOUN (名词: current, collector, resistor, drop)
NUM (数字: 5, 200)
PART (助词: ’s)
CCONJ (并列连词: and, flow)
PRON (代词: its, both, which)
AUX (助动词/系动词: is)
ADV (副词: now)

**dep_**
核心动作相关： nsubj (主语), ROOT (谓语核心), conj (连带动作), prt (动词颗粒/小品词), attr (表语)
修饰与限定： amod (形容词修饰), compound (复合修饰), det (限定), nummod (数量修饰), poss (所有格), case (格标记)
补充说明： dobj (直接宾语), prep (介词引导), pobj (介词宾语), appos (同位语), advmod (状语修饰)
连接与符号： cc (并列连接), punct (标点)

**压缩**
原文件96kb
使用doc.save() 无参数265kb；有参数228kb
使用doc.ez_save()162kb
使用字典 `highlights_map[color].append(rect)` 在key=color存放多个rect坐标。再 `quad_list = [r.quad for r in rect]` 合并为单一quad。`annot = page.add_highlight_annot(quads=quad_list)` 每页、每种颜色绘制一个高亮。94kb