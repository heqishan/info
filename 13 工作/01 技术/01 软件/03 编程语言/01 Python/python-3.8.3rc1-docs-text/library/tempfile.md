"tempfile" --- 生成临时文件和目录
*********************************

**源代码：** Lib/tempfile.py

======================================================================

该模块用于创建临时文件和目录，它可以跨平台使用。"TemporaryFile"、
"NamedTemporaryFile"、"TemporaryDirectory" 和 "SpooledTemporaryFile"
是带有自动清理功能的高级接口，可用作上下文管理器。"mkstemp()" 和
"mkdtemp()" 是低级函数，使用完毕需手动清理。

所有由用户调用的函数和构造函数都带有参数，这些参数可以设置临时文件和临
时目录的路径和名称。该模块生成的文件名包括一串随机字符，在公共的临时目
录中，这些字符可以让创建文件更加安全。为了保持向后兼容性，参数的顺序有
些奇怪。所以为了代码清晰，建议使用关键字参数。

这个模块定义了以下内容供用户调用：

tempfile.TemporaryFile(mode='w+b', buffering=None, encoding=None, newline=None, suffix=None, prefix=None, dir=None, *, errors=None)

   返回一个 *file-like object* （文件类对象）作为临时存储区域。创建该
   文件使用了与 "mkstemp()" 相同的安全规则。它将在关闭后立即销毁（包括
   垃圾回收机制关闭该对象时）。在 Unix 下，该文件在目录中的条目根本不
   创建，或者创建文件后立即就被删除了，其他平台不支持此功能。您的代码
   不应依赖使用此功能创建的临时文件名称，因为它在文件系统中的名称可能
   是可见的，也可能是不可见的。

   生成的对象可以用作上下文管理器（参见 示例）。完成上下文或销毁临时文
   件对象后，临时文件将从文件系统中删除。

   *mode* 参数默认值为 "'w+b'"，所以创建的文件不用关闭，就可以读取或写
   入。因为用的是二进制模式，所以无论存的是什么数据，它在所有平台上都
   表现一致。*buffering*、*encoding*、*errors* 和 *newline* 的含义与
   "open()" 中的相同。

   参数 *dir*、*prefix* 和 *suffix* 的含义和默认值都与它们在
   "mkstemp()" 中的相同。

   在 POSIX 平台上，它返回的对象是真实的文件对象。在其他平台上，它是一
   个文件类对象 (file-like object)，它的 "file" 属性是底层的真实文件对
   象。

   如果可用，则使用 "os.O_TMPFILE" 标志（仅限于 Linux，需要 3.11 及更
   高版本的内核）。

   引发一个 "tempfile.mkstemp" 审计事件，附带参数 "fullpath"。

   在 3.5 版更改: 如果可用，现在用的是 "os.O_TMPFILE" 标志。

   在 3.8 版更改: 添加了 *errors* 参数。

tempfile.NamedTemporaryFile(mode='w+b', buffering=None, encoding=None, newline=None, suffix=None, prefix=None, dir=None, delete=True, *, errors=None)

   此函数执行的操作与 "TemporaryFile()" 完全相同，但确保了该临时文件在
   文件系统中具有可见的名称（在 Unix 上表现为目录条目不取消链接）。从
   返回的文件类对象的 "name" 属性中可以检索到文件名。在临时文件仍打开
   时，是否允许用文件名第二次打开文件，在各个平台上是不同的（在 Unix
   上可以，但在 Windows NT 或更高版本上不行）。如果 *delete* 为 true（
   默认值），则文件会在关闭后立即被删除。该函数返回的对象始终是文件类
   对象 (file-like object)，它的 "file" 属性是底层的真实文件对象。文件
   类对象可以像普通文件一样在 "with" 语句中使用。

   引发一个 "tempfile.mkstemp" 审计事件，附带参数 "fullpath"。

   在 3.8 版更改: 添加了 *errors* 参数。

tempfile.SpooledTemporaryFile(max_size=0, mode='w+b', buffering=None, encoding=None, newline=None, suffix=None, prefix=None, dir=None, *, errors=None)

   此函数执行的操作与 "TemporaryFile()" 完全相同，但会将数据缓存在内存
   中，直到文件大小超过 *max_size*，或调用文件的 "fileno()" 方法为止，
   此时数据会被写入磁盘，并且写入操作与 "TemporaryFile()" 相同。

   此函数生成的文件对象有一个额外的方法——"rollover()"，可以忽略文件大
   小，让文件立即写入磁盘。

   返回的对象是文件类对象 (file-like object)，它的 "_file" 属性是
   "io.BytesIO" 或 "io.TextIOWrapper" 对象（取决于指定的是二进制模式还
   是文本模式）或真实的文件对象（取决于是否已调用 "rollover()"）。文件
   类对象可以像普通文件一样在 "with" 语句中使用。

   在 3.3 版更改: 现在，文件的 truncate 方法可接受一个 "size" 参数。

   在 3.8 版更改: 添加了 *errors* 参数。

tempfile.TemporaryDirectory(suffix=None, prefix=None, dir=None)

   此函数会安全地创建一个临时目录，且使用与 "mkdtemp()" 相同的规则。此
   函数返回的对象可用作上下文管理器（参见 示例）。完成上下文或销毁临时
   目录对象后，新创建的临时目录及其所有内容将从文件系统中删除。

   可以从返回对象的 "name" 属性中找到临时目录的名称。当返回的对象用作
   上下文管理器时，这个 "name" 会作为 "with" 语句中 "as" 子句的目标（
   如果有 as 的话）。

   可以调用 "cleanup()" 方法来手动清理目录。

   引发一个 "tempfile.mkdtemp" 审计事件，附带参数 "fullpath"。

   3.2 新版功能.

tempfile.mkstemp(suffix=None, prefix=None, dir=None, text=False)

   以最安全的方式创建一个临时文件。假设所在平台正确实现了 "os.open()"
   的 "os.O_EXCL" 标志，则创建文件时不会有竞争的情况。该文件只能由创建
   者读写，如果所在平台用权限位来标记文件是否可执行，那么没有人有执行
   权。文件描述符不会过继给子进程。

   与 "TemporaryFile()" 不同，"mkstemp()" 用户用完临时文件后需要自行将
   其删除。

   如果 *suffix* 不是 "None" 则文件名将以该后缀结尾，是 "None" 则没有
   后缀。"mkstemp()" 不会在文件名和后缀之间加点，如果需要加一个点号，
   请将其放在 *suffix* 的开头。

   如果 *prefix* 不是 "None"，则文件名将以该前缀开头，是 "None"
   则使用默认前缀。默认前缀是 "gettempprefix()" 或 "gettempprefixb()"
   函数的返回值（自动调用合适的函数）。

   如果 *dir* 不为 "None"，则在指定的目录创建文件，是 "None" 则使用默
   认目录。默认目录是从一个列表中选择出来的，这个列表不同平台不一样，
   但是用户可以设置 *TMPDIR*、*TEMP* 或 *TMP* 环境变量来设置目录的位置
   。因此，不能保证生成的临时文件路径很规范，比如，通过 "os.popen()"
   将路径传递给外部命令时仍需要加引号。

   如果 *suffix*、*prefix* 和 *dir* 中的任何一个不是 "None"，就要保证
   它们是同一数据类型。如果它们是 bytes，则返回的名称的类型就是 bytes
   而不是 str。如果确实要用默认参数，但又想要返回值是 bytes 类型，请传
   入 "suffix=b''"。

   如果指定了 *text* 参数，它表示的是以二进制模式（默认）还是文本模式
   打开文件。在某些平台上，两种模式没有区别。

   "mkstemp()" 返回一个元组，元组中第一个元素是句柄，它是一个系统级句
   柄，指向一个打开的文件（等同于 "os.open()" 的返回值），第二元素是该
   文件的绝对路径。

   引发一个 "tempfile.mkstemp" 审计事件，附带参数 "fullpath"。

   在 3.5 版更改: 现在，*suffix*、*prefix* 和 *dir* 可以以 bytes 类型
   按顺序提供，以获得 bytes 类型的返回值。之前只允许使用 str。*suffix*
   和 *prefix* 现在可以接受 "None"，并且默认为 "None" 以使用合适的默认
   值。

   在 3.6 版更改: *dir* 参数现在可接受一个路径类对象 (*path-like
   object*)。

tempfile.mkdtemp(suffix=None, prefix=None, dir=None)

   以最安全的方式创建一个临时目录，创建该目录时不会有竞争的情况。该目
   录只能由创建者读取、写入和搜索。

   "mkdtemp()" 用户用完临时目录后需要自行将其删除。

   *prefix*、*suffix* 和 *dir* 的含义与它们在 "mkstemp()" 中的相同。

   "mkdtemp()" 返回新目录的绝对路径。

   引发一个 "tempfile.mkdtemp" 审计事件，附带参数 "fullpath"。

   在 3.5 版更改: 现在，*suffix*、*prefix* 和 *dir* 可以以 bytes 类型
   按顺序提供，以获得 bytes 类型的返回值。之前只允许使用 str。*suffix*
   和 *prefix* 现在可以接受 "None"，并且默认为 "None" 以使用合适的默认
   值。

   在 3.6 版更改: *dir* 参数现在可接受一个路径类对象 (*path-like
   object*)。

tempfile.gettempdir()

   返回放置临时文件的目录的名称。这个方法的返回值就是本模块所有函数的
   *dir* 参数的默认值。

   Python 搜索标准目录列表，以找到调用者可以在其中创建文件的目录。这个
   列表是：

   1. "TMPDIR" 环境变量指向的目录。

   2. "TEMP" 环境变量指向的目录。

   3. "TMP" 环境变量指向的目录。

   4. 与平台相关的位置：

      * 在 Windows 上，依次为 "C:\TEMP"、"C:\TMP"、"\TEMP" 和 "\TMP"。

      * 在所有其他平台上，依次为 "/tmp"、"/var/tmp" 和 "/usr/tmp"。

   5. 不得已时，使用当前工作目录。

   搜索的结果会缓存起来，参见下面 "tempdir" 的描述。

tempfile.gettempdirb()

   与 "gettempdir()" 相同，但返回值为字节类型。

   3.5 新版功能.

tempfile.gettempprefix()

   返回用于创建临时文件的文件名前缀，它不包含目录部分。

tempfile.gettempprefixb()

   与 "gettempprefix()" 相同，但返回值为字节类型。

   3.5 新版功能.

本模块使用一个全局变量来存储由 "gettempdir()" 返回的临时文件目录路径。
可以直接给它赋值，这样可以覆盖自动选择的路径，但是不建议这样做。本模块
中的所有函数都带有一个 *dir* 参数，该参数可用于指定目录，这是推荐的方
法。

tempfile.tempdir

   当设置为 "None" 以外的其他值时，此变量将决定本模块所有函数的 *dir*
   参数的默认值。

   如果在调用除 "gettempprefix()" 外的上述任何函数时 "tempdir" 为
   "None" (默认值) 则它会按照 "gettempdir()" 中所描述的算法来初始化。


示例
====

以下是 "tempfile" 模块典型用法的一些示例:

   >>> import tempfile

   # create a temporary file and write some data to it
   >>> fp = tempfile.TemporaryFile()
   >>> fp.write(b'Hello world!')
   # read data from file
   >>> fp.seek(0)
   >>> fp.read()
   b'Hello world!'
   # close the file, it will be removed
   >>> fp.close()

   # create a temporary file using a context manager
   >>> with tempfile.TemporaryFile() as fp:
   ...     fp.write(b'Hello world!')
   ...     fp.seek(0)
   ...     fp.read()
   b'Hello world!'
   >>>
   # file is now closed and removed

   # create a temporary directory using the context manager
   >>> with tempfile.TemporaryDirectory() as tmpdirname:
   ...     print('created temporary directory', tmpdirname)
   >>>
   # directory and contents have been removed


已弃用的函数和变量
==================

创建临时文件有一种历史方法，首先使用 "mktemp()" 函数生成一个文件名，然
后使用该文件名创建文件。不幸的是，这是不安全的，因为在调用 "mktemp()"
与随后尝试创建文件的进程之间的时间里，其他进程可能会使用该名称创建文件
。解决方案是将两个步骤结合起来，立即创建文件。这个方案目前被
"mkstemp()" 和上述其他函数所采用。

tempfile.mktemp(suffix='', prefix='tmp', dir=None)

   2.3 版后已移除: 使用 "mkstemp()" 来代替。

   返回一个绝对路径，这个路径指向的文件在调用本方法时不存在。*prefix*
   、*suffix* 和 *dir* 参数与 "mkstemp()" 中的同名参数类似，不同之处在
   于不支持字节类型的文件名，不支持 "suffix=None" 和 "prefix=None"。

   警告:

     使用此功能可能会在程序中引入安全漏洞。当你开始使用本方法返回的文
     件执行任何操作时，可能有人已经捷足先登了。"mktemp()" 的功能可以很
     轻松地用 "NamedTemporaryFile()" 代替，当然需要传递 "delete=False"
     参数:

        >>> f = NamedTemporaryFile(delete=False)
        >>> f.name
        '/tmp/tmptjujjt'
        >>> f.write(b"Hello World!\n")
        13
        >>> f.close()
        >>> os.unlink(f.name)
        >>> os.path.exists(f.name)
        False
