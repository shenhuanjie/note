# CentOS7目录与文件操作

* pwd	查看当前所在目录位置

```bash
pwd: 用法:pwd [-LP]
```

* touch  创建文件目录

```bash
用法：touch [选项]... 文件...
Update the access and modification times of each FILE to the current time.

A FILE argument that does not exist is created empty, unless -c or -h
is supplied.

A FILE argument string of - is handled specially and causes touch to
change the times of the file associated with standard output.

Mandatory arguments to long options are mandatory for short options too.
  -a                    只更改访问时间
  -c, --no-create       不创建任何文件
  -d, --date=字符串     使用指定字符串表示时间而非当前时间
  -f                    (忽略)
  -h, --no-dereference          会影响符号链接本身，而非符号链接所指示的目的地
                                (当系统支持更改符号链接的所有者时，此选项才有用)
  -m                    只更改修改时间
  -r, --reference=FILE   use this file's times instead of current time
  -t STAMP               use [[CC]YY]MMDDhhmm[.ss] instead of current time
      --time=WORD        change the specified time:
                           WORD is access, atime, or use: equivalent to -a
                           WORD is modify or mtime: equivalent to -m
      --help            显示此帮助信息并退出
      --version         显示版本信息并退出

请注意，-d 和-t 选项可接受不同的时间/日期格式。

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
请向<http://translationproject.org/team/zh_CN.html> 报告touch 的翻译错误
要获取完整文档，请运行：info coreutils 'touch invocation'
```

* ls    查看目录
* ls /tmp 查看tmp目录

```bash
用法：ls [选项]... [文件]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                     不隐藏任何以. 开始的项目
  -A, --almost-all              列出除. 及.. 以外的任何项目
      --author                  与-l 同时使用时列出每个文件的作者
  -b, --escape                  以八进制溢出序列表示不可打印的字符
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'never', 'auto',
                               or 'always' (the default); more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
      --file-type            likewise, except do not append '*'
      --format=WORD          across -x, commas -m, horizontal -x, long -l,
                               single-column -1, verbose -l, vertical -C
      --full-time            like -l --time-style=full-iso
  -g                            类似-l，但不列出所有者
      --group-directories-first
                             group directories before files;
                               can be augmented with a --sort option, but any
                               use of --sort=none (-U) disables grouping
  -G, --no-group                以一个长列表的形式，不输出组名
  -h, --human-readable          与-l 一起，以易于阅读的格式输出文件大小
                                (例如 1K 234M 2G)
      --si                      同上面类似，但是使用1000 为基底而非1024
  -H, --dereference-command-line
                             follow symbolic links listed on the command line
      --dereference-command-line-symlink-to-dir
                             follow each command line symbolic link
                               that points to a directory
      --hide=PATTERN         do not list implied entries matching shell PATTERN
                               (overridden by -a or -A)
      --indicator-style=WORD  append indicator with style WORD to entry names:
                               none (default), slash (-p),
                               file-type (--file-type), classify (-F)
  -i, --inode                print the index number of each file
  -I, --ignore=PATTERN       do not list implied entries matching shell PATTERN
  -k, --kibibytes            default to 1024-byte blocks for disk usage
  -l                            使用较长格式列出信息
  -L, --dereference             当显示符号链接的文件信息时，显示符号链接所指示
                                的对象而并非符号链接本身的信息
  -m                            所有项目以逗号分隔，并填满整行行宽
  -n, --numeric-uid-gid         类似 -l，但列出UID 及GID 号
  -N, --literal                 输出未经处理的项目名称 (如不特别处理控制字符)
  -o                            类似 -l，但不列出有关组的信息
  -p,  --indicator-style=slash  对目录加上表示符号"/"
  -q, --hide-control-chars   print ? instead of nongraphic characters
      --show-control-chars   show nongraphic characters as-is (the default,
                               unless program is 'ls' and output is a terminal)
  -Q, --quote-name           enclose entry names in double quotes
      --quoting-style=WORD   use quoting style WORD for entry names:
                               literal, locale, shell, shell-always, c, escape
  -r, --reverse                 逆序排列
  -R, --recursive               递归显示子目录
  -s, --size                    以块数形式显示每个文件分配的尺寸
  -S                         sort by file size
      --sort=WORD            sort by WORD instead of name: none (-U), size (-S),
                               time (-t), version (-v), extension (-X)
      --time=WORD            with -l, show time as WORD instead of default
                               modification time: atime or access or use (-u)
                               ctime or status (-c); also use specified time
                               as sort key if --sort=time
      --time-style=STYLE     with -l, show times using style STYLE:
                               full-iso, long-iso, iso, locale, or +FORMAT;
                               FORMAT is interpreted like in 'date'; if FORMAT
                               is FORMAT1<newline>FORMAT2, then FORMAT1 applies
                               to non-recent files and FORMAT2 to recent files;
                               if STYLE is prefixed with 'posix-', STYLE
                               takes effect only outside the POSIX locale
  -t                         sort by modification time, newest first
  -T, --tabsize=COLS         assume tab stops at each COLS instead of 8
  -u                         with -lt: sort by, and show, access time;
                               with -l: show access time and sort by name;
                               otherwise: sort by access time
  -U                         do not sort; list entries in directory order
  -v                         natural sort of (version) numbers within text
  -w, --width=COLS           assume screen width instead of current value
  -x                         list entries by lines instead of by columns
  -X                         sort alphabetically by entry extension
  -1                         list one file per line

SELinux options:

  --lcontext                 Display security context.   Enable -l. Lines
                             will probably be too wide for most displays.
  -Z, --context              Display security context so it fits on most
                             displays.  Displays only mode, user, group,
                             security context and file name.
  --scontext                 Display only security context and file name.
      --help            显示此帮助信息并退出
      --version         显示版本信息并退出

SIZE is an integer and optional unit (example: 10M is 10*1024*1024).  Units
are K, M, G, T, P, E, Z, Y (powers of 1024) or KB, MB, ... (powers of 1000).

使用色彩来区分文件类型的功能已被禁用，默认设置和 --color=never 同时禁用了它。
使用 --color=auto 选项，ls 只在标准输出被连至终端时才生成颜色代码。
LS_COLORS 环境变量可改变此设置，可使用 dircolors 命令来设置。


退出状态：
 0  正常
 1  一般问题 (例如：无法访问子文件夹)
 2  严重问题 (例如：无法使用命令行参数)

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
请向<http://translationproject.org/team/zh_CN.html> 报告ls 的翻译错误
要获取完整文档，请运行：info coreutils 'ls invocation'
```



* rm a.c    删除文件
* locale -a |grep "zh_CN"    查看系统是否安装中文语言包
* echo $LANG    查看当前系统语言环境
* locale
* vim /etc/locale.conf
* ifconfig

---

**1. 新建文件夹**

* mkdir 文件名
* mkdir /home/test    新建一个名为test的文件夹在home下

**2. 新建文本**

* vi /home/test.sh    在home下新建一个test.sh脚本

**3. 删除文件或文件夹**

* rm /home/test    删除home目录下的test目录

* rm -r /home/test    这种不带参数的删除方法经常会提示无法删除，因为权限不够。

* rm -rf /home/test    -r是递归的删除参数表中的目录及其子目录。 目录将被清空并且删除。 当删除目录包含的具有写保护的文件时用户通常是被提示的。
* rm -ir /home/test    -f是不提示用户，删除目录下的所有文件。请注意检查路径，输成别的目录就悲剧了。
* rm -ir /home/test    -i是交互模式。使用这个选项，rm命令在删除任何文件前提示用户确认。

```bash
用法：rm [选项]... 文件...
Remove (unlink) the FILE(s).

  -f, --force           ignore nonexistent files and arguments, never prompt
  -i                    prompt before every removal
  -I                    prompt once before removing more than three files, or
                          when removing recursively; less intrusive than -i,
                          while still giving protection against most mistakes
      --interactive[=WHEN]  prompt according to WHEN: never, once (-I), or
                          always (-i); without WHEN, prompt always
      --one-file-system         递归删除一个层级时，跳过所有不符合命令行参
                                数的文件系统上的文件
      --no-preserve-root  do not treat '/' specially
      --preserve-root   do not remove '/' (default)
  -r, -R, --recursive   remove directories and their contents recursively
  -d, --dir             remove empty directories
  -v, --verbose         explain what is being done
      --help            显示此帮助信息并退出
      --version         显示版本信息并退出

默认时，rm 不会删除目录。使用--recursive(-r 或-R)选项可删除每个给定
的目录，以及其下所有的内容。

To remove a file whose name starts with a '-', for example '-foo',
use one of these commands:
  rm -- -foo

  rm ./-foo

请注意，如果使用rm 来删除文件，通常仍可以将该文件恢复原状。如果想保证
该文件的内容无法还原，请考虑使用shred。

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
请向<http://translationproject.org/team/zh_CN.html> 报告rm 的翻译错误
要获取完整文档，请运行：info coreutils 'rm invocation'
```

