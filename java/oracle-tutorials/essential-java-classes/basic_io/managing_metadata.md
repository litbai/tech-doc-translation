### Managing Metadata(File and File Store Attribute)

元数据的定义为：关于数据的数据。对于文件系统来说，数据存储于文件和目录中，而元数据是用来描述文件和目录本身的数据：比如一个文件是普通文件、目录文件或者符号链接？它的size、创建时间、最后修改时间、文件拥有者、组拥有者、访问权限等等等等。


一个文件系统的元数据通常指的是它的文件的属性。Files类有一些方法可以获得一个文件的属性、或设置文件属性。


|methods|comments|
|------------------------|-----------------------------------|
|size(Path)|返回文件的字节数|
|isDirectory(Path, LinkOption)|path指定的文件是否是一个目录文件|
|isRegularFile(Path, LinkOption)|path指定的文件是否是一个普通文件|
|isSymboliclink(Path)|path指定的文件是否是符号链接|
|isHidden(Path)|是否是隐藏文件|
|getLastModifiedTime(Path, LinkOption)|返回文件的最后修改时间|
|setLastModifiedTime(Path, FileTime)|设置文件的最后修改时间|
|getOwner(Path, LinkOption)|返回文件拥有者|
|setOwner(Path, UserPrincipal)|设置文件拥有者|
|getPosixFilePermissions(Path, LinkOption...)|返回文件的posix权限|
|setPosixFilePermissions(Path, Set<PosixFilePermission>)|设置文件的Posix权限|
|getAttribute(Path, String, LinkOption...)|获取文件属性|
|setAttribute(Path, String, Object, LinkOption...)|设置文件属性|


如果一个程序需要同时读取文件的多个属性，那么循环的使用设置单属性的方法效率比较低。因此，Files类提供了两个readAttributes方法来一次获取文件的多个属性：

|method|comment|
|------------------------|----------------------------------|
|readAttributes(Path, String, LinkOption...)|读取文件多个属性, String可指定多个文件属性，以逗号隔开|
|readAttributes(Path, Class\<A\>, LinkOption...)|读取文件多个属性，Class\<A\>是文件属性的类型|


仅看上述的表格，可能比较迷惑如何使用这两个方法，后面将会有代码示例来展示如何使用这两个方法。

在展示如何使用上述两个方法之前，有必要提醒读者不同的文件系统有不同的属性定义。由于这个原因，通常将相关的文件属性被组合到视图中。一个视图映射到一个特定的文件系统的实现，如POSIX或DOS，或一个共同的功能，如文件的所有权。


目前支持的视图如下：

* BasicFileAttributeView -- 提供了一个基本的文件属性的视图， 这些属性被所有的文件系统支持
* DosFileAttributeView -- 扩展了基本文件属性视图，添加了DOS属性支持
* PosixFileAttributeView -- 扩展了基本文件属性视图，添加了对支持POSIX标准的文件系统的支持，例如UNIX。这些属性包括 file owner, group owner以及相关的文件权限。
* FileOwnerAttributeView -- 所有支持file owner的文件系统实现都支持这个view
* AclFileAttributeView -- 支持读取或者更新一个文件的Access Control Lists(ACL).
* UserDefinedFileAttributeView -- 支持用户自定义的metadata。


一个具体的文件系统实现可能仅支持上面的基本属性视图，或者支持多个属性视图。一个文件系统的实现还可能包括除了上述列出的API包含的视图之外的其他视图。


在大部分的场景下，你不应该直接使用任何FileAttributeView接口。你可以使用Files.getFileAttributeView(Path, Class<V>, LinkOption)来获得一个View。


readAttributes方法使用了泛型，可以使用它获得所有类型的View。


#### Basic File Attributes

前面提到过，如果你想读取一个文件的基本属性，那么可以使用Files.readAttributes方法。这个方法可以一次性读取文件的多个基本属性，这个方法比每次只读取一个属性要高效的多。可变参数LinkOption目前仅支持 NOFOLLOW_LINKS。如果你想读取符号链接本身的属性，而不是target的属性，那么你需要指定这个选项。


** A word about time stamps: 基本的属性包含3个时间戳： 创建时间、最后修改时间、最后访问时间。具体的实现中，上述几个属性可能不会被全部支持，在这种情况下，会返回一个与具体实现有关的值。当全部支持这3个属性时，会返回一个FileTime对象。 **


下面的代码片段读取并打印出了文件的基本属性，使用了BasicFileAttributes接口中的一些方法:

```
Path file = ...;
BasicFileAttributes attr = Files.readAttributes(file, BasicFileAttributes.class);

System.out.println("creationTime: " + attr.creationTime());
System.out.println("lastAccessTime: " + attr.lastAccessTime());
System.out.println("lastModifiedTime: " + attr.lastModifiedTime());

System.out.println("isDiretory: " + attr.isDirectory());
System.out.println("isOther: " + attr.isOther());
System.out.println("isRegularFile: " + attr.isRegularFile());
System.out.println("isSymblicLink: " + arrt.isSymbolicLink());
System.out.println("size: " + attr.size());

```

除了上述访问方法，还有个fileKey方法，这个方法可以返回一个对象唯一标识一个文件，或者返回null，如果file key不可用的话。


#### Setting Time Stamps

下面的代码片段可以设置lastModifiedTime：


```
Path file = ...;

BasicFileAttributes attr = Files.readAttributes(file, BasicFileAttributes.class);

long currentTime = System.currentTimeMillis();

FileTime ft = FileTime.fromMillils(currentTime);
Files.setLastModifiedTime(file, ft);

```


#### DOS File Attribute

DOS文件属性在除了DOS的文件系统上也被支持，例如Samba。代码示例：


```
Path file = ...;
try {
    DosFileAttributes attr =
        Files.readAttributes(file, DosFileAttributes.class);
    System.out.println("isReadOnly is " + attr.isReadOnly());
    System.out.println("isHidden is " + attr.isHidden());
    System.out.println("isArchive is " + attr.isArchive());
    System.out.println("isSystem is " + attr.isSystem());
} catch (UnsupportedOperationException x) {
    System.err.println("DOS file" +
        " attributes not supported:" + x);
}

```

你也可以使用setAttribute方法来设置文件属性：

```
Path file = ...;
Files.setAttribute(file, "dos:hidden", true);

```

#### POSIX File Permissions


POSIX是Portable Operating System Interface for UNIX 的首字母缩写，是IEEE和ISO指定的一系列标准，来保证在不同的UNIX发行版本中保持互操作性。如果一个程序遵循了POSIX标准，它很容易的可以移植到其他POSIX兼容的操作系统中。


除了文件拥有者和组拥有者，POSIX一共支持9种权限：文件拥有者、所在组、以及其他组成员对文件的读、写、执行权限。


下面的代码读取了一个文件的POSIX文件属性，并且将它们进行打印，它使用了PosixFileAttributes类中的一些方法。


```
Path file = ...;
PosixFileAttribute attr = Files.readAttributes(file, PosixFileAttributes.class);

System.out.format("%s %s %s%n", attr.owner().getName(), attr.group().getName(), PosixFilePermissions.toString(attr.permissions()));

```


PosixFilePermissions辅助类提供了一些有用的方法：

* toString(): 将file permission转化为字符串形式（格式为 rwxrw-r---）
* fromString(): 将一个代表文件权限的字符串转化为一个Set<PosixFilePermission>
* asFileAttribute(): 接收一个Set<PosixFilePermission>， 构造一个file attribute对象，这个对象可以传递给Files.createFile()或者Files.createDirectory()方法.


下面的代码片段读取了一个文件的属性，并且创建了一个新文件，将新文件的属性设置为与读取的文件属性一致：


```
Path sourceFile = ...;
Path newFile = ...;

PosixFileAttributes attrs = Files.readAttributes(sourceFile, PosixFileAttributes.class);

FileAttribute<Set<PosixFilePermission>> attr = PosixFilePermissions.asFileAttribute(attrs.permissions());

Files.createFile(file, attr);

```

asFileAttribute方法将一个Set<PosixFilePermission>包装为一个FileAttribute对象。然后基于这个对象，创建一个新文件。需要注意的是，创建新文件时，虽然指定了权限，比如指定了'rwxrwxrw-'，但是这个权限还必须减掉umask的值，才是新创建的文件的真正权限。umask可以保证系统更安全。 查看umask的命令：  umask， 默认是022.


如果你想要将文件设置为期望的属性，你可以在创建文件后，使用setPosixFilePermissions()方法来设置，这时候不受umask的干扰：


```

Path file = ...;

Set<PosixFilePermission> perms = PosixFilePermissions.fromString("rwxrwxrw-");

Files.setPosixFilePermissions(file, perms);

```

Chmod示例程序，递归的改变文件的权限，与chmod命令的工作方式类似： http://docs.oracle.com/javase/tutorial/displayCode.html?code=http://docs.oracle.com/javase/tutorial/essential/io/examples/Chmod.java


#### Setting a File or Group Owner


为了将一个name转为一个file owner或者group owner对象，你可以使用UserPrincipalLookupService。这个类可以接收传入的字符串表示的name或group，返回一个UserPrincipal对象。你可以使用FileSystem.getUserPrincipalLookupService()方法来得到这个service对象。

下面的代码可以更改文件所有者，但是可能需要管理员权限才可以更改成功。

```
Path file = ...;
UserPrincipal owner = file.getFileSystem().getUserPrincipalLookupService().lookupPrincipalByName("sally");

Files.setOwner(file, owner);

```

Files类中没有提供更改组拥有者的方法，但是可以通过PosixFileAttributeView来做到：

```
Path file = ...;

GroupPrincipal group = file.getFileSystem().getUserPrincipalLookupService().lookupPrincipalByGroupName("green");

Files.getFileAttributeView(file, PosixFileAttributeView.class).setGroup(group);

```


#### User-Defined File Attributes


如果文件系统支持的属性不满足你的需求，你还可以使用UserDefinedAttributeView来创建并监测自定义的属性。


... 暂用不到


#### File Store Attribute


你可以使用FileStore类来学习一个file Store，例如 有多少空间可用。getFileStore(Path)方法可以得到一个具体文件的file store。


```
Path file = ...;
FileStore store = Files.getFileStore(file);

long total = store.getTotalSpace() / 1024;

long used = (store.getTotalSpace() - store.getUnallocatedSpace()) / 1024;

long avail = store.getUsableSpace() / 1024;

```

DiskUsage示例使用了上述的API打印了磁盘信息。

http://docs.oracle.com/javase/tutorial/displayCode.html?code=http://docs.oracle.com/javase/tutorial/essential/io/examples/DiskUsage.java


































