## 一、Linux文件profile、bashrc、bash_profile区别

 profile、bashrc 和 bash_profile 都是与 Shell 相关的设置文件，它们的主要差别在于执行时机和范围。  

 在每次交互登录时，bash shell都会执行 .bash_profile。  

 如果在主目录中找不到 .bash_profile，bash将执行从 .bash_login 和 .profile 中找到的某个可读文件。

但是，在每次交互式非登录shell启动时，bash都会 .bashrc。

通常情况下，环境变量会被放入 .bash_profile。

由于交互式登录shell是名列前茅个shell，因此环境设置所需的所有默认设置都被放入.bash_profile。

因此，它们只设置一次而且在所有子shell中继承。

同样地，别名和函数也会被放入 .bashrc 确保每次从现有环境中启动shell时都加载这些。

然而，为了避免登录和非登录交互shell设置的差异。.bash_profile 调用 .bashrc。

具体区别如下：

1. **<mark style="background: #FF5582A6;">profile</mark>**：是一个<mark style="background: #FFB86CA6;">系统级别的文件</mark>，位于 /etc/profile 或 /etc/profile.d 目录下，用于全局配置环境变量和用户登录 Shell 时的一些操作，比如系统启动时就会执行 /etc/profile 文件。
2. **<mark style="background: #FF5582A6;">bashrc</mark>**：是一个<mark style="background: #FFB86CA6;">用户级别的文件</mark>，通常位于用户的 home 目录下，用于定义用户自己的 Bash 环境，包括 alias 别名、环境变量等。当用户打开一个新的终端窗口时，该文件将会执行。
3. **<mark style="background: #FF5582A6;">bash_profile</mark>**：也是<mark style="background: #FFB86CA6;">用户级别的文件</mark>，通常位于用户的 home 目录下，该文件是专门用来定义 Bash 的个人环境设置，当用户登录到系统的时候执行该文件。如果存在 bash_profile 文件，则不会执行 bashrc 文件。

在使用时，通常会<mark style="background: #FFB86CA6;">在 profile 文件中引入 bashrc 文件</mark>（通过 source 命令），以使得用户自定义的环境变量和别名等能够被全局使用。而 bash_profile 文件则是定义个人化的 Bash 设置的优异选择。

## 二、Linux文件profile、bashrc、bash_profile简介

### 1、profile

**作用**：用于设置<mark style="background: #FFB86CA6;">系统级</mark>的环境变量和启动程序，在这个文件下配置会<mark style="background: #FFB86CA6;">对所有用户生效</mark>。

当用户登录（login）时，文件会被执行，并从/etc/profile.d目录的配置文件中查找shell设置。

在profile中设置环境变量，一般不建议在/etc/profile文件中添加环境变量，因为在这个文件中添加的设置会对所有用户起作用。

当必须添加时，我们可以按以下方式添加：

添加一个 HOST 值为 xx.cn的环境变量：

```
export HOST=xx.cn
```

添加时，可以在行尾使用;号，也可以不使用。一个变量名可以对应多个变量值，多个变量值需要使用:进行分隔。

添加环境变量后，需要重新登录才能生效，也可以使用 source 命令强制立即生效：

```
source /etc/profile
```

查看是否生效可以使用 echo 命令：

```
$ echo $HOSTxx.cn
```

### 2、bashrc

**作用**：bashrc 文件用于<mark style="background: #FFB86CA6;">配置函数或别名</mark>。

- bashrc 文件有两种级别：
- 系统级
- 用户级

系统级的位于/etc/bashrc，对所有用户生效。

用户级的位于~/.bashrc，仅对当前用户生效。

bashrc 文件只会对指定的 shell 类型起作用，bashrc 只会被 bash shell 调用。

### 3、bash_profile

bash_profile只<mark style="background: #FFB86CA6;">对单一用户有效</mark>，文件存储位于~/.bash_profile

该文件是一个用户级的设置，可以理解为某一个用户的 profile 目录下。

这个文件同样也可以<mark style="background: #FFB86CA6;">用于配置环境变量和启动程序</mark>，但只针对单个用户有效。

和 profile 文件类似，bash_profile 也会在用户登录（login）时生效，也可以用于设置环境变理。

但与 profile 不同，bash_profile 只会对当前用户生效。

---

`/etc/profile` 此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行.并从`/etc/profile.d`目录的配置文件中搜集shell的设置。

`/etc/bashrc` 为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取。

`~/.bash_profile` 每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件**仅仅执行一次**! 默认情况下,他设置一些环境变量,执行用户的`.bashrc`文件。

`~/.bashrc` 该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该该文件被读取。

`~/.bash_logout` 少见，但是意味着当每次退出系统(退出bash shell)时,执行该文件。

另外`/etc/profile` 中设定的变量(全局)的可以作用于任何用户, 而`~/.bashrc` 等中设定的变量(局部)只能继承 `/etc/profile` 中的变量,他们是"父子"关系。

---

`profile`用于登录式shell, 而`bashrc`用于每个交互式shell

`~/.bash_profile` 是交互式、login 方式进入 bash 运行的

`~/.bashrc` 是交互式 non-login 方式进入 bash 运行的

通常二者设置大致相同，所以通常前者会调用后者。

所以一般优先把变量设置在`.bashrc`里面。比如在crontab里面执行一个命令，`.bashrc` 设置的环境变量会生效，而 `.bash_profile` 不会。

设置生效：可以重启生效，也可以使用命令：`source`

作者：LeoinUSA  
链接：https://www.jianshu.com/p/11d71bacef1a  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
