#  我经常问的问题

## ubuntu删除报错软件，点击dock栏上图标最小化

sudo apt purge apport

gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
## ubuntu下的steam无法输入中文
在 Ubuntu 下，Steam 可能无法输入中文，因为它是基于 QT 的库，而 QT 默认不支持中文输入法。可以尝试以下方法来解决这个问题：

1. 安装fcitx作为输入法：

```
sudo apt-get update
sudo apt-get installfcitx
sudo apt-get installfcitx-chinese-addons
```

2. 配置 fcitx：

打开终端，输入以下命令：

```
sudo nano /etc/xinput.conf
```

在文件中添加以下内容（注意，不要删除原有内容）：

```
options for "/dev/input/event0"
{
    libinput Emulate3ButtonMouse=1
    libinput EmulateDoubleClick=1
    libinput Accessibility=1
    libinput RepeatDelay=200
    libinput RepeatRate=60
}
```

3. 重启系统：

```
sudo reboot
```

4. 重新启动 Steam 并测试输入法：

```
steam
```

希望以上方法能帮助你解决这个问题。

## linux的软件卸载后重新安装，之前设置的为什么艘没变

 在 Linux 中，如果您卸载一个软件包并重新安装它，通常会保留之前设置的配置。这是因为软件包通常会安装一个名为“配置脚本”的工具，该工具负责在安装过程中设置软件的配置文件。

当您重新安装软件包时，配置脚本会自动检测软件包是否已经安装过，并从上次安装中恢复配置。因此，如果您之前已经配置了软件，那么在重新安装软件包后，之前的配置将被保留。

然而，请注意，如果您在重新安装软件包之前对配置文件进行了更改，这些更改可能会被覆盖。例如，如果您在卸载软件包后手动编辑了配置文件，这些更改可能会在重新安装软件包时被丢失。

如果您希望在重新安装软件包后保留之前的配置，可以考虑使用软件包的“备份”功能。大多数 Linux 发行版都允许您在卸载软件包之前备份配置文件。要备份配置文件，请在卸载软件包之前运行以下命令：

```
sudo dpkg-reconfigure -p <软件包名称>
```

请注意，这将在重新安装软件包之前提示您选择之前设置的配置文件。选择它们后，配置文件将被备份到您的家目录。然后，您可以使用这些备份文件在重新安装软件包后恢复之前的配置。

## 那我不想保留删除前的配置怎么办

 如果您不想保留删除前软件的配置，可以考虑使用以下方法：

1. 卸载软件包：

   使用 `dpkg` 卸载软件包的命令如下：

   ```
   sudo dpkg -r <软件包名称>
   ```

   请将 `<软件包名称>` 替换为您要卸载的软件包名称。

2. 安装软件包：

   使用 `dpkg` 安装软件包的命令如下：

   ```
   sudo dpkg -i <软件包文件>
   ```

   请将 `<软件包文件>` 替换为您要安装的软件包的文件名，通常以 `.deb` 结尾。

请注意，这种方法将删除软件包删除之前的配置，并从头开始设置新的配置。如果您之前对配置文件进行了更改，这些更改将丢失。

如果您需要保留删除前软件的配置，可以考虑使用其他方法，例如使用软件包的“备份”功能或手动备份配置文件。

==smy@smy-NB50TJ1-TK1:~$ sudo dpkg -r ardour
dpkg: 警告: 忽略卸载 ardour 的要求，系统中仅存在其配置文件；
可使用 --purge 来把这些配置文件一并删除
smy@smy-NB50TJ1-TK1:~$ sudo dpkg --purge ardour
(正在读取数据库 ... 系统当前共安装有 305949 个文件和目录。)
正在清除 ardour (1:6.9.0+ds0-1build1) 的配置文件 ...
smy@smy-NB50TJ1-TK1:~$ sudo dpkg -r ardour
dpkg: 警告: 忽略删除 ardour 的要求，系统没有安装该软件包==

## ubuntu如何查看依赖关系

在Ubuntu系统中，要查看依赖关系，您可以使用apt工具。以下是查看依赖关系的步骤：

1. 打开终端（快捷键Ctrl+Alt+T）。

2. 输入以下命令来查看已安装的软件包及其依赖关系：

  ```
  sudo apt-cache depends <软件包名称>
  ```

  将`<软件包名称>`替换为您要查询的软件包的名称。例如，如果要查看`nginx`软件包的依赖关系，请输入以下命令：

  ```
  sudo apt-cache depends nginx
  ```

3. 等待命令执行完成，系统将显示该软件包及其所有依赖关系。

如果您想查看系统中所有软件包的依赖关系，可以使用以下命令：

```
sudo apt-cache dumpdep -a
```

这将显示系统中所有软件包及其依赖关系。请注意，这个命令可能需要一段时间才能完成。

此外，您还可以使用`apt-rdepends`命令来查看软件包的逆依赖关系。逆依赖关系是指哪些软件包依赖于当前软件包。例如，如果要查看`nginx`软件包的逆依赖关系，请输入以下命令：

```
sudo apt-rdepends nginx
```

这将显示所有依赖`nginx`软件包的软件包及其逆依赖关系。

## auto什么意思

"auto"在计算机科学中通常表示自动、自动处理或自动执行。

例如，在操作系统中，当您执行某些操作时，系统可能自动执行一些操作来自动处理数据或自动完成任务。这通常是由操作系统或应用程序自动处理的。

在编程中，"auto"通常用于表示自动生成代码或自动处理数据。例如，在C++中，可以使用"auto"关键字来简化代码，使其更易于阅读和编写。

在许多编程语言中，都有类似"auto"的功能，用于自动推定变量类型或自动处理数据。

因此，"auto"通常表示自动处理或自动执行某些操作。

## purge什么意思

"purge"在计算机科学中通常表示清除、删除或完全删除。

在操作系统中，当您使用`apt`等软件包管理器安装软件时，有时会安装一些不必要的依赖项或遗留软件包。这些软件包可能不会影响程序的正常运行，但可能会占用磁盘空间并占用系统资源。

在这种情况下，您可以使用`apt purge`命令来完全删除这些不必要的软件包。`apt purge`命令将删除软件包及其所有依赖项，而不仅仅是已安装的软件包。

在编程中，"purge"通常用于表示清除或删除某些数据或对象。例如，在C++中，可以使用`purge`函数来清除对象的数据和指针。

因此，"purge"通常表示清除或删除某些数据或对象。

## auto , purge , remove的区别

sudo apt autopurge cpod
正在读取软件包列表... 完成
正在分析软件包的依赖关系树... 完成
正在读取状态信息... 完成                 
下列软件包将被【卸载】：
  cpod* gconf2* libappindicator1*
升级了 0 个软件包，新安装了 0 个软件包，要卸载 3 个软件包，有 5 个软件包未被升级。
解压缩后将会空出 158 MB 的空间。
您希望继续执行吗？ [Y/n] n
中止。

## 我的pip为什么把requests下到python3中

如果你使用`pip`命令安装的Python库是安装到Python 3版本中，那么它会被安装在Python 3的`site-packages`目录中。

默认情况下，`pip`会安装在系统默认的Python版本中。如果你想要强制使用Python 3版本来安装库，可以在`pip`命令中指定Python版本，例如：
```
pip3 install requests
```
这将强制使用Python 3版本来安装`requests`库。

如果你想要在Python 3中安装库，但希望它也能够在Python 2中使用，那么可以在安装时指定`--user`选项，例如：
```
pip3 install --user requests
```
这将把`requests`库安装到当前用户的`site-packages`目录中，这样它也可以在Python 2中使用。

总的来说，如果你想要在Python 3中安装库，那么使用`pip3`命令会更合适。如果你希望在Python 2和Python 3中都使用库，那么可以使用`--user`选项来安装它。

## `vscode`如何设置pip ？

在 Visual Studio Code 中设置 pip 需要进行以下步骤:

1. 打开 Visual Studio Code。
2. 点击左侧边栏的扩展图标,搜索 "Python" 并安装 "Python" 扩展。
3. 安装完成后,打开一个新的 Python 文件,或者在已有的 Python 项目中打开终端。
4. 在终端中输入以下命令来安装 pip:

  对于 Windows 用户:

插入复制

```
python -m ensurepip --default-pip
```

  对于 `macOS` 和 Linux 用户:

插入复制

```
python3 -m ensurepip --default-pip
```

  这将确保你的系统上安装了 pip。

1. 安装完成后,你可以在终端中使用 pip 命令来安装 Python 库。例如,要安装名为 "requests" 的库,可以在终端中输入以下命令:

插入复制

```
pip install requests
```

  这将安装 "requests" 库并将其添加到你的 Python 项目中。

注意:在某些系统上,可能需要使用 `python3` 代替 `python` 来安装 pip。

---

## windows上`vscode`识别不到我下的pip ?

如果你在 Windows 上使用 Visual Studio Code 并在其终端中无法识别已安装的 pip,可以尝试以下步骤来解决问题:

1. 确保你已经安装了 Python,并且 Visual Studio Code 能够找到 Python 的二进制文件。通常,这些文件位于 `C:\PythonXX\python.exe` 其中 `XX` 是 Python 的版本号。
2. 确保你已经安装了 pip,并且 Visual Studio Code 能够找到 pip 的二进制文件。通常,这些文件位于 `C:\PythonXX\Scripts\pip.exe` 其中 `XX` 是 Python 的版本号。
3. 打开 Visual Studio Code 的设置,搜索 "`python.pythonPath`",并将其设置为 Python 二进制文件的路径。例如,如果 Python 二进制文件位于 `C:\Python39\python.exe`,则将该设置设置为 `"C:\\Python39\\python.exe"`。
4. 打开 Visual Studio Code 的设置,搜索 "`python.pythonInterpreter`",并将其设置为 "Path"。这将使用系统默认的 Python 二进制文件。
5. 打开 Visual Studio Code 的设置,搜索 "`python.pipPath`",并将其设置为 pip 二进制文件的路径。例如,如果 pip 二进制文件位于 `C:\Python39\Scripts\pip.exe`,则将该设置设置为 `"C:\\Python39\\Scripts\\pip.exe"`。
6. 打开 Visual Studio Code 的设置,搜索 "`python.linting.pylintPath`",并将其设置为 `"C:\\Python39\\Scripts\\pylint.exe"`。
7. 重新启动 Visual Studio Code,然后尝试在终端中使用 pip 命令安装 Python 库。

如果这些步骤不能解决问题,请检查你的 Python 和 pip 安装是否正确,并确保 Visual Studio Code 能够找到它们。

## 如何把添加的dns保存到网络配置文件中（访问github方法）

您可以将添加的 DNS 服务器配置保存到网络配置文件中，以便在重启电脑后仍然可以使用该 DNS 服务器。以下是一些常见操作系统中保存 DNS 服务器配置的方法：

1. Windows 操作系统：

在 Windows 系统中，您可以将 DNS 服务器配置保存到“Internet 选项”对话框中。具体步骤如下：

a. 打开“Internet 选项”对话框：

按下 Windows + X 组合键，然后选择“Internet 选项”。

b. 转到“高级”选项卡：

在“Internet 选项”对话框中，点击“高级”选项卡。

c. 修改 DNS 服务器：

在“DNS 服务器”区域中，点击“自定义”按钮，然后添加新的 DNS 服务器地址。

d. 保存设置：

点击“确定”按钮，将新的 DNS 服务器配置保存到网络配置文件中。

2. ==Linux 操作系统==：

在 Linux 系统中，您可以将 DNS 服务器配置保存到“/etc/resolv.conf”文件中。具体步骤如下：

a. 打开“/etc/resolv.conf”文件：

使用文本编辑器打开“/etc/resolv.conf”文件，例如使用“nano”或“vi”。

b. 添加新的 DNS 服务器：

在文件末尾添加新的 DNS 服务器地址，每行一个。

c. 保存并关闭文件：

保存更改并关闭文件。

3. macOS 操作系统：

在 macOS 系统中，您可以将 DNS 服务器配置保存到“System Preferences”对话框中。具体步骤如下：

a. 打开“System Preferences”对话框：

按下 Command + Space 组合键，然后选择“System Preferences”。

b. 转到“Network”选项卡：

在“System Preferences”对话框中，点击“Network”选项卡。

c. 修改 DNS 服务器：

在“Network”选项卡中，点击“Edit”按钮，然后选择“DNS”区域。

d. 添加新的 DNS 服务器：

在“DNS”区域中，点击“+”按钮，然后输入新的 DNS 服务器地址。

e. 保存设置：

点击“Save”按钮，将新的 DNS 服务器配置保存到网络配置文件中。

请注意，这些步骤可能因操作系统和网络管理软件而异。如果您无法保存新的 DNS 服务器配置，请检查您的网络配置文件或网络管理软件的设置，以确保它们能够正确识别和保存新的 DNS 服务器配置。

## linux添加dns后还需要做些什么？如何确认已经添加了dns

添加 DNS 后，您需要重启系统以确保新的 DNS 服务器配置生效。在 Linux 系统中，您可以使用以下命令来重启系统：

```
sudo reboot
```

或者，您可以使用以下命令来手动重启网络服务：

```
sudo service networking restart
```

如果您使用的是不同的操作系统，请查阅该操作系统的文档以获取正确的重启命令。

在重启系统后，您可以使用以下命令来确认新的 DNS 服务器配置是否已生效：

1. 查看当前的 DNS 服务器地址：

```
cat /etc/resolv.conf
```

这将显示当前系统的 DNS 服务器地址。

2. 检查网络连接是否正常：

您可以使用 ping 命令来测试网络连接是否正常。例如，如果您使用的是 Google 的 DNS 服务器，您可以使用以下命令来测试连接：

```
ping 8.8.8.8
```

如果连接正常，您将看到类似以下的输出：

```
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=128 time=12.1 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=128 time=12.1 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=128 time=12.1 ms
```

这表明您的计算机可以成功地连接到 Google 的 DNS 服务器。

通过以上步骤，您可以确认新的 DNS 服务器配置是否已生效。

## dns的注意事项

### dns的作用

DNS（Domain Name System，域名系统）是用于将域名映射到 IP 地址的网络服务。它使得用户可以通过域名而不是 IP 地址来访问互联网上的网站和设备。

DNS 的主要作用包括以下几点：

1. 将域名映射到 IP 地址：DNS 服务器负责将域名（例如 "example.com"）映射到相应的 IP 地址（例如 "192.168.0.1"）。这样，当用户在浏览器中输入域名时，DNS 服务器会将其转换为 IP 地址，以便计算机可以找到正确的网络设备。

2. 解析 IP 地址：DNS 服务器负责将 IP 地址解析为相应的域名。当计算机需要将数据发送到某个 IP 地址时，DNS 服务器会将其解析为相应的域名，以便数据可以正确路由到正确的目的地。

3. 缓存查询结果：DNS 服务器会将常见的 DNS 查询结果进行缓存，以提高查询效率。这样，当多个用户同时访问同一个域名时，DNS 服务器可以直接从缓存中返回结果，而不是重新查询一遍。

4. 防止域名解析障碍：DNS 服务器可以防止因网络故障或 DNS 服务器故障而导致的域名解析障碍。当 DNS 服务器无法解析域名时，它会将查询转发给其他 DNS 服务器，以确保域名可以正确解析。

总之，DNS 的主要作用是将域名映射到 IP 地址，解析 IP 地址为域名，缓存查询结果，以及防止域名解析障碍。这使得用户可以通过域名访问互联网上的资源，而不仅仅是通过 IP 地址。

### 本地dns和其他dns的区别

本地 DNS（Domain Name System，域名系统）和其他 DNS 之间的区别主要体现在以下几个方面：

1. 服务范围不同：本地 DNS 通常只提供本地域名的解析服务，而其他 DNS 则提供全局域名解析服务，即解析所有域名的请求。

2. 解析结果不同：本地 DNS 只解析本地域名的请求，并将结果返回给本地设备。而其他 DNS 则将所有域名的请求都解析为 IP 地址，并将结果返回给请求方。

3. 优先级不同：本地 DNS 的优先级通常高于其他 DNS，因为本地 DNS 更接近用户，可以更快地响应用户的请求。

4. 功能不同：本地 DNS 通常只提供域名解析服务，而其他 DNS 则提供了更多的功能，例如反向 DNS 查询、DNS 缓存、DNS 安全等。

总的来说，本地 DNS 和其他 DNS 的区别在于它们的服务范围、解析结果、优先级和功能等方面。本地 DNS 更适合本地设备使用，而其他 DNS 则更适合全局域名解析。

### 主义我们在添加其他dns时是添加某域名的dns的ip

例如谷歌dns的ip是：8.8.8.8 和8.8.4.4

因为ip不是固定的，他会定时的更新ip所以要注意！

还有就是dns的ip个域名的ip不是同一个ip！

---

## python

### python中的关键字

'False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield'

### 1. python的列表

#### 1.1 Python列表的属性

##### 		1.1.1 Python列表的属性包括：

1. `__init__`：构造函数，用于初始化列表。
2. `__class__`：返回列表所属的类。
3. `__cmp__`：比较两个列表是否相等。
4. `__contains__`：检查元素是否存在于列表中。
5. `__delattr__`：删除属性。
6. `__delitem__`：删除指定索引的元素。
7. `__delslice__`：删除指定范围的元素。
8. `__eq__`：比较两个列表是否相等。
9. `__format__`：格式化列表。
10. `__ge__`：比较两个列表的大小。
11. `__getattribute__`：获取属性。
12. `__getitem__`：获取指定索引的元素。
13. `__getslice__`：获取指定范围的元素。
14. `__gt__`：比较两个列表的大小。
15. `__hash__`：返回列表的哈希值。
16. `__iadd__`：使用+运算符向列表添加元素。
17. `__imul__`：使用*运算符向列表添加元素。
18. `__init__`：构造函数，用于初始化列表。
19. `__iter__`：返回列表的迭代器。
20. `__le__`：比较两个列表的大小。
21. `__len__`：返回列表的长度。
22. `__lt__`：比较两个列表的大小。
23. `__mul__`：使用*运算符向列表添加元素。
24. `__ne__`：比较两个列表是否不相等。
25. `__new__`：创建列表对象。
26. `__reduce__`：返回列表的序列化表示。
27. `__reduce_ex__`：返回列表的序列化表示，包括自定义的显式属性。
28. `__repr__`：返回列表的表示。
29. `__rmul__`：使用*运算符向列表添加元素。
30. `__setattr__`：设置属性。
31. `__setitem__`：设置指定索引的元素。
32. `__setslice__`：设置指定范围的元素。
33. `__sizeof__`：返回列表的大小。
34. `__str__`：返回列表的表示。
35. `append`：向列表末尾添加一个元素。
36. `clear`：清空列表。
37. `copy`：返回列表的副本。
38. `count`：返回指定元素在列表中出现的次数。
39. `extend`：向列表末尾添加一个可迭代对象中的所有元素。
40. `index`：返回指定元素在列表中的索引。
41. `insert`：向列表中插入一个元素。
42. `pop`：删除并返回指定索引处的元素。
43. `remove`：删除指定元素在列表中首次出现的位置。
44. `reverse`：将列表中的元素反转。
45. `sort`：对列表进行排序。

这些属性是Python列表对象所具有的常用属性和方法，可以帮助你更详细地了解和操作列表。

##### 	1.1.2**带下划线的属性和不带下划线的属性的区别**

在Python中，带有下划线的属性和不带下划线的属性通常具有以下区别：

1. 带有下划线的属性：这些属性通常是类的私有属性，用于存储类的内部状态。它们通常不建议直接访问，而是通过公共方法暴露给外部。带有下划线的属性通常由类定义，而不是实例化对象。例如，`__name__`属性是一个类的私有属性，用于存储类的名称。

2. 不带下划线的属性：这些属性是类的公共属性，用于存储类的实例状态。它们可以通过实例化对象来访问，也可以通过公共方法暴露给外部。例如，`name`属性是一个类的公共属性，用于存储对象的名称。

需要注意的是，虽然带有下划线的属性通常是私有的，但仍然可以被外部访问。这是因为Python的类定义具有特殊的属性，可以访问和修改实例的属性。但是，不带下划线的属性通常不应该被外部访问，因为它们通常不建议直接访问。

总的来说，带有下划线的属性是类的私有属性，主要用于类的内部状态管理，而不带下划线的属性是类的公共属性，用于类的实例状态管理。

### 2.python循环语句

#### 2.1 python的循环语句都有什么？之间的区别是什么 ?

Python 的循环语句主要包括 for 循环和 while 循环两种。

1. for 循环:for 循环用于遍历序列(如列表、元组、字符串等)中的元素。它通常与 in 关键字一起使用,其中序列放在方括号中,而变量放在冒号后面。当序列中的元素被遍历完时,for 循环将自动退出。

例如,以下代码将遍历列表中的每个元素并打印它们:

插入复制

```
fruits = ['apple', 'banana', 'orange']
for fruit in fruits:
   print(fruit)
```

输出:

插入复制

```
apple
banana
orange
```

1. while 循环:while 循环用于在满足某个条件时反复执行一段代码。只要条件为真,while 循环就会一直执行,直到条件变为假为止。

例如,以下代码将使用一个初始值为 1 的变量 i,并在 while 循环中不断将其乘以 2,直到 i 大于 20 为止:

插入复制

```
i = 1
while i <= 20:
   print(i * 2)
   i += 1
```

输出:

插入复制

```
2
4
6
8
10
12
14
16
18
20
22
24
26
28
30
32
34
36
38
40
42
44
46
48
50
```

for 循环和 while 循环之间的主要区别在于,for 循环更适用于遍历序列中的元素,而 while 循环则更适合在满足某个条件时反复执行一段代码。

---

###  3. python的爬虫

#### 3.1 如何知道爬一个网页犯不犯法 ？

爬取网页数据时，需要注意是否侵犯他人的权利，通常需要遵守以下几个原则：

1. 尊重网站的robots.txt规则：robots.txt文件是网站管理员设置的爬虫访问规则，如果网站没有设置robots.txt，则默认允许所有爬虫访问。如果网站禁止爬虫访问，则会拒绝爬取请求。

   > robots.txt 通常位于网站的根目录下,即在网站的域名后面加上 /robots.txt 即可访问该文件。robots.txt 是网站管理员为搜索引擎设置的爬虫访问规则,可以指定搜索引擎爬虫可以抓取的网页范围,以及爬虫在抓取网页时需要遵循的规则。
   >
   > robots.txt 文件中通常包括以下内容:
   >
   > 1. ==User-agent: 指定爬虫的名称或标识符==,例如:
   >
   > 插入复制
   >
   > ```
   > User-agent: *
   > ```
   >
   > 表示该规则适用于所有爬虫。
   >
   > 1. ==Disallow: 指定爬虫不能抓取的网页路径==,例如:
   >
   > 插入复制
   >
   > ```
   > Disallow: /admin
   > ```
   >
   > 表示爬虫不能抓取 /admin 目录下的网页。
   >
   > 1. ==Allow: 指定爬虫可以抓取的网页路径==,例如:
   >
   > 插入复制
   >
   > ```
   > Allow: /news
   > ```
   >
   > 表示爬虫可以抓取 /news 目录下的网页。
   >
   > robots.txt 文件中的规则是可选的,即网站管理员可以不设置该文件,或者只设置其中的一部分规则。如果网站管理员没有设置 robots.txt 文件,则默认规则为 allow。

2. 遵循爬取策略：在进行爬取操作时，应遵循一定的爬取策略，如爬取速度、爬取范围等，避免对目标网站造成过度的负担或影响正常运行。

3. 保护网站安全：爬虫操作可能会对目标网站造成威胁，如SQL注入、跨站脚本攻击等，应采取适当的防护措施，保护网站的安全。

4. 保护用户隐私：爬取用户隐私信息可能会侵犯用户的权利，应遵循相关法律法规，保护用户隐私。

在爬取网页数据时，应遵循一定的道德规范，避免侵犯他人的权利，尊重网站的权益。如果对爬取行为有疑虑，可以咨询律师或法规部门，以确定是否犯法。

#### 3.2 怎么控制python爬虫时的速度 ？

控制Python爬虫速度的方法有很多，其中比较常用的有以下几种：

1. time模块：使用time模块可以实现对爬虫运行速度的控制。在爬取网页时，可以设置一个时间间隔，每次爬取网页后，暂停相应的时间，然后再进行下一次爬取。

示例代码：

```python
import time

# 爬取网页
response = requests.get(url)

# 暂停时间
time.sleep(1)
```

2. random模块：使用random模块可以生成随机时间，然后在每次爬取网页后，暂停相应的时间，这样可以实现爬虫速度的控制。

示例代码：

```python
import random

# 爬取网页
response = requests.get(url)

# 暂停时间
time.sleep(random.uniform(1, 3))
```

3. 设置爬取间隔：可以在爬取网页时，设置一个爬取间隔，每次爬取网页后，暂停相应的时间，这样可以实现爬虫速度的控制。

示例代码：

```python
# 爬取网页
response = requests.get(url)

# 设置爬取间隔
time.sleep(1)
```

4. 使用多线程：使用多线程可以实现对爬虫速度的控制，每个线程可以并行爬取多个网页，从而提高爬取速度。

示例代码：

```python
import threading

# 创建线程
def crawl(url):
   response = requests.get(url)

# 创建线程池
threads = []
for i in range(5):
   t = threading.Thread(target=crawl, args=(url,))
   threads.append(t)
   t.start()

# 等待所有线程完成
for t in threads:
   t.join()
```

以上方法可以根据具体需求进行选择和组合，实现对Python爬虫速度的控制。

#### 3.3 python中爬虫用到的所有模块 ？

Python中爬虫用到的模块有很多，其中比较常用的有以下几种：

1. requests：用于发送HTTP请求，获取网页内容。
2. BeautifulSoup：用于解析网页内容，提取所需信息。
3. lxml：与BeautifulSoup功能相似，但更强大，支持XPath和CSS选择器。
4. selenium：用于模拟用户操作浏览器，爬取一些需要登录或需要交互的网页。
5. urllib：与requests功能相似，但主要用于处理URL。
6. re：用于正则表达式匹配。
7. time：用于控制爬虫的运行速度。
8. random：用于生成随机数。
9. json：用于处理JSON数据。
10. os：用于处理操作系统相关操作。

这些模块可以组合使用，以实现各种爬虫任务。当然，根据具体需求，可能还需要使用其他模块或库。

#### 3.4 爬到图片，视频，音频，文字不同的保存方法有哪些 ？

爬取到图片、视频、音频、文字等不同类型的数据时,可以使用不同的方法进行保存。下面是一些常用的方法:

1. 图片

可以使用 Python 的 `requests` 库下载图片,并将其保存到本地文件中。例如:

```python
import requests

url = 'https://example.com/image.jpg'
response = requests.get(url)
with open('image.jpg', 'wb') as f:
  f.write(response.content)
```

2. 视频

可以使用 Python 的 `moviepy` 库将视频保存到本地文件中。例如:

```python
from moviepy.editor import *

url = 'https://example.com/video.mp4'
clip = VideoFileClip(url)
clip.write_videofile('video.mp4')
```

3. 音频

可以使用 Python 的 `moviepy` 库将音频保存到本地文件中。例如:

```python
from moviepy.editor import *

url = 'https://example.com/audio.mp3'
clip = AudioFileClip(url)
clip.write_audiofile('audio.mp3')
```

4. 文字

可以使用 Python 的 `requests` 库下载网页内容,并将其保存到本地文件中。例如:

```python
import requests

url = 'https://example.com'
response = requests.get(url)
with open('text.html', 'w', encoding='utf-8') as f:
  f.write(response.text)
```

总之,根据需要选择合适的方法将爬取到的数据保存到本地文件中即可。

---

## 类

### 一、我可不可以认为python中的类是一个生产具有相同属性或不同属性，同一类变量的工具？

是的，你可以说 Python 中的类是一个生产具有相同属性或不同属性，同一类变量的工具。

类是 Python 中一种抽象的数据类型，它定义了一组属性和方法，这些属性和方法可以被实例化对象所使用。类中的属性用于存储对象的状态信息，而方法用于实现对象的功能。

在 Python 中，你可以使用类来创建具有相同属性和方法的对象，也可以创建具有不同属性和方法的对象。例如，你可以创建一个名为 `Person` 的类，其中包含属性 `name` 和 `age`，以及方法 `say_hello` 和 `greet`。然后，你可以创建多个具有相同属性和方法的对象，这些对象可以共享相同的属性和方法，也可以具有不同的属性和方法。

此外，类还可以用于创建同一类变量的工具。例如，你可以使用类来创建一个名为 `Counter` 的类，其中包含一个名为 `count` 的类变量。然后，你可以创建多个具有相同类变量的对象，这些对象可以共享相同的计数状态。

总之，类是 Python 中一种抽象的数据类型，它用于创建具有相同属性和方法的对象，也可以用于创建同一类变量的工具。

### 二、我可以把python的一个集合或者一个字典，而对象的实例化则是数学中的组合？

当然可以。在Python中，你可以使用集合（set）或字典（dict）来表示数学中的组合概念。

集合中的元素是唯一的，所以你可以用集合来表示组合中的元素。例如，如果你有一个集合{1, 2, 3}，那么它的所有子集就是{1, 2, 3}本身，以及{1, 2}, {1, 3}, {2, 3}, 和{1}, {2}, {3}。

字典中的键和值可以是任何类型的对象，但通常情况下，字典的键是唯一的。所以你可以用字典来表示组合中的元素及其相应的权重。例如，如果你有一个字典{'A': 0.5, 'B': 0.3, 'C': 0.2}，那么它的所有子集就是{'A': 0.5, 'B': 0.3, 'C': 0.2}本身，以及{'A': 0.5, 'B': 0.3}, {'A': 0.5, 'C': 0.2}, {B': 0.3, 'C': 0.2}, 和{'A': 0.5}, {'B': 0.3}, {'C': 0.2}。

==请注意，字典的键和值可以是任何类型的对象，但通常情况下，字典的键是唯一的。这意味着如果你有两个字典，它们具有相同的键，那么它们的值将会被合并。如果值不相等，那么将会引发一个异常。==

### 三、类之间能不能嵌套？

是的，在Python中，类之间可以嵌套。嵌套类是指在一个类中定义另一个类。嵌套类通常用于表示类的继承关系。

下面是一个类之间的嵌套示例：

```python
class ParentClass:
  def __init__(self, name):
      self.name = name

  def say_hello(self):
      print(f"Hello, my name is {self.name}.")

class ChildClass(ParentClass):
  def __init__(self, name, age):
      super().__init__(name)
      self.age = age

  def say_hello(self):
      super().say_hello()
      print(f"I am {self.age} years old.")

# 创建一个ChildClass类的实例
my_object = ChildClass("Alice", 30)

# 调用实例的方法
my_object.say_hello()
```

在这个示例中，我们定义了一个名为`ParentClass`的类，它具有一个构造函数`__init__`，用于初始化类的实例。构造函数接受一个参数`name`，并将它存储在实例的`name`属性中。我们还定义了一个名为`say_hello`的方法，用于打印一条包含实例的`name`属性的消息。

接下来，我们定义了一个名为`ChildClass`的类，它继承了`ParentClass`。在`ChildClass`的构造函数中，我们使用`super()`关键字调用`ParentClass`的构造函数，并将`name`参数传递给它。我们还定义了一个名为`say_hello`的方法，它首先调用`ParentClass`的`say_hello`方法，然后打印一条包含实例的`age`属性的消息。

最后，我们创建了一个名为`my_object`的`ChildClass`类的实例，并传入`name`和`age`参数。然后，我们调用`say_hello`方法，该方法将首先打印一条包含`my_object`的`name`属性的消息，然后打印一条包含`my_object`的`age`属性的消息。



---

## Godot

### 一、gdscript的基础语法

GDScript 是一种用于创建 VR 游戏和 3D 应用程序的编程语言。以下是 GDScript 的基本语法：

1. 变量声明：在 GDScript 中，变量声明使用关键字 `var` 或 `const`。例如：
```
var x = 10
const y = 20
```
2. 数据类型：GDScript 支持多种数据类型，如 `int`、`float`、`bool`、`string`、`Vector2`、`Vector3`、`Quat`、`Color` 等。
3. 变量命名：在 GDScript 中，变量命名遵循 camelCase 命名法，即首字母小写，每个单词的首字母大写。
4. 控制结构：GDScript 支持条件语句（`if`）、循环语句（`for`、`while`、`do_while`）和跳转语句（`break`、`continue`、`return`）。
5. 函数定义：在 GDScript 中，函数定义使用关键字 `func`。例如：
```
func greet(name):
   print("Hello, " + name + "!")
```
6. 类定义：在 GDScript 中，类定义使用关键字 `class`。例如：
```
class Person:
   var name = "Unknown"
   var age = 0

   func say_hello():
       print("Hello, my name is " + name + " and I am " + str(age) + " years old.")
```
7. 继承：在 GDScript 中，类可以继承其他类。例如：
```
class Employee(Person):
   var salary = 0

   func say_hello():
       super().say_hello()
       print("I am an employee with a salary of " + str(salary) + ".")
```
这些是 GDScript 的基本语法。GDScript 还提供了许多其他功能，如数组、列表、字典、对象等。要了解更多信息，请查阅 GDScript 文档。

## 二、在GDScript中，代码通常可以划分为以下几个部分：

1. 类定义：GDScript中的类定义使用`class`关键字，后面跟类名，然后是类属性和方法。例如：
```
class Example:
   var a = 1
   var b = 2

   func _ready():
       print("Hello, world!")
```
2. 变量声明：GDScript中的变量声明使用`var`关键字，后面跟变量名和变量类型。例如：
```
var x = 1
var y = "hello"
```
3. 函数定义：GDScript中的函数定义使用`func`关键字，后面跟函数名，然后是函数参数列表和函数体。例如：
```
func add(a, b):
   return a + b
```
4. 类方法：类方法是在类中定义的函数，使用`func`关键字，后面跟方法名，然后是方法参数列表和函数体。例如：
```
class Example:
   func _ready():
       print("Hello, world!")
```
5. 控制结构：GDScript中的控制结构包括条件语句（如`if`、`elif`、`else`）、循环语句（如`for`、`while`）和跳转语句（如`return`、`break`、`continue`）。
6. 常量定义：GDScript中的常量定义使用`const`关键字，后面跟常量名和常量值。例如：
```
const PI = 3.14159
const TRUE = true
```
7. 注释：GDScript中的注释使用`#`符号，后面跟注释内容。例如：
```
# This is a comment
```

这些部分是GDScript代码的基本组成部分，通过组合这些部分，可以编写出完整的GDScript程序。



---

## 矢量图

### 一、什么是向量图片(矢量图的原理)

矢量图的原理是基于数学公式和坐标来描述图像的。与使用像素矩阵表示的图像不同，向量图使用的是几何图形和曲线，通过计算机绘图软件（如Adobe Illustrator、Sketch等）创建。

向量图中的每个像素都由一个数学公式和一个坐标来定义。这些公式和坐标描述了图像中每个像素的位置、颜色和形状。这些公式和坐标被存储在一个文件中，该文件可以被任何支持该格式的计算机绘图软件打开和编辑。

向量图的特点是图像质量高、颜色丰富、分辨率高，但同时，向量图的文件大小通常比同质量的图像文件大。由于使用的是数学公式和坐标，向量图可以轻松地缩放、旋转、扭曲等，而不会影响图像的质量。

向量图通常用于需要高度精度、复杂图形和排版任务的场景，例如：标志设计、广告设计、排版等。

### 二、矢量图的数学原理

矢量图的数学原理是基于数学公式和坐标来描述图像的。在矢量图中，每个像素由一个数学公式和一个坐标来定义。这些公式和坐标描述了图像中每个像素的位置、颜色和形状。

例如，对于一个矩形，我们可以使用以下数学公式和坐标来描述它的形状：

x = 0, y = 0
x = 100, y = 0
x = 100, y = 100
x = 0, y = 100

这些公式和坐标定义了矩形的四个顶点，然后我们可以使用计算机绘图软件将这些公式和坐标转换为实际的图形。

同样地，对于每个像素，我们也可以使用类似的数学公式和坐标来描述它的位置、颜色和形状。这些公式和坐标被存储在一个文件中，该文件可以被任何支持该格式的计算机绘图软件打开和编辑。

矢量图的优点之一是其灵活性，因为使用的是数学公式和坐标，所以可以轻松地缩放、旋转、扭曲等，而不会影响图像的质量。此外，矢量图的文件大小通常比同质量的图像文件大，因为需要存储更多的数学公式和坐标。

## 属性/方法

### 1. 共有属性是所有的bin的命令的使用吗？ :happy: 我在这里指的是bin文件夹里的指令，ai把bin理解成bin()函数是！

不是，共有属性通常指的是类的公共属性，即不需要通过实例化对象就可以访问到的属性。这些属性通常由类定义，而不是由实例化对象来定义。

在Python中，类定义的属性可以是公共的（不需要实例化对象就可以访问的）或私有的（需要实例化对象才能访问的）。带有下划线的属性通常是私有的，不带下划线的属性通常是公共的。

例如，在下面的示例中，`__name__`属性是一个类的私有属性，而`name`属性是一个类的公共属性：
```python
class MyClass:
   def __init__(self, name):
       self.__name__ = name
       self.name = name
```
在这个例子中，`__name__`属性是一个类的私有属性，用于存储类的名称，而`name`属性是一个类的公共属性，用于存储对象的名称。

因此，如果要使用`bin`命令，你需要使用Python的内置函数`bin()`来将一个整数转换为二进制表示，而不是直接访问`bin`命令的属性。

### 2. python是否一切是类

Python是一种面向对象编程语言，这意味着在Python中，一切都可以看做是一个类。但是，Python中有一些特殊的对象，如整数、浮点数、字符串等，它们不是类，而是Python的内置类型。

在Python中，类是一种抽象的数据类型，用于创建对象。类定义了一组属性和方法，对象就是类的实例，表示类的一个具体化。在Python中，你可以使用`class`关键字来定义一个类，然后使用`()`运算符来创建类的实例。

例如，下面是一个简单的类定义：
```python
class MyClass:
  def __init__(self, name):
      self.name = name

  def say_hello(self):
      print("Hello, my name is", self.name)
```
在这个例子中，`MyClass`是一个类，它有两个方法：`__init__`用于初始化类的实例，`say_hello`用于打印一条问候消息。

然后，你可以使用`MyClass`类创建一个对象，并调用它的方法：
```python
my_object = MyClass("John")
my_object.say_hello()  # 输出：Hello, my name is John
```
在这个例子中，`my_object`是一个`MyClass`类的实例，它具有`name`属性，并可以调用`say_hello`方法。

因此，虽然Python中的一切都可以看做是一个类，但有一些特殊的对象，如整数、浮点数、字符串等，它们不是类，而是Python的内置类型。

### 3. linux中命令点后的是属性还是方法？

在Linux系统中，命令行后跟的符号是属性（attribute）或方法（method）取决于所指的对象。

属性（attribute）是对象的一个组成部分，它描述了对象的状态或特征。属性可以通过`.`符号访问。例如，如果你有一个名为`my_object`的对象，并且它有一个名为`attribute`的属性，则可以使用`my_object.attribute`来访问该属性。

方法（method）是对象的一个操作，它执行某种操作并返回一个值。方法可以通过`.`符号访问，并且需要使用小括号来调用。例如，如果你有一个名为`my_object`的对象，并且它有一个名为`method`的方法，则可以使用`my_object.method()`来调用该方法。

因此，在Linux系统中，命令行后跟的符号是属性还是方法取决于所指的对象。如果所指的对象是一个属性，则使用`.`符号访问；如果所指的对象是一个方法，则使用`.`符号访问并使用小括号调用。

## linux

### 1. linux中which是查找什么的？

`which`命令用于查找系统中某个命令的路径。它会在系统路径（通常存储在`/usr/bin`、`/usr/sbin`、`/usr/local/bin`等目录中）中搜索指定的命令，并返回第一个找到的命令的路径。

例如，如果你在终端中输入以下命令：
```
which ls
```
`which`命令会搜索系统中所有的`ls`命令，并返回第一个找到的`ls`命令的路径，例如：
```
/bin/ls
```
这表示`ls`命令已经安装在你的系统上，并且它的路径是`/bin/ls`。

总的来说，`which`命令可以帮助你查找系统中某个命令的路径，这对于使用命令行工具或编写脚本时非常有用。

### 2. linux中除了which还有其他类似的命令吗

是的，除了`which`命令之外，还有其他类似的命令可以帮助你查找系统中某个命令的路径。

`whereis`命令与`which`命令类似，但它会返回命令的多个路径，而不是只返回一个。例如，如果你使用`whereis`命令搜索`ls`命令，它可能会返回：
```
/bin/ls /usr/bin/ls
```
这表示`ls`命令有两个路径，分别是`/bin/ls`和`/usr/bin/ls`。

`locate`命令也是类似的，但它会搜索系统中的所有文件，而不是只搜索命令。例如，如果你使用`locate`命令搜索包含`python`的文件，它可能会返回：
```
/usr/bin/python
/usr/bin/python2
/usr/bin/python3
```
这表示系统中有三个名为`python`的可执行文件。

总的来说，这些命令都可以帮助你查找系统中某个命令的路径，但它们的使用方式和返回值可能会有所不同。

### 3. 可不可以举几个查找全局内的文件夹的例子

当然可以。以下是一些查找全局内文件夹的例子：

1. 如果你想要查找系统中名为`example_folder`的文件夹，可以使用以下命令：
```
ls /path/to/folder
```
这将会在指定的路径中列出所有文件夹，包括名为`example_folder`的文件夹。
2. 如果你想要查找系统中所有名为`example_folder`的文件夹，可以使用以下命令：
```
find / -type d -name "example_folder"
```
这将会在系统中的所有路径中搜索名为`example_folder`的文件夹，并返回它们的路径。
3. 如果你想要查找系统中所有名为`example_folder`的文件夹，并且希望只显示它们的名称，可以使用以下命令：
```
find / -type d -name "example_folder" -printf '%f\n'
```
这将会在系统中的所有路径中搜索名为`example_folder`的文件夹，并只显示它们的名称。

总的来说，你可以使用`ls`命令来查找全局范围内的文件夹，也可以使用`find`命令来搜索系统中的所有路径，以查找名为特定文件夹名称的文件夹。

### 4. 为什么whereis requests搜不到

`whereis`命令主要用于查找可执行文件，而不是文件夹。因此，如果你使用`whereis`命令搜索文件夹名，它可能会返回空结果。

如果你想要搜索文件夹，可以尝试使用`find`命令或其他方法。例如，你可以使用`find`命令搜索当前目录及其子目录中的所有名为`example_folder`的文件夹，如下所示：
```
find . -type d -name "example_folder"
```
这将会在当前目录及其子目录中搜索名为`example_folder`的文件夹，并返回它们的路径。

总的来说，如果你使用`whereis`命令搜索文件夹名，它可能会返回空结果，因此建议使用其他方法来搜索文件夹。

### 5.ubuntu中卸载软件时，但我不知的包名怎么办

在Ubuntu中卸载软件时，如果您不确定包名，可以通过以下步骤来查找和卸载软件：

1. 打开终端。

2. 输入以下命令来列出所有已安装的软件包：
```
sudo apt list --installed
```

3. 输入您要查找的软件名称，例如：
```
sudo apt search 软件名称
```

4. 在搜索结果中，找到您要卸载的软件包。查看其详细信息，例如版本、发布器和大小等信息。

5. 如果您确定要卸载的软件包，输入以下命令来卸载软件包：
```
sudo apt-get remove 软件包名
```

6. 输入`y`确认卸载。

7. 如果您想完全删除软件包及其配置文件，可以使用以下命令：
```
sudo apt-get purge 软件包名
```

8. 输入`y`确认卸载。

请注意，卸载软件包可能会影响已安装的软件和系统配置。在卸载前，请务必备份重要文件和数据。

### 6.为什么apt list --installed没有星火商店的下载的软件

`apt list --installed`命令只显示已安装的软件包，不包括未安装的软件包。因此，如果您在星火商店中下载了软件但尚未安装，它不会显示在`apt list --installed`命令的输出中。

如果您想要查看星火商店中下载的所有软件包，可以使用`apt list`命令。它将显示所有可用的软件包，包括已安装的软件包、未安装的软件包和已卸载的软件包。

如果您已经安装了星火商店中的软件，但无法在`apt list --installed`命令的输出中看到它们，可能是因为软件包的名称或版本与`apt list --installed`命令中的名称或版本不匹配。在这种情况下，您可以使用`apt list`命令来查找正确的软件包名称或版本。

## steam++

### .sh转.desktop

要将 .sh 文件保存为 .desktop 文件，您可以按照以下步骤操作：

1. 打开终端并导航到脚本所在的目录。
2. 创建一个新的 .desktop 文件，例如：`Steam++.desktop`。
3. 使用文本编辑器将以下内容添加到 .desktop 文件中：

```
[Desktop Entry]
Version=1.0
Name=Steam++
Comment=A script to run Steam++
Exec=/path/to/your/Steam++.sh
Icon=/path/to/your/icon.png
Terminal=false
Type=Application
```

请注意，您需要将 `/path/to/your/Steam++.sh` 替换为脚本的实际路径，将 `/path/to/your/icon.png` 替换为图标的实际路径。

4. 保存文件并关闭文本编辑器。
5. 现在，您可以双击 .desktop 文件来运行脚本。

注意：如果您使用的是 Steam 客户端，请确保您已经安装并运行了 Steam 服务。如果没有，请访问 Steam 官方网站下载并安装。

### .sh文件不能运行

sudo chmod +x ~~.sh

