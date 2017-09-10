## 移动光标

    `Ctrl-a`  # 移动光标到行首。
    `Ctrl-e`  # 移动光标到行尾。
    `Ctrl-f`  # 光标前移一个字符；和右箭头作用一样。
    `Ctrl-b`  # 光标后移一个字符；和左箭头作用一样。
    `Alt-f `  # 光标前移一个字。
    `Alt-b `  # 光标后移一个字。
    `Ctrl-l`  # 清空屏幕，移动光标到左上角。clear 命令完成同样的工作。

## 修改文本

    `Ctrl-d`  # 删除光标位置的字符。
    `Ctrl-t`  # 光标位置的字符和光标前面的字符互换位置。
    `Alt-t `  # 光标位置的字和其前面的字互换位置。
    `Alt-l `  # 把从光标位置到字尾的字符转换成小写字母。
    `Alt-u `  # 把从光标位置到字尾的字符转换成大写字母。
    `Ctrl-u`  # 删除整行。
    `Ctrl-w`  # 删除光标左边单词。

## 剪切和粘贴文本

    `Ctrl-k`         # 剪切从光标位置到行尾的文本。
    `Ctrl-u`         # 剪切从光标位置到行首的文本。
    `Alt-d`          # 剪切从光标位置到词尾的文本。
    `Alt-Backspace`  # 剪切从光标位置到词头的文本。如果光标在一个单词的开头，剪切前一个单词。
    `Ctrl-y`         # 把剪切环中的文本粘贴到光标位置。

## 系统命令集合

```
`!!`                     # 重复上次命令
`ssh user@host`          # 以 user 用户身份连接到 host
`ssh -p port user@host`  # 在端口 port 以 user 用户身份连接到 host
`ssh-copy-id user@host`  # 将密钥添加到 host 以实现无密码登录
`bg`                     # 列出已停止或后台的作业
`fg`                     # 将最近的作业带到前台
`fg n`                   # 将作业 n 带到前台
```

## 有用命令集合

**> 将匹配的数据合并为一行**

    panamax -h | grep ": " | awk -F ':' '{print $1}' | tr "\n" " " | xargs echo

**> find 命令相关 
查找当前目录24小时内改变过的文件，但是排除文件名为 ghost 的文件，然后把这些找到的文件移动到 ghost 目录下**

    find -maxdepth 1 -ctime 0 ! -name "." ! -name "ghost" -exec mv -f {} ./ghost/ \;

查找文件并且统计大小

    find /media/d/libs -type f | xargs du -sch

查找文件并且转换编码

    find /media/d/libs -type f | xargs enconv -x UTF-8
    # sed  -e "s/\(\s\+\)/\\\\\1/g"  空格路径SHELL中正确化
    # sed  -e "s/ /\\\ /g" | less 单个空格简单处理路径SHELL中正确化



**> 显示目录**

    ls -ld */

**> rename**

    rename 's/^/[单品] /' *
    rename 's/^([^\[])/[单品] $1/' *

**> 将vagrant 命令找出来**

    vagrant -h | egrep '^\s{2,}[a-z]' | awk '{print $1}' | tr '\n' ' '
    vagrant -h | awk '/^ +[a-z]/{print $1}' | tr "\n" "  "

## Linux 报警
**> `password`设置简单密码**

    Warnning：You must choose a longer password

修改文件　/etc/pam.d/common-password

    password    [success=1 default=ignore]  pam_unix.so minlen=1 sha512