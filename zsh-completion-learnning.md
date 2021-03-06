> Come from: http://www.linux-mag.com/id/1106/

If you've been reading this column for the past few months, you've learned about the zsh shell's (http://www.zsh.org) fabulous tab completion system. By adding the following two lines to your \$HOME/.zshrc file, you can use the tab key to not only expand file names, but also get lists of command-line options.

By [John Beppu](http://www.linux-mag.com/author/175)

Monday, July 15th, 2002

If you’ve been reading this column for the past few months, you’ve learned about the *zsh* shell’s ([http://www.zsh.org](http://www.zsh.org/)) fabulous tab completion system. By adding the following two lines to your *\$HOME/.zshrc* file, you can use the tab key to not only expand file names, but also get lists of command-line options.

~~~~
autoload -U compinit
compinit
~~~~

To provide the list of command-line options for a given Linux command, *zsh* executes the*completion function* associated with that command (i.e., if you type *ls* and press the tab key,*zsh* executes the ls completion function). Each completion function lists command options and shows what options require arguments. A completion function is also context-sensitive. For example, type `man 1` and hit the tab key. Notice the names of the man pages. Next, type `man 3` followed by tab. The list is different. The completion function for `man` is section-specific.

*zsh* includes completion functions for many Linux commands, but not all of them. Hang on. Don’t fret. You can extend the capabilities of *zsh* by writing your own completion functions.

**A Caveat**

Although it’s actually quite easy to write completion func-tions, you’d never know it by reading the *zsh* documentation. Currently, there’s no single source of information to quickly teach you how to write them.

The man pages *zshcompsys* and *zshcompwid* are the most complete source of information, but they don’t explain how to write a completion function from start to finish. Thus, they are best used as a reference.

Similarly, the *Zsh User’s Guide* provides many annotated examples, but lacks a thorough tutorial. It also presents some very advanced material without a lot of background.

The most practical source of information is the set of pre-written completion functions distributed with *zsh* (you can find these by looking in the directories enumerated in the `$fpath`variable). However, even the source code is rather incomplete. It’s poorly commented and not intended as a teaching aid. Most of the existing completion functions are like masterworks that the young function programmer should aspire to.

While it might sound like learning to write completion functions is a near-hopeless task, that’s not the case. The information in this article will help make the other documentation easier to understand.

**Getting Started**

If you’re ready to take the plunge into the world of completion functions, start by adding the lines in *Figure One* to your *\$HOME/.zshrc* file. These settings instruct completion functions to be as verbose as possible. Even if you decide not to write your own completion functions, the settings will let you experience the full power of them.


Figure One: Useful lines in your .zshrc file


    zstyle ‘:completion:*’ verbose yes
    zstyle ‘:completion:*:descriptions’ format ‘%B%d%b’
    zstyle ‘:completion:*:messages’ format ‘%d’
    zstyle ‘:completion:*:warnings’ format ‘No matches for: %d’
    zstyle ‘:completion:*’ group-name ”


If you’re ready to take the plunge into the world of completion functions, start by adding the lines in *Figure One* to your *\$HOME/.zshrc* file. These settings instruct completion functions to be as verbose as possible. Even if you decide not to write your own completion functions, the settings will let you experience the full power of them.


Figure One: Useful lines in your .zshrc file


    zstyle ‘:completion:*’ verbose yes
    zstyle ‘:completion:*:descriptions’ format ‘%B%d%b’
    zstyle ‘:completion:*:messages’ format ‘%d’
    zstyle ‘:completion:*:warnings’ format ‘No matches for: %d’
    zstyle ‘:completion:*’ group-name ”


**The Basic Mechanics of the Completion System**

If you look at the actual source of the completion functions (by searching through the directories in `$fpath`), you might notice how often the `_arguments()` function is used. It’s everywhere because it’s the most important function authors of completion functions use.

The `_arguments()` function is actually a wrapper around the `compadd()` built-in function. It’s`compadd()` that ultimately takes information and feeds it to the core of the completion system. Another important function to be aware of is `compdef()` which is used to bind completion functions to commands.

**A Completion Function for *Figlet***

*Listing One* shows a completion function for the command *figlet*([http://ianchai.50megs.com/figlet.html](http://ianchai.50megs.com/figlet.html)), a program that takes text and renders each letter using ASCII art fonts.


> Listing One: The _figlet completion function
>
     1   #compdef figlet
     2
     3   typeset -A opt_args
     4   local context state line
     5   local fontdir
     6   fontdir=$(_call_program path figlet -I2 2>/dev/null)
     7
     8   _arguments -s -S \
     9     “(-l -c -r)-x[use default justification of font]” \
    10     “(-x -c -r)-l[left justify]” \
    11     “(-x -l -r)-c[center justify]” \
    12     “(-x -l -c)-r[right justify]” \
    13     “(-S -s -o -W -m)-k[use kerning]” \
    14     “(-k -s -o -W -m)-S[smush letters together or else!]” \
    15     “(-k -S -o -W -m)-s[smushed spacing]” \
    16     “(-k -S -s -W -m)-o[let letters overlap]” \
    17     “(-k -S -s -o -m)-W[wide spacing]” \
    18     “(-p)-n[normal mode]” \
    19     “(-n)-p[paragraph mode]” \
    20     “(-E)-D[use Deutsch character set]” \
    21     “(-D)-E[use English character set]” \
    22     “(-X -R)-L[left-to-right]” \
    23     “(-L -X)-R[right-to-left]” \
    24     “(-L -R)-X[use default writing direction of font]” \
    25     “(-w)-t[use terminal width]” \
    26     “(-t)-w+[specify output width]:output width (in columns):” \
    27     “(-k -S -s -o -W)-m+[specify layout mode]:layout mode:” \
    28     “(-I)-v[version]” \
    29     “(-v)-I+[display info]:info code:(-1 0 1 2 3 4)” \
    30     “-d+[specify font directory]:font directory:_files -/” \
    31     “-f+[specify font]:font:->fonts” \
    32     “(-N)-C+[specify control file]:control file:->controls” \
    33     “(-C)-N[clear controlfile list]” \
    34     && return 0
    35
    36   (( $+opt_args[-d] )) && fontdir=$opt_args[-d]
    37
    38   case $state in
    39   (fonts)
    40     _files -W $fontdir -g ‘*flf*(:r)’ && return 0
    41     ;;
    42   (controls)
    43     _files -W $fontdir -g ‘*flc*(:r)’ && return 0
    44     ;;
    45   esac
    46
    47   return 1


To use this function, store it in a file called *_figlet* and place it somewhere in your `$fpath`. If you don’t w ant (or don’t have the permission) to add files into a directory in `$fpath`, you can create a directory in your home directory (for example, *fun*) and prepend that directory to the list in `$fpath` by adding these commands to your *\$HOME/.zshrc.*

~~~~
fpath=(~/fun $fpath)
autoload -U ~/fun/*(:t)
~~~~

When you next start *zsh,* the completion system will see the *_figlet* completion function (and any other completion functions stored in *fun*).

The leading underscore is necessary because `compinit()` (the built-in that initializes the completion system) looks in the `$fpath` directories for files that have leading underscores. Then, based on the first line of the file, `compinit()` will take various actions.

The first line of any completion function should start with `#compdef` followed by the command the function performs completions for. That’s the case in *Listing One.*

Lines 3 and 4 initialize the variables `$opt_args`, `$context`, `$state`, and `$line`.`$opt_args` is an associative array that contains command-line options like `-d` or `-f` as its keys and the actual parameters to those options (if any) as its values. `$state` is a scalar variable used by the state mechanism in the `_arguments() `function (we’ll talk about state later on in this article). The other two variables, `$context` and `$line` won’t be used directly in this function, but `_arguments()` uses them behind the scenes. The reason they are declared as local in this *_figlet* function is to prevent namespace pollution. (*zsh* uses dynamic scoping for its variables rather than lexical scoping).

We’ve declared one other variable, `$fontdir`, on line 5 and it’s unique to the *_figlet* function. Its purpose is to hold the directory where the *figlet* fonts are stored. We can find out what this directory is by executing `figlet -I2` on line 6, but notice that we don’t just call the program directly.

Instead, we call the `_call_program()` function to execute `figlet -I2` and redirect standard error to `/dev/null`. To save the output of an external command, you should use`_call _program()` as it lets advanced users override the actual program that is called. Doing this for *figlet* is somewhat contrived, but it’s something to be aware of when writing completion functions.

**Calling the _arguments Function**

With initialization behind us, we’re finally ready to call `_arguments()`. This function reduces the task of writing completion functions to an exercise in taking human-readable documentation and turning it into something that’s machine-readable. If you run *man figlet* you’ll see that lines 9 through 34 of the code are identical to the man page.

Let’s start by looking at lines 9 through 12 which describe the options for line justification. Each of these lines has three distinct parts.

-   An optional exclusion list surrounded by parentheses
-   The option itself
-   Help text surrounded by square brackets

The purpose of the exclusion list becomes clear when you ask yourself, “Does it make sense to left-justify and center text at the same time?” It doesn’t, and you have to tell *zsh *that explicitly. For example, line 12 says that if `-r` appears on the command line, do not provide the options `-x`, `-l`, or `-c` as completions.

That’s all there is to simple single-letter options. If you look closely, lines 9 through 25 have handled all of *figlet*‘s single-letter options. Now we have to move on to more complicated types of arguments.

On line 26, we encounter an option that takes an argument. The `-w` option tells *figlet* how many characters wide the output should be, and it wants an integer as an argument. There are two things different in line 26. First, the option part has a `+` after it — that means this option can take another argument in the same word or in the next word. For example, `-w 64` and `-w72`would both be valid. The second difference is that there’s an additional string after the help text that’s used as a hint when the user presses the tab key (`output width (in columns)`). However, this text is displayed only if you’ve enabled verbose mode as described in *Figure One* (pg. 40). The final colon is a separator between the hint and an action, but there’s no action for `-w`.

On line 29, though, there is an option that *does* use an action. Here the option (`-I`) takes an argument. There are only six possible values that can be used with `-I`; after the hint, we list those six arguments in parentheses.

Line 30 adds another variation. The `-d` option requires a directory as an argument. We use the*zsh* internal function called `_files()` to build and present the list. We pass `-/` as an argument so that only directories are presented in the completion list.

Line 31 is the last variation we’ll see in the completion function for *_figlet*. Sometimes, building a completion list for an option’s arguments is non-trivial and might have to be done with custom code. That’s when we use the *state mechanism.* See where it says, “`->fonts`“? That means, “Set the `$state` variable to ‘fonts’ and let me handle it.”

Up to this point, `_arguments()` has been doing most of the work, and the flow of control never needed to go past line 34. However, when we use the state mechanism, we’re going to fall through and hit line 36 which will load the `$fontdir` variable with the value of any `-d`argument that was specified. Then on line 38, we’re going to hit the `case` statement.

Based on the value of the `$state` variable, we’re going to build a completion list for either fonts or control files. In both cases, we’re going to use the `_files()` function again (lines 40 and 43). We use the `-W` option to `_files()` to start the search for completions in the directories that `$fontdir` contains, and with the `-g` option, we specify a glob pattern that limits which files are enumerated for the completion list.

And, we’re done. We return 0 if there have been no problems or 1 (or non-zero) if there is any kind of failure (hopefully there won’t ever be). That’s it for a completion function.

**What’s Next?**

Writing a completion function can be a lot more complicated than our example. If you found the function for *figlet* too easy, take a look at the completion functions for *tar, cvs,* and *ssh.* They can teach you a lot, but your code-reading skills will have to be fairly strong.

Still, you can do a lot with what you’ve learned here. Anything you don’t know can be found out by looking through the documentation. And again, the source of other completion functions is a good place to look for real-life examples (though they will not be well-documented).

Finally, the *zsh-workers* mailing list ([http://zsh.sunsite.dk/Arc/mlist.html](http://zsh.sunsite.dk/Arc/mlist.html)) is full of helpful people, so don’t be afraid to subscribe. And remember, if you choose to contribute your work back to the community, realize that you’re helping to making the *zsh* experience better for everyone.


> Reloading Completion Functions

> When writing your own completion functions, you’ll find yourself reloading the function frequently for debugging purposes. Here’s a helpful function to blindly unfunction and then autoload everything in~/fun.

> 
    r() {
      local f
      f=(~/fun/*(.))
      unfunction $f:t 2> /dev/null
      autoload -U $f:t
    }



* * * * *

*John Beppu \<[beppu@cpan.org](mailto:beppu@cpan.org)\> encourages you to use your computer skillfully and creatively.*

Fatal error: Call to undefined function aa_author_bios() in /opt/apache/dms/b2b/linux-mag.com/site/www/htdocs/wp-content/themes/linuxmag/single.php on line 62