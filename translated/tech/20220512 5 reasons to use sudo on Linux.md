[#]: subject: "5 reasons to use sudo on Linux"
[#]: via: "https://opensource.com/article/22/5/use-sudo-linux"
[#]: author: "Seth Kenlon https://opensource.com/users/seth"
[#]: collector: "lkxed"
[#]: translator: "MjSeven"
[#]: reviewer: " "
[#]: publisher: " "
[#]: url: " "

在 Linux 上使用 sudo 命令的 5 个理由
======
以下是切换到 Linux sudo 命令的五个安全原因。下载 sudo 备忘录获取更多技巧。

![命令行提示符][1]
Image by: Opensource.com

在传统的 Unix 和类 Unix 系统上，新系统中存在的第一个也是唯一一个用户是 **root**。使用 root 账户登录并创建“普通”用户。在初始交互之后，你应该以普通用户身份登录。

以普通用户身份运行系统是一种自我施加的限制，可以防止你犯愚蠢的错误。例如，作为普通用户，你不能删除定义网络接口的配置文件或意外覆盖用户和组列表。作为普通用户，你无法犯这些错误，因为你无权访问这些重要文件。当然，作为系统的实际所有者，你始终可以使用 `su` 命令成为超级用户（root）并做任何你想做的事情，但对于日常任务，你应该使用普通账户。

几十年来，`su` 运行良好，但随后出现了 `sudo` 命令。

对于长期的超级用户来说，`sudo` 命令乍一看似乎是多余的。在某些方面，它感觉很像 `su` 命令。例如：

```
$ su root
<enter passphrase>
# dnf install -y cowsay
```

`sudo` 做同样的事情：

```
$ sudo dnf install -y cowsay
<enter passphrase>
```

它们的作用几乎完全相同。然而大多数发行版推荐使用 `sudo` 而不是 `su`，而且大多数发行版已经完全取消了 root 账户。让 Linux 变得愚蠢是一个阴谋吗？

事实上，并非如此。`sudo` 使 Linux 比以往任何时候都更加灵活和可配置，并且没有损失功能，还有[几个显著的优点][2]。

### 为什么在 Linux 上 sudo 比 root 更好？

以下是你应该使用 `sudo` 而不是 `su` 的五个原因。

### 1. Root 是确认的对象

我使用 [Firewalls][3]、[fail2ban][4] 和 [SSH 密钥][5]的常用组合来防止一些针对服务器不必要的访问。在我理解 `sudo` 的价值之前，我对日志中的暴力攻击感到恐惧。自动尝试以 root 身份登录是最常见的，这是有充分理由的。

有足够知识尝试入侵的攻击者应该也知道，在广泛使用 `sudo` 之前，基本上每个 Unix 和 Linux 都有一个 root 账户。这样攻击者就会少一种猜测。因为登录名总是正确的，只要它是 root 就行，所以攻击者只需要一个有效的密码。

删除 root 账户可提供大量保护。如果没有 root，服务器就没有确认的登录账户。攻击者必须猜测登录名以及密码。这不是两次猜测，而是两个必须同时正确的猜测。

### 2. Root 是最终的攻击媒介

在失败访问日志中，root 是很常见的，因为它可能是最强大的用户。如果你要设置一个脚本强行进入他人的服务器，为什么要浪费时间尝试以一个只有部分权限的普通用户进入呢？只有最强大的用户才有意义。

Root 既是唯一已知的用户名，又是最强大的用户账户。因此，root 基本上使尝试暴力破解其他任何东西变得毫无意义。

### 3. 可选择的权限

`su` 命令要么全有要么全没有。如果你有 `su root` 的密码，你就可以变成超级用户。如果你没有 `su` 的密码，那么你就没有任何管理员权限。这个模型的问题在于，系统管理员必须在将主密钥移交系统或保留密钥和对系统的所有控制权之间做出选择。这并不总是你想要的，[有时候你只是想授权。][6]

例如，假设你想授予用户以 root 身份运行特定应用程序的权限，但你不想为用户提供 root 密码。通过编辑 `sudo` 配置，你可以允许指定用户，或属于指定 Unix 组的任何用户运行特定命令。`sudo` 命令需要用户的现有密码，而不是你的密码，当然也不是 root 密码。

### 4.超时

使用 `sudo` 运行命令时，经过身份验证的用户的权限会提升 5 分钟。在此期间，他们可以运行管理员授予他们运行权限的任何命令。

5 分钟后，认证缓存被清空，下次使用 `sudo` 再次提示输入密码。超时可防止用户意外执行某些操作（例如，不小心搜索 shell 历史记录或多次按下**向上**箭头）。如果第一个用户离开办工桌而没有锁定计算机屏幕，它还可以确保另一个用户不能运行这些命令。

### 5. 日志记录

Shell 历史功能可以作为一个用户所做事情的日志。如果你需要了解系统发生了什么，你可以（理论上，取决于 shell 历史记录的配置方式）使用 `su` 切换到其他人的账户，查看他们的 shell 历史记录，也可以了解用户执行了哪些命令。

但是，如果你需要审计 10 或 100 名用户的行为，你可能会注意到此方法无法扩展。Shell 历史记录的轮转速度很快，默认为 1000 条，并且可以通过在任何命令前加上空格来轻松绕过它们。

当你需要管理任务的日志时，`sudo` 提供了一个完整的[日志记录和警报子系统][7]，因此你可以在一个特定位置查看活动，甚至在发生重大事件时获得警报。

### 学习 sudo 其他功能

除了本文列举的一些功能，`sudo` 命令还有很多新功能，包括已有的或正在开发中的。因为 `sudo` 通常是你配置一次然后就忘记的东西，或者只在新管理员加入团队时才配置的东西，所以很难记住它的细微差别。

下载 [sudo 备忘录][8]，在你最需要的时候把它当作一个有用的指导书。

--------------------------------------------------------------------------------

via: https://opensource.com/article/22/5/use-sudo-linux

作者：[Seth Kenlon][a]
选题：[lkxed][b]
译者：[MjSeven](https://github.com/MjSeven)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: https://opensource.com/users/seth
[b]: https://github.com/lkxed
[1]: https://opensource.com/sites/default/files/lead-images/command_line_prompt.png
[2]: https://opensource.com/article/19/10/know-about-sudo
[3]: https://www.redhat.com/sysadmin/secure-linux-network-firewall-cmd
[4]: https://www.redhat.com/sysadmin/protect-systems-fail2ban
[5]: https://opensource.com/article/20/2/ssh-tools
[6]: https://opensource.com/article/17/12/using-sudo-delegate
[7]: https://opensource.com/article/19/10/know-about-sudo
[8]: https://opensource.com/downloads/linux-sudo-cheat-sheet