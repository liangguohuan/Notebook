> Official：http://zsh.sourceforge.net/Doc/      
> Tips：    http://www.rayninfo.co.uk/tips/zshtips.html  
> Lovers：  http://grml.org/zsh/zsh-lovers.html  
> Wiki：    http://zshwiki.org/home/

- 使用 ** 作为下级目录的通配符:

```
$ ls **/*.pyc
foo.pyc bar.pyc lib/wibble.pyc
$ rm **/*.pyc
$ ls **/*.pyc
```

- 在文件筛选中使用匹配模式:

```
$ ls *.(py|sh)
```

- 在文件筛选中使用修饰符，如：(@) 限制只匹配符号链接：

```
$ ls -l *(@)
```

- 或者在kill的时候自动完成：

```
$ kill Dock[TAB]
```

- 使用r来重复上一条命令，可以带替换方式！

```
$ touch foo.htm bar.htm
$ mv foo.htm foo.html
$ r foo=bar
mv bar.htm bar.html
```

## alias
- 文件关联执行程序
```
alias -s [ext]=[cmd]
```

`ext` 文件扩展名  
`cmd` 关联的执行程序

以下实例为将扩展名为`php`的文件关联`vim`执行程序，这样只要输入扩展名为`php`的文件名，然后回车就可用`vim`打开

```
autoload -U zsh-mime-setup
zsh-mime-setup
alias -s php=vim
```

- 全局别名：可以理解为将别名当作变量
```
alias -g [var]
```

实例：

    alias -g C='| wc -l'
    grep alias ~/.zsh/* C

