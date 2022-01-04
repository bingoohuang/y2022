# BrainFuck

Brainfuck 是一种极小化的计算机语言，它是由 Urban Müller 在 1993 年创建的。由于 fuck 在英语中是脏话，这种语言有时被称为 `brainf\*ck` 或 `brainf\*\*k` ，甚至被简称为 BF。

**它只有 8 种关键字！**

Brainfuck 语言可以和图灵机互相模拟。

## 相关链接

- [BF 的官网](http://www.muppetlabs.com/~breadbox/bf/)
- [BF 的 Wikipedia](http://en.wikipedia.org/wiki/Brainfuck)

## brainfuck 语言入门

https://www.leavesongs.com/OTHERLAN/Beginbrainfuck.html

将八个操作符 < > + - [ ] . , 分别转写为：

1. `>` 右
1. `<` 左
1. `+` 上
1. `-` 下
1. `[` 始
1. `]` 终
1. `.` 写
1. `,` 读

然后找一张方格纸，对就是小学作文本那种，一支铅笔（不，钢笔不行，Lamy 的也不行），一块橡皮，一张从你的旧 C 语言书上撕下来的 ASCII 码表。

现在来看 +++[>+<-] 这行程序。按照我们的转写，它变成：上上上始右上左下终。

接下来在你的方格纸上的第一个格子里写下 0 。

`[0]`

很好。现在我们一边读转写好的程序，一边按照以下规则行事：

    - 遇到什么都没写的格子就当里面写了 0
    - 右：向右移动一个格子。嗯你盯着它看就行了，什么都不用做
    - 左：向左移动一个格子
    - 上：给格子里的数字加上 1，擦掉原来的数字再写回去。现在你知道为什么要用铅笔了吧，少年！
    - 下：给格子里的数字减去 1
    - 始：开始重复「始……终」之间的指令，直到你读到「始」之前盯着的那个格子里的数字变成 0 为止。（什么？那个格子里已经是负数了？……不要这么没有下限好不好）
    - 终：如果当前格子里的数字为 0，就跳过，否则回头到「始」那里
    - 写：查当前格子里的数字在 ASCII 表上对应的字母，把它写下来（不，别写在格子里，就写在你买来一直立志想用但是没有用的日记本上吧）
    - 读：随便想一个英文字母，查表找到它对应的数字，写到当前格子里

我们开始吧。方括号代表当前你盯着看的格子。首先是上，上，上：
［3］
接下来，始：
［3］
嗯，你要重复做它到「终」之间的事情。首先是「右」：
```sh
3［0］
上：
3［1］
左：
［3］1
下：
［2］1
终：
碰到「始」之前你盯着的那个格子，你还记得吗？碰巧就是当前这个。目前里面是 2 ，不是 0，所以我们回去「始」。
右：
2［1］
上：
2［2］
左：
［2］2
下：
［1］2
终：
回去「始」
右：
1［2］
上：
1［3］
左：
［1］3
下：
［0］3
终：
```

格子里是 0 了，你可以跳过「终」读下去了。 这段程序就此完结，但如果你读懂了这些，其实就已经看懂了我那个程序最开始的部分：

`++++++++++[>+++>++++>+++++++>++++++++++>+++++++++++<<<<<-]`

无非就是，先把最左边的格子加到 10，然后向右移动，移动到第二个格子里加 3，第三个加 4，第四个加 7，第五个加 10，第六个加 11，然后向左移动五次，回到第一格，减 1。如此重复十次之后，最左边的格子变成 0，循环终止，而我有了如下格子：

［0］ 30 40 70 100 110

接下来的部分，就是挪到某个格子，加上或者减去若干次 1，直到获得想要字母的 ASCII 代码，然后用 . 把它打印出来。比如紧接着的 >>>++. 就是右移三次，到写了 70 的格子上，加两次 1，得到 ASCII 72 ，大写字母 H，再用 . 把它显示出来。

很简单吧？学会了？恭喜你……

> It is practically impossible to teach functional programming to students that have had a prior exposure to brainfuck: as potential programmers they are mentally mutilated beyond hope of regeneration.
> –– Edsger W. Dijkstra (paraphrased)

## BrainFuck 解释器（C 语言实现）

https://blog.csdn.net/redraiment/article/details/7483062

码农的业余休闲活动就是去学习一门冷门的语言或者研究一项非主流的技术。BrainFuck 是一门小巧的编程语言，顾名思义，阅读这门语言的代码就像在强奸你的大脑一样。事实证明开发它的解释器比读懂它的 Hello World 要快。

BrainFuck 只有八条指令：

| 指令 | 含义                                                          | 等价的 C 代码      |
| ---- | ------------------------------------------------------------- | ------------------ |
| \>   | 指针加一                                                      | ++ptr;             |
| \<   | 指针减一                                                      | --ptr;             |
| \+   | 指针指向的字节的值加一                                        | ++\*ptr;           |
| \-   | 指针指向的字节的值减一                                        | --\*ptr;           |
| \.   | 输出指针指向的单元内容（ASCII 码）                            | putchar(\*ptr);    |
| \,   | 输入内容到指针指向的单元（ASCII 码）                          | \*ptr = getchar(); |
| \[   | 如果指针指向的单元值为零，向后跳转到对应的]指令的次一指令处   | while (\*ptr) {    |
| \]   | 如果指针指向的单元值不为零，向前跳转到对应的[指令的次一指令处 | }                  |

## 示例

```sh
🕙[2022-01-04 23:52:24.059] ❯ cc brainfuck.c

y2022/BrainFuck on  main [!?] via C base
🕙[2022-01-04 23:52:28.282] ❯ ls
README.md   a.out       brainfuck.c

y2022/BrainFuck on  main [!?] via C base
🕙[2022-01-04 23:52:30.000] ❯ ./a.out helloword.bf
Hello World!
```

[helloword.bf](helloword.bf)解释

```
+++ +++ +++ +           initialize counter (cell #0) to 10
[                       use loop to set the next four cells to 70/100/30/10
    > +++ +++ +             add  7 to cell #1
    > +++ +++ +++ +         add 10 to cell #2
    > +++                   add  3 to cell #3
    > +                     add  1 to cell #4
    <<< < -                 decrement counter (cell #0)
]
>++ .                   print 'H' ascii 72
>+.                     print 'e' ascii 101
+++ +++ +.              print 'l'
.                       print 'l'
+++ .                   print 'o'
>++ .                   print ' '
<<+ +++ +++ +++ +++ ++. print 'W'
>.                      print 'o'
+++ .                   print 'r'
--- --- .               print 'l'
--- --- --.             print 'd'
>+.                     print '!'
>.                      print '\n'`
```

[Ascii Table](https://www.rapidtables.com/code/text/ascii-table.html)

```sh
Dec	Hex	Binary	HTML	Char	Description
0	00	00000000	&#0;	NUL	Null
1	01	00000001	&#1;	SOH	Start of Header
2	02	00000010	&#2;	STX	Start of Text
3	03	00000011	&#3;	ETX	End of Text
4	04	00000100	&#4;	EOT	End of Transmission
5	05	00000101	&#5;	ENQ	Enquiry
6	06	00000110	&#6;	ACK	Acknowledge
7	07	00000111	&#7;	BEL	Bell
8	08	00001000	&#8;	BS	Backspace
9	09	00001001	&#9;	HT	Horizontal Tab
10	0A	00001010	&#10;	LF	Line Feed
11	0B	00001011	&#11;	VT	Vertical Tab
12	0C	00001100	&#12;	FF	Form Feed
13	0D	00001101	&#13;	CR	Carriage Return
14	0E	00001110	&#14;	SO	Shift Out
15	0F	00001111	&#15;	SI	Shift In
16	10	00010000	&#16;	DLE	Data Link Escape
17	11	00010001	&#17;	DC1	Device Control 1
18	12	00010010	&#18;	DC2	Device Control 2
19	13	00010011	&#19;	DC3	Device Control 3
20	14	00010100	&#20;	DC4	Device Control 4
21	15	00010101	&#21;	NAK	Negative Acknowledge
22	16	00010110	&#22;	SYN	Synchronize
23	17	00010111	&#23;	ETB	End of Transmission Block
24	18	00011000	&#24;	CAN	Cancel
25	19	00011001	&#25;	EM	End of Medium
26	1A	00011010	&#26;	SUB	Substitute
27	1B	00011011	&#27;	ESC	Escape
28	1C	00011100	&#28;	FS	File Separator
29	1D	00011101	&#29;	GS	Group Separator
30	1E	00011110	&#30;	RS	Record Separator
31	1F	00011111	&#31;	US	Unit Separator
32	20	00100000	&#32;	space	Space
33	21	00100001	&#33;	!	exclamation mark
34	22	00100010	&#34;	"	double quote
35	23	00100011	&#35;	#	number
36	24	00100100	&#36;	$	dollar
37	25	00100101	&#37;	%	percent
38	26	00100110	&#38;	&	ampersand
39	27	00100111	&#39;	'	single quote
40	28	00101000	&#40;	(	left parenthesis
41	29	00101001	&#41;	)	right parenthesis
42	2A	00101010	&#42;	*	asterisk
43	2B	00101011	&#43;	+	plus
44	2C	00101100	&#44;	,	comma
45	2D	00101101	&#45;	-	minus
46	2E	00101110	&#46;	.	period
47	2F	00101111	&#47;	/	slash
48	30	00110000	&#48;	0	zero
49	31	00110001	&#49;	1	one
50	32	00110010	&#50;	2	two
51	33	00110011	&#51;	3	three
52	34	00110100	&#52;	4	four
53	35	00110101	&#53;	5	five
54	36	00110110	&#54;	6	six
55	37	00110111	&#55;	7	seven
56	38	00111000	&#56;	8	eight
57	39	00111001	&#57;	9	nine
58	3A	00111010	&#58;	:	colon
59	3B	00111011	&#59;	;	semicolon
60	3C	00111100	&#60;	<	less than
61	3D	00111101	&#61;	=	equality sign
62	3E	00111110	&#62;	>	greater than
63	3F	00111111	&#63;	?	question mark
64	40	01000000	&#64;	@	at sign
65	41	01000001	&#65;	A	 
66	42	01000010	&#66;	B	 
67	43	01000011	&#67;	C	 
68	44	01000100	&#68;	D	 
69	45	01000101	&#69;	E	 
70	46	01000110	&#70;	F	 
71	47	01000111	&#71;	G	 
72	48	01001000	&#72;	H	 
73	49	01001001	&#73;	I	 
74	4A	01001010	&#74;	J	 
75	4B	01001011	&#75;	K	 
76	4C	01001100	&#76;	L	 
77	4D	01001101	&#77;	M	 
78	4E	01001110	&#78;	N	 
79	4F	01001111	&#79;	O	 
80	50	01010000	&#80;	P	 
81	51	01010001	&#81;	Q	 
82	52	01010010	&#82;	R	 
83	53	01010011	&#83;	S	 
84	54	01010100	&#84;	T	 
85	55	01010101	&#85;	U	 
86	56	01010110	&#86;	V	 
87	57	01010111	&#87;	W	 
88	58	01011000	&#88;	X	 
89	59	01011001	&#89;	Y	 
90	5A	01011010	&#90;	Z	 
91	5B	01011011	&#91;	[	left square bracket
92	5C	01011100	&#92;	\	backslash
93	5D	01011101	&#93;	]	right square bracket
94	5E	01011110	&#94;	^	caret / circumflex
95	5F	01011111	&#95;	_	underscore
96	60	01100000	&#96;	`	grave / accent
97	61	01100001	&#97;	a	 
98	62	01100010	&#98;	b	 
99	63	01100011	&#99;	c	 
100	64	01100100	&#100;	d	 
101	65	01100101	&#101;	e	 
102	66	01100110	&#102;	f	 
103	67	01100111	&#103;	g	 
104	68	01101000	&#104;	h	 
105	69	01101001	&#105;	i	 
106	6A	01101010	&#106;	j	 
107	6B	01101011	&#107;	k	 
108	6C	01101100	&#108;	l	 
109	6D	01101101	&#109;	m	 
110	6E	01101110	&#110;	n	 
111	6F	01101111	&#111;	o	 
112	70	01110000	&#112	p	 
113	71	01110001	&#113;	q	 
114	72	01110010	&#114;	r	 
115	73	01110011	&#115;	s	 
116	74	01110100	&#116;	t	 
117	75	01110101	&#117;	u	 
118	76	01110110	&#118;	v	 
119	77	01110111	&#119;	w	 
120	78	01111000	&#120;	x	 
121	79	01111001	&#121;	y	 
122	7A	01111010	&#122;	z	 
123	7B	01111011	&#123;	{	left curly bracket
124	7C	01111100	&#124;	|	vertical bar
125	7D	01111101	&#125;	}	right curly bracket
126	7E	01111110	&#126;	~	tilde
127	7F	01111111	&#127;	DEL	delete
```