[Tabular](https://github.com/godlygeek/tabular)
-----------------------------------------------

### 1. 常用简单处理

`:Tabularize /[symbol]`

文本原型:

    Some short phrase,some other phrase
    A much longer phrase here,and another long phrase

`:Tabularize /,` 命令处理后

    Some short phrase         , some other phrase
    A much longer phrase here , and another long phrase

### 2. 格式化处理

`:Tabularize /[symbol]/[lcr][num]`

-   l 文本向左对齐
-   c 文本居中
-   r 文本向右对齐 num 表示[symbol]两边填充多少个空格

> lcr 不能作用于 symbol 分割的第一个字段

#### 2.1 实例1:

    Some short phrase,some other phrase
    A much longer phrase here,and another long phrase

`:Tabularize /,/l5` 命令处理后

    Some short phrase             ,     some other phrase
    A much longer phrase here     ,     and another long phrase

#### 2.2 实例2:

    WORD          | COUNT | SUM      
    Edinburgh     | 
