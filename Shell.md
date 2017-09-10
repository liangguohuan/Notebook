```shell
#=======================================================================
# /bin/bash /bin/sh diff in ubuntu
#=======================================================================
 一般我们写脚本默认用 /bin/bash, 支持语法更多。
 之前 ubuntu 中的 /bin/sh 是软链到 /bin/bash，
 后来 Ubuntu 6.10 将 /bin/sh 软链到 /bin/dash。dash 是鉴于 bash 过于复杂
 有人把 ash 从 NetBSD 移植到 Linux 并更名为 dash（Debian Almquist Shell），
 并建议将 /bin/sh 指向它，以获得更快的脚本执行速度。

#=======================================================================
 echo $VAR 与 echo "$VAR" 区别：没引号字符串合并为一行

#=======================================================================
# single and dobule quotes diff
#=======================================================================
 单引号和双引号区别：单引号中内容如果有空格会被当作命令来执行

#=======================================================================
# cat + heredoc
#=======================================================================
 cat << EOF
 EOF

#=======================================================================
# check string contains substring
#=======================================================================
 str="this is a string",
 [[ $str =~ "this" ]] && echo "\$str contains this" 
 [[ $str =~ "that" ]] || echo "\$str does NOT contain this"

#=======================================================================
# shell array
#=======================================================================
 COMMANDS=(suspend resume up halt ssh ssh-config)

#=======================================================================
# tabs format output sample
#=======================================================================
 sudo head -n 20 /etc/passwd |awk  -F ':'  '{print $1"\r\t   "$7}'
}}}

#=======================================================================
# => Argsparse Sample
#=======================================================================
 argsparse_use_option file 'file to save' short:f type:int value
 argsparse_use_option input 'input file to do' short:i cumulative value

 # Command line parsing is done here.
 argsparse_parse_options "$@"

 option="file"
 echo ${program_options[$option]}

 option="input"
 array_name="$(argsparse_get_cumulative_array_name "$option")[@]"
 array=( "${!array_name}" )
 arrprint array

 printf "Options reporting:\n"
 # Simple reporting function.
 argsparse_report
 printf "End of argsparse report.\n\n"

 function getfilename() {
   # argsparse_use_option all 'file to save' short:a type:int value
   # argsparse_parse_options "$@"
   # argsparse_report
 }

 getfilename

```

