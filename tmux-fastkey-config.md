> 文章转载自：https://linuxtoy.org/archives/from-screen-to-tmux.html

### 更改默认按键前缀

从 screen 切换到 tmux 十分平滑，tmux 的按键设置与 screen
大都相同，只是其默认按键前缀为 `Ctrl-b`。为了延续在 screen
中的使用习惯，我将其更改为 `Ctrl-a`。将下列内容加到 \$HOME/.tmux.conf
中即可：

    set -g prefix ^a
    unbind ^b
    bind a send-prefix

> 此段为新增内容

### 快捷键

    `!`  # Break the current pane out of the window.
    `"`  # Split the current pane into two, top and bottom.
    `#`  # List all paste buffers.
    `$`  # Rename the current session.
    `%`  # Split the current pane into two, left and right.
    `&`  # Kill the current window.
    `'`  # Prompt for a window index to select.
    `(`  # Switch the attached client to the previous session.
    `)`  # Switch the attached client to the next session.
    `,`  # Rename the current window.
    `-`  # Delete the most recently copied buffer of text.
    `.`  # Prompt for an index to move the current window.
    `0`  # to 9 Select windows 0 to 9.
    `:`  # Enter the tmux command prompt.
    `;`  # Move to the previously active pane.
    `=`  # Choose which buffer to paste interactively from a list.
    `?`  #  List all key bindings.
    `D`  # Choose a client to detach.
    `L`  # Switch the attached client back to the last session.
    `[`  # Enter copy mode to copy text or view the history.
    `]`  # Paste the most recently copied buffer of text.
    `c`  # Create a new window.
    `d`  # Detach the current client.
    `f`  # Prompt to search for text in open windows.
    `i`  # Display some information about the current window.
    `l`  # Move to the previously selected window.
    `n`  # Change to the next window.
    `o`  # Select the next pane in the current window.
    `p`  # Change to the previous window.
    `q`  # Briefly display pane indexes.
    `r`  # Force redraw of the attached client.
    `s`  # Select a new session for the attached client interactively.
    `t`  # Show the time.
    `w`  # Choose the current window interactively.
    `x`  # Kill the current pane.
    `z`  # Toggle zoom state of the current pane.
    `{`  # Swap the current pane with the previous pane.
    `}`  # Swap the current pane with the next pane.
    `~`  # Show previous messages from tmux, if any.
    `Page Up`  # Enter copy mode and scroll one page up.
    Up, Down
    Left, Right
    # Change to the pane above, below, to the left, or to the right of the current pane.
    M-1 to M-5
    # Arrange panes in one of the five preset layouts: even-horizontal, even-vertical, 
    # main-horizontal, main-vertical, or tiled.
    `Space`  # Arrange the current window in the next preset layout.
    `M-n`    # Move to the next window with a bell or activity marker.
    `M-o`    # Rotate the panes in the current window backwards.
    `M-p`    # Move to the previous window with a bell or activity marker.
    C-Up, C-Down
    C-Left, C-Right
    # Resize the current pane in steps of one cell.
    M-Up, M-Down
    M-Left, M-Right
    # Resize the current pane in steps of five cells.



---------------------------


### 按键绑定

我还在 .tmux.conf 中定义了以下按键绑定：

- 水平或垂直分割窗口

```
unbind '"'
bind - splitw -v # 分割成上下两个窗口
unbind %
bind | splitw -h # 分割成左右两个窗口
```

- 选择分割的窗格

```
bind k selectp -U # 选择上窗格
bind j selectp -D # 选择下窗格
bind h selectp -L # 选择左窗格
bind l selectp -R # 选择右窗格
```

- 重新调整窗格的大小

```
bind ^k resizep -U 10 # 跟选择窗格的设置相同，只是多加 Ctrl（Ctrl-k）
bind ^j resizep -D 10 # 同上
bind ^h resizep -L 10 # ...
bind ^l resizep -R 10 # ...
```

- 交换两个窗格

```
bind ^u swapp -U # 与上窗格交换 Ctrl-u
bind ^d swapp -D # 与下窗格交换 Ctrl-d
```

- 执行命令，比如看 Manpage、查 Perl 函数

```
bind m command-prompt "splitw -h 'exec man %%'"
bind @ command-prompt "splitw -h 'exec perldoc -f %%'"
```

### 定制状态行

状态行左边默认就很好了，我对右边定制了一下，显示 uptime 和 loadavg：

    set -g status-right "#[fg=green]#(uptime.pl)#[default] • #[fg=green]#(cut -d ' ' -f 1-3 /proc/loadavg)#[default]"

下面两行设置状态行的背景和前景色:

    set -g status-bg black
    set -g status-fg yellow

### 默认启动应用

当 tmux 启动时，可以默认启动一些应用：

    new -s work mutt # 新建名为 work 的会话，并启动 mutt
    neww rtorrent # 启动 rtorrent
    neww vim # 启动 vim
    neww zsh
    selectw -t 3 # 默认选择标号为 3 的窗口

### 复制与粘贴操作

1. 按 `C-a [` 进入复制模式，如果有设置&nbsp;`setw -g mode-keys vi`&nbsp;的话，可按
   vi 的按键模式操作。移动至待复制的文本处，按一下空格，结合 vi
   移动命令开始选择，选好后按回车确认。

2. 按 `C-a ]` 粘贴已复制的内容。

### 参考

tmux的官方主页: [http://tmux.sourceforge.net](http://tmux.sourceforge.net/)  
我的 [.tmux.conf](https://bitbucket.org/xuxiaodong/dotman/src)

**Read More:**

- [tmux：GNU screen 替代品](https://linuxtoy.org/archives/tmux.html "Permanent Link: tmux：GNU screen 替代品")
- [tmux 1.7 发布](https://linuxtoy.org/archives/tmux-1-7.html "Permanent Link: tmux 1.7 发布")
- [脚本化 tmux](https://linuxtoy.org/archives/scripting-tmux.html "Permanent Link: 脚本化 tmux")
- [tmux 1.4 发布](https://linuxtoy.org/archives/tmux-14.html "Permanent Link: tmux 1.4 发布")
- [tmux powerline](https://linuxtoy.org/archives/tmux-powerline.html "Permanent Link: tmux powerline")

------------------------

```
#
# author:    Xu Xiaodong <xxdlhy@gmail.com>
# modified:  2012 Apr 16
#

#-- base --#

set -g default-terminal "screen-256color"
set -g display-time 3000
set -g history-limit 65535
set -g base-index 1
set -g pane-base-index 1
set -s escape-time 0

#-- mouse --#
set -g mouse-resize-pane on
set -g mouse-select-pane on
set -g mouse-select-window on
set -g mouse-utf8 on
setw -g mode-mouse on

#-- bindkeys --#

set -g prefix ^a
unbind ^b
bind a send-prefix

unbind '"'
bind - splitw -v
unbind %
bind | splitw -h

bind k selectp -U
bind j selectp -D
bind h selectp -L
bind l selectp -R

bind ^k resizep -U 10
bind ^j resizep -D 10
bind ^h resizep -L 10
bind ^l resizep -R 10

bind ^u swapp -U
bind ^d swapp -D

bind ^e last
bind q killp
bind a displayp

bind '~' splitw htop
bind ! splitw ncmpcpp
bind m command-prompt "splitw -h 'exec man %%'"
bind @ command-prompt "splitw 'exec perldoc -t -f %%'"
bind * command-prompt "splitw 'exec perldoc -t -v %%'"
bind % command-prompt "splitw 'exec perldoc -t %%'"
bind / command-prompt "splitw 'exec ri -T %% | less'"

bind C-c run "tmux save-buffer - | xsel -ib" \; display "Copied tmux buffer to system clipboard"
bind C-v run "tmux set-buffer \"$(xsel -ob)\"; tmux paste-buffer"
bind C-a run "tmux kill-server"
bind C-s run "tmux kill-session"

# bind r source-file ~/.tmux.conf \; display "Reloaded!"

#-- statusbar --#

set -g status-justify centre

set -g status-left "#[fg=green]#S:w#I.p#P#[default]"
set -g status-left-attr bright
set -g status-left-length 20

set -g status-right "#[fg=green]#(/home/xiaodong/bin/uptime)#[default] • #[fg=green]#(cut -d ' ' -f 1-3 /proc/loadavg)#[default]"
set -g status-right-attr bright

set -g status-utf8 on
set -g status-interval 1

set -g visual-activity off # Prevent tmux from displaying “Activity in window n”
setw -g monitor-activity on

setw -g automatic-rename off

set -g status-keys vi
setw -g mode-keys vi

#set -g status-bg black
#set -g status-fg yellow

#setw -g window-status-current-attr bright
#setw -g window-status-current-bg red
#setw -g window-status-current-fg white

#-- colorscheme --#
#-- see also: https://github.com/seebi/tmux-colors-solarized --#

# default statusbar colors
set -g status-bg colour235 #base02
set -g status-fg colour136 #yellow
set -g status-attr default

# default window title colors
setw -g window-status-fg colour244
setw -g window-status-bg default
setw -g window-status-attr dim

# active window title colors
setw -g window-status-current-fg colour166 #orange
setw -g window-status-current-bg default
setw -g window-status-current-attr bright

# not active window title status colors
set-option -gw window-status-activity-attr default
set-option -gw window-status-activity-bg colour235 
set-option -gw window-status-activity-fg colour244

# pane border
set -g pane-border-fg colour235 #base02
# set -g pane-active-border-fg colour240 #base01
set -g pane-active-border-fg colour235 #base01

# message text
set -g message-bg colour235 #base02
set -g message-fg colour166 #orange

# pane number display
set -g display-panes-active-colour colour33 #blue
set -g display-panes-colour colour166 #orange

# clock
setw -g clock-mode-colour colour64 #green

#-- apps --#
#-- warn: neww ... [command] , the command must be deamon program,
#-------  the window will be closed when the command exec finish.
# new -s fav -n home
# neww -n vim "vim"
# neww -n jekyll -c /media/d/jekyll/dark.github.io "jekyll s -w"
# neww -n ruby -c ~/RubyProjects
# # neww -n top htop # exit the program will kill the window in tmux.
# selectw -t 1
```
