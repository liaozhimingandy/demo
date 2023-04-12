## SOAP Web Service Client配置

#### 入参示例

![Web Service Client入参示例](/docs-note-rhapsody/assets/images/image-20211117110811355.png)

???+ warning highlight blink"xml节点包含特殊字符如&时,需要提前转义;转义规则如下"
	| 特殊字符 | 代替符号  | 特殊原因                 |
	| -------- | --------- | ------------------------ |
	| `&`      | `&amp;`   | 每一个代表符号的开头字符 |
	| `>`      | `&gt; `   | 标记的结束字符           |
	| `<`      | `&lt; `   | 标记的开始字符           |
	| `"`      | `&quot;`  | 设定属性值               |
	| `''`     | `&apos; ` | 设定属性值               |

#### 出参解析示例

![Web Service Client出参示例](/docs-note-rhapsody/assets/images/image-20211117110923808.png)

