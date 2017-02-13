这篇文章主要聊一聊 HTML5 中的 `<b>` 和 `<strong>`，以及 `<i>` 和 `<em>`。

从页面显示效果来看，被 `<b>` 和 `<strong>` 包围的文字将会被加粗，而被 `<i>` 和 `<em>` 包围的文字将以斜体的形式呈现。那大家可能就会疑惑了，既然效果一样，那为什么还要重复定义标签呢？这就要从 HTML5 的一个最大的特性 -- 语义化来谈了。

不得不说，`<b>` 和 `<i>` 创建之初就是简单地表示粗体和斜体样式，但现在是 HTML5 的天下。语义化是 HTML5 最大的特性之一，而所有被 HTML5 保留的标签都带有其特有的语义，`<b>` 和 `<i>` 也不例外，它们分别被重新赋予了语义。相比较而言，标签的样式反而变得无足轻重，所以上面所讲的两组标签，虽然样式上表现极其相似，但其实语义上各有侧重。

下面就来一一介绍这 4 个标签。

## `<i>`

根据 W3C 对 [`<i>`](https://www.w3.org/TR/html5/text-level-semantics.html#the-i-element) 的定义：

> The i element represents a span of text in an alternate voice or mood, or otherwise offset from the normal prose in a manner indicating a different quality of text, such as a taxonomic designation, a technical term, an idiomatic phrase from another language, transliteration, a thought, or a ship name in Western texts.

翻译一下就是：

> i 元素代表在普通文本中具有不同语态或语气的一段文本，某种程度上表明一段不同特性的文本，比如一个分类学名称，一个技术术语，一个外语习语，一个音译，一个想法，或者西方文本中的一艘船名。

## `<b>`

根据 W3C 对 [`<b>`](https://www.w3.org/TR/html5/text-level-semantics.html#the-b-element) 的定义：

> The b element represents a span of text to which attention is being drawn for utilitarian purposes without conveying any extra importance and with no implication of an alternate voice or mood, such as key words in a document abstract, product names in a review, actionable words in interactive text-driven software, or an article lede.

翻译一下就是：

> b 元素代表侧重实用目的而不带有任何额外重要性也不暗示不同语态或语气的一段文本，比如一段文本摘要中的关键词、一段审查中的产品名称、文本驱动软件中的可执行语句或者一篇文章的导语。

## `<em>`

根据 W3C 对 [`<em>`](https://www.w3.org/TR/html5/text-level-semantics.html#the-em-element) 的定义：

> The em element represents stress emphasis of its contents.

翻译一下就是：

> em 元素代表对其内容的强调

强调位置的不同通常会带来整个句子含义的变化。看下面举的例子：

```javascript
// 这是一句不带任何强调的句子
<p>Cats are cute animals.</p>

// em 包围 Cats，强调猫是种可爱的动物，而不是狗或者其他动物
<p><em>Cats</em> are cute animals.</p>

// em 包围 are，代表句子所说是事实，来反驳那些说猫不可爱的人
<p>Cats <em>are</em> cute animals.</p>

// em 包围 cute，强调猫是一种可爱的动物，而不是有人说的刻薄、讨厌的动物
<p>Cats are <em>cute</em> animals.</p>

// 这里强调猫是动物，而不是植物
<p>Cats are cute <em>animals</em>.</p>
```

## `<strong>`

根据 W3C 对 [`<strong>`](https://www.w3.org/TR/html5/text-level-semantics.html#the-strong-element) 的定义：

> The strong element represents strong importance, seriousness, or urgency for its contents.

翻译一下就是：

> strong 元素代表内容的强烈的重要性、严重性或者紧急性。

### 重要性

`<strong>` 元素可以被用在标题（heading）、说明（caption）或者段落（paragraph）上，来显示这部分被包围的文字的重要性。

```javascript
// 章节序号不重要，章节的名字才重要
<h1>Chapter 1: <strong>The Praxis</strong></h1>
```

### 严重性

`<strong>` 元素可以被用来标记警告或者警示标志。

```javascript
<p><strong>Warning.</strong> This dungeon is dangerous.</p>
```

### 紧急性

`<strong>` 元素可以被用来表示需要被尽快看见的部分。

需要注意的是，`<strong>` 元素仅仅对文本内容的重要性、严重性或紧急性产生作用，而不像 `<em>` 对句子含义进行改变。

```javascript
<p>Welcome to Remy, the reminder system.</p>
<p>Your tasks for today:</p>
<ul>
 <li><p><strong>Turn off the oven.</strong></p></li>
 <li><p>Put out the trash.</p></li>
 <li><p>Do the laundry.</p></li>
</ul>
```
