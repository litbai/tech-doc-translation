### Moving a File or Directory

你可以使用move(Path, Path, CopyOption...)方法来移动一个文件或者目录。移动操作会失败，如果target文件存在的话，除非指定了REPLACE_EXISTING选项。

空目录可以被移动。如果目录非空，只可以移动目录，而不能移动目录里面的内容(在mac OS下，貌似也移动了里面的内容)。在UNIX系统中，在同一个目录下移动文件，相当于重命名文件。在这种情况下，拷贝目录会同时拷贝目录里面的内容。


这个方法也接收可变参数，支持下面的StandardCopyOption：

* REPLACE_EXISTING -- 即使目标目录或文件存在，依然执行move操作。如果target是一个符号链接，符号链接会被replace，但是它指向的目标是不变的。
* ATOMIC_MOVE -- 原子的执行move操作。如果文件系统不支持原子的move，会抛出Exception。通过ATOMIC_MOVE, 你可以将一个文件移动到一个目录中，并且可以保证任何访问这个目录的进程都可以看到完成的文件，而不是只看到文件的一部分。


下面的代码展示了如何使用move方法：

```
import static java.nio.file.StandardCopyOption.*;

Files.move(source, target, REPLACE_EXISTING);

```


尽管你可以实现move一个目录的操作，但是这个方法通常和文件数递归操作一同使用。








































