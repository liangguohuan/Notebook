## [Tabular](https://github.com/godlygeek/tabular)

### 1. 常用简单处理

`:Tabularize /[symbol]`

文本原型:

    Some short phrase,some other phrase
    A much longer phrase here,and another long phrase

`:Tabularize /,` 命令处理后

    Some short phrase         , some other phrase
    A much longer phrase here , and another long phrase

<!-- more -->

### 2. 格式化处理

`:Tabularize /[symbol]/[lcr][num]`

- l 文本向左对齐
- c 文本居中
- r 文本向右对齐
  num 表示[symbol]两边填充多少个空格

> lcr 不能作用于 symbol 分割的第一个字段

#### 2.1 实例1:

    Some short phrase,some other phrase
    A much longer phrase here,and another long phrase

`:Tabularize /,/l5` 命令处理后

    Some short phrase             ,     some other phrase
    A much longer phrase here     ,     and another long phrase

#### 2.2 实例2:

    WORD          | COUNT | SUM      
    Edinburgh     | 9     | 2092235  
    London        | 12    | 3339260  
    New York      | 11    | 3588090  
    San Francisco | 14    | 3293475  
    Sidney        | 2     | 181000   
    Singapore     | 4     | 913725   
    Tokyo         | 5     | 963925   
    TOTAL         | 57    | 14371710 

`:Tabularize /|/l2r2r2` 命令处理后

    WORD           |  COUNT  |       SUM
    Edinburgh      |      9  |   2092235
    London         |     12  |   3339260
    New York       |     11  |   3588090
    San Francisco  |     14  |   3293475
    Sidney         |      2  |    181000
    Singapore      |      4  |    913725
    Tokyo          |      5  |    963925
    TOTAL          |     57  |  14371710

### 3. 正则匹配

`:Tabularize /[regex]\zs[symbol]/[lcr][num]`

- `regex` symbol 前边字符串正则匹配
- `\zs` put the start of the regex here (everything from this point is matched)
- `symbol` 如果为空，则默认为空格

#### 3.1 实例1：

    abc,def,ghi
        a,b
        a,b,c

`:Tabularize /^[^,]*\zs,/r0c0l0` 命令处理后

    abc,def,ghi
      a,b
      a,b,c

#### 3.2 实例2：

    one @abc @rstuvw &foo  
    three @defg &bar 
    four @mn @opq &kludge &hack  
    twelve @hijkl &baz &quux

`:Tabularize/@[^&]*\|&.*/l1l0` 命令处理后

    one    @abc @rstuvw  &foo
    three  @defg         &bar
    four   @mn @opq      &kludge &hack
    twelve @hijkl        &baz &quux

#### 3.3 实例3：

    alpha one oclock hey
    beta two oclock hey
    phi three oclock hey

`:Tabularize /^[a-z]\+\s\+[a-z]\+\zs/l2c5l3` 命令处理后

    alpha one       oclock hey
    beta two        oclock hey
    phi three       oclock hey

#### 3.4 实例4：

    alpha one oclock hey
    beta two oclock hey
    phi three oclock hey

`:Tabularize /^.*ock\zs/l2c5l3` 命令处理后

    alpha one oclock       hey
    beta two oclock        hey
    phi three oclock       hey

#### 3.5 实例5：

    alpha one oclock hey
    beta two oclock hey
    phi three oclock hey

`:Tabularize /^\(\S\+\s\+\)\{2}\S\+\zs/l2c5l3` 命令处理后

    alpha one oclock       hey
    beta two oclock        hey
    phi three oclock       hey

## [NERDTree](https://github.com/scrooloose/nerdtree)

    'o'  #打开文件或者目录
    'go' #打开文件或者目录，但光标并不跳转到打开的 Buffer 中去
    'x'  #关闭选中节点的父目录
    'p'  #跳转光标到父目录
    'P'  #跳转光标到根目录
    'K'  #跳转到当前目录的第一个节点处
    'J'  #跳转到当前目录的最后一个节点处
    'C'  #将选中的目录（如果选中文件时则为此文件的父目录）设置为根节点
    'r'  #刷新选中的目录
    'R'  #递归刷新根目录
    'cd' #将选中目录设置为当前目录
    'q'  #关闭 NERD Tree


    # NERD tree (4.2.0) quickhelp~
    # ============================
### File node mappings~
    double-click,
    <CR>,
    'o'  #open in prev window
    'go' #preview
    't'  #open in new tab
    'T'  #open in new tab silently
    middle-click,
    'i'  #open split
    'gi' #preview split
    's'  #open vsplit
    'gs' #preview vsplit

### Directory node mappings~
    double-click,
    'o'  #open & close node
    'O'  #recursively open node
    'x'  #close parent of node
    'X'  #close all child nodes of current node recursively
    middle-click,
    'e'  #explore selected dir

### Bookmark table mappings~
    double-click,
    'o'  #open bookmark
    't'  #open in new tab
    'T'  #open in new tab silently
    'D'  #delete bookmark
### Tree navigation mappings~
    'P'  #go to root
    'p'  #go to parent
    'K'  #go to first child
    'J'  #go to last child
    '<C-j>' #go to next sibling
    '<C-k>' #go to prev sibling
### Filesystem mappings~
    'C'  #change tree root to the selected dir
    'u'  #move tree root up a dir
    'U'  #move tree root up a dir but leave old root open
    'r'  #refresh cursor dir
    'R'  #refresh current root
    'm'  #Show menu
    'cd' #change the CWD to the somelected dir
    'CD' #change tree root to CWD
### Tree filtering mappings~
    'I'  #hidden files (off)
    'f'  #file filters (on)
    'F'  #files (on)
    'B'  #bookmarks (off)
### Other mappings~
    'q'  #Close the NERDTree window
    'A'  #Zoom (maximize-minimize) the NERDTree window
    '?'  #toggle help
### BookmarkToRootokmark commands~
    :Bookmark [<name>]
    :BookmarkToRoot <name>
    :RevealBookmark <name>
    :OpenBookmark <name>
    :ClearBookmarks [<names>]
    :ClearAllBookmarks

## [vim-bookmark](https://github.com/MattesGroeger/vim-bookmarks)

    'mm' #设定或者删除一个书签
    'mi' #书签自定义名称
    'mn' #跳转到下一个书签
    'mp' #跳转到前一个书签
    'ma' #显示所有书签
    'mc' #清除当前buffer中的书签
    'mx' #清除所有buffer中的书签
    # 书签组功能：https://github.com/name5566/vim-bookmark

## [surround](https://github.com/tpope/vim-surround)
### 1. 快捷键
#### 1.1 普通模式下

- `ds`  删除包围符号命令
- `cs`  替换包围符号命令
- `ys`  单词加上包围符号命令

#### 1.2 可视模式下

`S`   单词加上包围符号命令

### 2. 实例
光标在

    "Hello world!"

中时按下 `cs"'`，则会替换双引号为单引号：

    'Hello world!'

继续按下 `cs'<q>`，则会替换单引号为标签

    <q>Hello world!</q>

按下 `cst"`，则回到初始的双引号：

    "Hello world!"

要删除符号，则按下 `ds"`

    Hello world!

当光标在hello上时，按下 `ysiw]`，则会变为

    [Hello] world!

这个操作为其加上了包围符号。

**再说一个小技巧：**在包围符号为括时，输入左括号(或者{,则会留一个空格

    Hello w*orld!             ysiw(       Hello ( world )!

