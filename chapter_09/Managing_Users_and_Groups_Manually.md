### 手动管理用户和组

如同Slackware Linux其他的东西一样，用户和组信息也是存储为纯文本。这意味着你只需要编辑文本文件，就能修改用户细节，创建用户，创建家目录等。当然，你观赏如何做到这一切之后你就会发现蕴含其中的简单的美感。

我们的第一站是`/etc/passwd`文件。除了密码（这一点略显奇特）以外，所有用户信息都存放在这里。原因十分简单，因为`/etc/passwd`必须要让系统里所有用户都有读权限，所以尽管密码是加密过的，也不能存放在这里。让我们来瞟一眼这个文件：

```
alan:x:1000:100:,,,:/home/alan:/bin/bash
```

这个文件的每一行都包含一些由冒号区分开的区域。其含义分别为（从左到右）：*用户名*、*密码*、*UID*、*GUID*、*注解栏*、*家目录*和*登陆shell*。你会发现每行的密码密码区都是`x`, 这是因为Slackware使用了影子口令，所以真实的加密过后的密码都存放在`/etc/shadow`。让我们看一眼：

```
alan:$1$HlR?M3fkL@oeJmsdLfhsLFM*4dflPh8:14197:0:99999:7:::
```

`shadow`文件除了密码外还包含了一些其他内容。它们分别是（从左到右）：*用户名*、*加密后的密码*、*上一次修改密码的时间*（单位：天数）、*下一次应该更新密码的时间间隔*（单位：天数）、*密码失效前的天数*、*用户由于密码失效被禁用的时间*和*保留域*。你会注意到这些“天数”们有一些是很大的数字，原因是Slackware从“Epoch”（即1970年01月01日）那一天开始计时。

要想新建一个用户，你需要使用`vipw(8)`来打开这些文件，它会使用你的`VISUAL`环境变量（如果没有定义`VISUAL`的话，会转而使用`EDITOR`。如果两个环境变量都不存在，就会使用`vi`）指定的编辑器打开`/etc/passwd`。如果使用`-s`参数的话，它会打开`/etc/shadow`。请务必使用`vipw`而不是直接使用编辑器打开，因为它会从此刻起锁定文件禁止其他程序编辑。

这就是你所需的全部工具。你还要创建用户的家目录，并用`passwd`为它设置密码。

