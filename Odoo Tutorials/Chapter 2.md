# Chapter 2: Development environment setup
根据预期的用例，有多种方法可以安装Odoo。对于开发人员 Odoo社区和Odoo员工一样，首选方式是执行源码安装 （从源代码运行Odoo）。

## 准备环境
首先，按照贡献指南的“环境设置”部分进行准备 您的环境。

 >*重要*
 >> 以下步骤仅适用于Odoo员工。上述存储库不是 可供第三方访问。

到现在为止，您应该已将源代码下载到两个本地存储库中，一个用于，一个用于。这些存储库设置为将更改推送到预定义的共享 GitHub 上的分支。当您开始为代码库做出贡献时，这将被证明是方便的，但是 在本教程的范围内，我们希望避免通过训练污染共享存储库 材料。然后，让我们在第三个存储库中开发自己的模块。 与前两个存储库一样，它将成为引用所有存储库的一部分 包含Odoo模块的目录。odoo/odooodoo/enterprisetechnical-training-sandboxaddons-path

1. 按照与 和 存储库相同的过程，访问 github.com/odoo/technical-training-sandbox 并单击 Fork 按钮以 在您的帐户下创建存储库的分支。odoo/odooodoo/enterprise

2. 使用以下命令克隆计算机上的存储库：

 git clone git@github.com:odoo/technical-training-sandbox.git
3. 配置存储库以将更改推送到分支：

 cd technical-training-sandbox/
 git remote add dev git@github.com:<your_github_account>/technical-training-sandbox.git
 git remote set-url --push origin you_should_not_push_on_this_repository
就是这样！您的环境现在已准备好从源代码运行Odoo，并且您已经成功 创建了一个存储库作为插件目录。这将允许您将工作推送到 GitHub上。

现在，在存储库中进行一些小的更改，例如更新文件。然后，按照“首次贡献”部分进行操作 的贡献指南，将您的更改推送到 GitHub 并创建一个technical-training-sandboxREADME.md公关. 这将使您能够分享即将到来的工作并获得反馈。将说明调整为 使用分支和存储库。mastertechnical-training-sandbox

为确保持续的反馈循环，我们建议在到达新的提交后立即推送新的提交 里程碑，例如完成教程的一章。

 > 注解
    >> 存储库在文件系统上的具体位置并不重要。但是，对于 为简单起见，我们假设您已经克隆了同一存储库下的所有存储库 目录。如果不是这种情况，请确保相应地调整以下命令， 提供从存储库到存储库的相应相对路径。odoo/odooodoo/technical-training-sandbox

## Run the server
### Launch with odoo-bin
设置完所有依赖项后，可以通过运行命令行来启动Odoo 服务器的接口。odoo-bin
```
 cd $HOME/src/odoo/
 ./odoo-bin --addons-path="addons/,../enterprise/,../technical-training-sandbox" -d rd-demo
```
可以使用多个命令行参数来运行 服务器。在本培训中，您只需要其中的一些.

-d<database>
将要使用的数据库。

--addons-path<directories>
以逗号分隔的存储模块的目录列表。扫描这些目录 用于模块。

--limit-time-cpu<limit>
防止工作线程对每个请求使用超过 <limit> CPU 秒数。

--limit-time-real<limit>
防止工作人员处理请求的时间超过 <limit> 秒。

> *小技巧*
>> * 这--limit-time-cpu和--limit-time-real参数可用于防止 在调试源代码时，工作线程不会被杀死。
>> * 您可能会遇到类似于 的错误。在这种情况下，您可能需要重新安装模块AttributeError: module '<MODULE_NAME>' has no attribute '<$ATTRIBUTE'>$ 点 安装 --upgrade --force-reinstall <MODULE_NAME>.
如果多个模块发生此错误，您可能需要重新安装所有 要求 $ pip install --upgrade --force-reinstall -r requirements.txt.
You can also clear the python cache to solve the issue:
```
 cd $HOME/.local/lib/python3.8/site-packages/
 find -name '*.pyc' -type f -delete
```
其他常用的参数包括：

-i：在运行服务器之前安装一些模块 （逗号分隔列表）。

-u：在运行服务器之前更新一些模块 （逗号分隔列表）。

### Log in to Odoo
在浏览器上打开 http://localhost:8069/。我们建议使用 Chrome、Firefox 或 任何其他带有开发工具的浏览器。

要以管理员用户身份登录，请使用以下凭据：

电子邮件：admin

password: admin

## 启用开发人员模式
开发人员或调试模式对于训练很有用，因为它可以访问其他（高级） 工具。在接下来的章节中，我们将始终假设您已经启用了开发人员模式。

立即启用开发人员模式。选择您喜欢的方法;他们是 都是等效的。

 注解

只有在安装了至少一个应用程序的情况下，才能访问“设置”屏幕的主页。 在下一章中，您将被引导安装自己的应用程序。

额外工具
有用的 Git 命令
以下是一些对日常工作有用的 Git 命令。

切换分支：
切换分支时，两个存储库（odoo 和 enterprise）必须同步，即 两者都需要位于同一分支中。
 cd $HOME/src/odoo
 git switch 17.0

 cd $HOME/src/enterprise
 git switch 17.0
Fetch and rebase:

 cd $HOME/src/odoo
 git fetch --all --prune
 git rebase --autostash odoo/17.0

 cd $HOME/src/enterprise
 git fetch --all --prune
 git rebase --autostash enterprise/17.0
Code Editor
如果你在Odoo工作，你的许多同事都在使用VSCode、VSCodium（相当于开源）、PyCharm或Sublime Text。但是，您可以自由选择您喜欢的编辑器。

正确配置 linter 非常重要。使用 linter 通过显示语法和 语义警告或错误。Odoo源代码试图尊重Python和JavaScript的标准， 但其中一些可以忽略不计。

对于 Python，我们使用 PEP8 并忽略以下选项：

E501：线条太长

E301： 预计 1 个空行，找到 0 个

E302： 预期 2 个空行，找到 1 个

对于 JavaScript，我们使用 ESLint，您可以在此处找到配置文件示例。

Administrator tools for PostgreSQL
您可以使用前面演示的命令行来管理 PostgreSQL 数据库，或者使用 GUI 应用程序，例如 pgAdmin 或 DBeaver。

要将 GUI 应用程序连接到数据库，我们建议您使用 Unix 套接字进行连接。

Host name/address: /var/run/postgresql

Port: 5432

Username: $USER

Python Debugging
当遇到错误或试图理解代码是如何工作的时，简单地打印出来就可以 还有很长的路要走，但合适的调试器可以节省大量时间。

可以使用经典的 Python 库调试器（pdb、pudb 或 ipdb），也可以 使用编辑器的调试器。

在下面的示例中，我们使用 ipdb，但该过程与其他库类似。

安装库：

pip install ipdb
放置触发器（断点）：

import ipdb; ipdb.set_trace()
 Example

def copy(self, default=None):
    import ipdb; ipdb.set_trace()
    self.ensure_one()
    chosen_name = default.get('name') if default else ''
    new_name = chosen_name or _('%s (copy)') % self.name
    default = dict(default or {}, name=new_name)
    return super(Partner, self).copy(default)
下面是命令列表：

h(elp)[command]
如果未提供参数，则打印可用命令的列表。以命令为参数， 打印有关该命令的帮助。

ppexpression
的值是使用模块漂亮打印的。expressionpprint

w(here)
打印堆栈跟踪，底部有最新的帧。

d(own)
在堆栈跟踪中将当前帧向下移动一级（到较新的帧）。

u(p)
在堆栈跟踪中将当前帧向上移动一级（到较旧的帧）。

n(ext)
继续执行，直到到达当前函数中的下一行或返回。

c(ontinue)
继续执行，并且仅在遇到断点时停止。

s(tep)
执行当前行。在第一个可能的情况下停止（在以下功能中 调用或在当前函数的下一行）.

q(uit)
退出调试器。正在执行的程序已中止。

现在您的服务器正在运行，是时候开始编写您自己的应用程序了！