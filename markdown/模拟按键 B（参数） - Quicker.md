> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [getquicker.net](https://getquicker.net/kc/help/doc/sendkeys)

> Quicker，Windows 效率工具。快速触发 + 自动化。

发送指定的按键序列到目标窗口。

*   本功能在内部使用了 C# 的 [System.Windows.Forms.SendKeys.SendWait()](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.sendkeys?view=netframework-4.8) 函数。因此，参数格式可以直接参考该文档。
*   **此操作可能会受到输入法的影响，请在使用前将输入法切换到英文状态。**[不受输入法影响的方式，请参考本文。](https://getquicker.net/KC/Kb/Article/1045)

此模块和 “[模拟按键 A（录入）](https://getquicker.net/kc/help/doc/keyinput)” 模块的区别为：

*   “模拟按键（录入）” 模块使用直接录入的方式指定要发送的内容，只能发送固定的内容。
*   本模块使用文本参数的形式传入要发送的内容，可以接受参数或使用插值，可以和其他模块协作动态变更发送的按键序列内容。

![](https://files.getquicker.net/_sitefiles/kc/yuque/0/2019/png/272392/1561596023388-eb4e3af8-330d-497a-a84a-d3afce89a831.png)

按键序列参数格式
--------

### 要点

*   `^`代表 Ctrl 键
*   `+`代码 Shift 键
*   `%`代表 Alt 键
*   其它普通字母和数字键使用小写形式。 如`^c`表示 Ctrl+C（复制）。特殊键使用`{键名}`的格式，参见下表。
*   不支持 Win 键和一些特殊按键，如 F17-F24、媒体键等。

### 详解

（1）普通字符使用字符本身表示，如 “a” 表示发送字符 “a”，“abc” 表示发送 “abc” 三个字符。

（2）加号 (+)、插入符 (^)、百分比符号 (%)、上划线 (~) 及圆括号 ( ) 都具有特殊意义。为了指定上述任何一个字符，要将它放在大括号 ({}) 当中。例如，要指定正号，可用 {+} 表示。方括号 ([ ]) 并不具有特殊意义，但必须将它们放在大括号中。为了指定大括号字符，请使用 "{{}" 和 "{}}"。

（3）为了在按下按键时指定那些不显示的字符，例如 ENTER 或 TAB 以及那些表示动作而非字符的按键，请使用下列代码：

<table><tbody><tr><td><p><strong>按键</strong></p></td><td><p><strong>代码</strong></p></td></tr><tr><td><p>WIN</p></td><td><p>底层 API<strong> 不支持</strong></p></td></tr><tr><td><p>BACKSPACE</p></td><td><p>{BACKSPACE}, {BS}, or {BKSP}</p></td></tr><tr><td><p>BREAK</p></td><td><p>{BREAK}</p></td></tr><tr><td><p>CAPS LOCK</p></td><td><p>{CAPSLOCK}</p></td></tr><tr><td><p>DEL or DELETE</p></td><td><p>{DELETE} or {DEL}</p></td></tr><tr><td><p>DOWN ARROW</p></td><td><p>{DOWN}</p></td></tr><tr><td><p>END</p></td><td><p>{END}</p></td></tr><tr><td><p><strong>ENTER 回车</strong></p></td><td><p>{ENTER} or ~</p></td></tr><tr><td><p>ESC</p></td><td><p>{ESC}</p></td></tr><tr><td><p>HELP</p></td><td><p>{HELP}</p></td></tr><tr><td><p>HOME</p></td><td><p>{HOME}</p></td></tr><tr><td><p>INS or INSERT</p></td><td><p>{INSERT} or {INS}</p></td></tr><tr><td><p>LEFT ARROW</p></td><td><p>{LEFT}</p></td></tr><tr><td><p>NUM LOCK</p></td><td><p>{NUMLOCK}</p></td></tr><tr><td><p>PAGE DOWN</p></td><td><p>{PGDN}</p></td></tr><tr><td><p>PAGE UP</p></td><td><p>{PGUP}</p></td></tr><tr><td><p>PRINT SCREEN</p></td><td><p>{PRTSC} (reserved for future use)</p></td></tr><tr><td><p>RIGHT ARROW</p></td><td><p>{RIGHT}</p></td></tr><tr><td><p>SCROLL LOCK</p></td><td><p>{SCROLLLOCK}</p></td></tr><tr><td><p>TAB</p></td><td><p>{TAB}</p></td></tr><tr><td><p>UP ARROW</p></td><td><p>{UP}</p></td></tr><tr><td><p>F1</p></td><td><p>{F1}</p></td></tr><tr><td><p>F2</p></td><td><p>{F2}</p></td></tr><tr><td><p>F3</p></td><td><p>{F3}</p></td></tr><tr><td><p>F4</p></td><td><p>{F4}</p></td></tr><tr><td><p>F5</p></td><td><p>{F5}</p></td></tr><tr><td><p>F6</p></td><td><p>{F6}</p></td></tr><tr><td><p>F7</p></td><td><p>{F7}</p></td></tr><tr><td><p>F8</p></td><td><p>{F8}</p></td></tr><tr><td><p>F9</p></td><td><p>{F9}</p></td></tr><tr><td><p>F10</p></td><td><p>{F10}</p></td></tr><tr><td><p>F11</p></td><td><p>{F11}</p></td></tr><tr><td><p>F12</p></td><td><p>{F12}</p></td></tr><tr><td><p>F13</p></td><td><p>{F13}</p></td></tr><tr><td><p>F14</p></td><td><p>{F14}</p></td></tr><tr><td><p>F15</p></td><td><p>{F15}</p></td></tr><tr><td><p>F16</p></td><td><p>{F16}</p></td></tr><tr><td><p>Keypad add</p></td><td><p>{ADD}</p></td></tr><tr><td><p>Keypad subtract</p></td><td><p>{SUBTRACT}</p></td></tr><tr><td><p>Keypad multiply</p></td><td><p>{MULTIPLY}</p></td></tr><tr><td><p>Keypad divide</p></td><td><p>{DIVIDE}</p></td></tr></tbody></table>

（4）为了表示在按下某个按键时同时要按下的 SHIFT、CTRL 和 ALT 控制键，可以在按键字符前插入下面的代码：

如 Ctrl+C 可以表示为 “^c”。

如果在按下 SHIFT、CTRL、ALT 组合的同时需要按下多个其他按键，则需要将他们包含在括号中。如：要表示按下 SHIFT 的同时依次按下 e 和 c，可以用 “+(ec)” 表示。

（5）如果要设定按键的重复次数，使用 {按键 次数} 的格式。按键和次数之间放置一个空格。如：{LEFT 42}表示按下方向键←42 次，{h 10}表示按下 H 键 10 次。

请注意，字符的大小写可能会影响执行的结果。如 ^s 和 ^S 可能会产生不同的结果。请多测试以确保目标软件按预期执行操作。

### 按键组合示例

<table><tbody><tr><td><p><strong>代码</strong></p></td><td><p><strong>按键序列</strong></p></td></tr><tr><td><p>^p</p></td><td><p>Ctrl+p&nbsp;组合键</p></td></tr><tr><td><p>+p</p></td><td><p>Shift+p&nbsp;组合键</p></td></tr><tr><td><p>%p</p></td><td><p>Alt+p 组合键</p></td></tr><tr><td><p>^+s</p></td><td><p>Ctrl+Shift+s&nbsp;组合键</p></td></tr><tr><td><p>^(kc)</p></td><td><p>按 Ctrl 同时按 K 和 C</p></td></tr><tr><td><p>^kc</p></td><td><p>先按 Ctrl+k 组合键，全部松开后再按 c 键</p></td></tr><tr><td><p>Hello~New Line</p></td><td><p>Hello(回车)</p><p>New Line</p></td></tr><tr><td><p>中文字符</p></td><td><p>中文字符</p></td></tr><tr><td><p>{LEFT 10}</p></td><td><p>按←键 10 次</p></td></tr><tr><td><p>{h 10}</p></td><td><p>按 h 键 10 次</p></td></tr></tbody></table>

*   选择一个快捷键组合发送：[https://getquicker.net/Sharedaction?code=67129c30-9d18-40c7-0ab8-08d714376b4c](https://getquicker.net/Sharedaction?code=67129c30-9d18-40c7-0ab8-08d714376b4c)