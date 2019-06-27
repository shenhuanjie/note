# 第18章 Java I/O系统

> 对程序语言的设计者来说，创建一个好的输入/输出（I/O)系统是一项艰难的任务。

现有的大量不同方案已经说明了这一点。挑战似乎来自于要涵盖所有的可能性。不仅存在各种I/O源端和想要与之通信的接收端（文件、控制台、网络链接等），而且还需要以多种不同的方式与它们进行通信（顺序、随机存取、缓冲、二进制、按字符、按行、按字等）。

Java类库的设计者通过创建大量的类来解决这个难题。一开始，可能会对Java I/O系统提供了如此多的类而感到不知所措（具有讽刺意味的是，Java I/O设计的初衷是为了避免过多的类）。自从Java 1.0版本以来，Java的I/O类发生了明显改变，在原来面向字节的类中添加了面向字符和基于Unicode的类。在JDK 1.4中，添加了nio类（对于“新I/O”来说，这是一个从现在起我们将要使用若干年的名称，即使它们在JDK1.4中就已经被引入了，因此它们已经“旧‘了）添加进来是为了改进性能及功能。因此，在充分理解Java I/O系统以便正确地运用之前，我们需要学习相当数量的类。另外，很有必要理解I/O类库的演化过程，即使我们的第一反应是“不要用历史打扰我，只需告诉我怎么用。”问题是，如果缺乏历史的眼光，很快我们就会对什么时候该使用哪些类，以及什么时候不该使用它们而感到迷惑。

本章就介绍Java标准类库中各种各样的类以及它们的用法。

## 18.1 File类

在学习那些真正用于在流中读写数据的类之前，让我们先看一个实用类库工具，它可以帮助我们处理文件目录问题。

**File**（文件）类这个名字有一定的误导性；我们可能会认为它指代的是文件，实际上却并非如此。它既能代表一个特定文件的名称，又能代表一个目录下的一组文件的名称。如果它指的是一个文件集，我们就可以理解返回的是一个数组而不是某个更具灵活性的内容器，因为元素的个数是固定的，所以如果我们想取得不同的目录列表，只需要再创建一个不同的File对象就可以了。实际上，FilePath（文件路径）对这个类来说是个更好的名字。本节举例示范了这个类的用法，包括了与它相关的**FilenameFilter**接口。

### 18.1.1 目录列表器

假设我们想查看一个目录列表，可以用两种方法来使用File对象。如果我们调用不带参数的list()方法，便可以获得此File对象包含的全部列表。然而，如果我们想获得一个受限列表，例如，想要得到所有扩展名为.java的文件，那么我们就要用到“目录过滤器”，这个类会告诉我们该怎样显示符合条件的File对象。

下面是一个示例，注意，通过使用java.utils.Array.sort()和String.CASE_INSENEITVE.ORDERComponent，可以很容易地对结果进行排序（按字母顺序）。

```java
package com.shenhuanjie.io;

import java.io.File;
import java.util.Arrays;

/**
 * DirList
 * <p>
 * Display a directory listing using regular expressions.
 *
 * @author shenhuanjie
 * @date 2019/6/27 17:34
 */
public class DirList {
    public static void main(String[] args) {
        File path = new File(".");
        String[] list;
        if (args.length == 0) {
            list = path.list();
        } else {
            list = path.list(new DirFilter(args[0]));
        }
        Arrays.sort(list, String.CASE_INSENSITIVE_ORDER);
        for (String dirItem : list) {
            System.out.println(dirItem);
        }
    }
}

```

```java
package com.shenhuanjie.io;

import java.io.File;
import java.io.FilenameFilter;
import java.util.regex.Pattern;

/**
 * DirFilter
 * <p>
 * Display a dirctory listing using regular expression.
 * <p>
 * {Args:C.*\.java}
 *
 * @author shenhuanjie
 * @date 2019/6/27 17:07
 */
public class DirFilter implements FilenameFilter {
    private Pattern pattern;

    /**
     * DirFilter
     */
    public DirFilter(String regex) {
        pattern = Pattern.compile(regex);
    }

    /**
     * Tests if a specified file should be included in a file list.
     *
     * @param dir  the directory in which the file was found.
     * @param name the name of the file.
     * @return <code>true</code> if and only if the name should be
     * included in the file list; <code>false</code> otherwise.
     */
    @Override
    public boolean accept(File dir, String name) {
        return pattern.matcher(name).matches();
    }
}

```

```java
/**
 * .idea
 * pom.xml
 * src
 * target
 * think-in-java.iml
 */
```

这里，DirFilter类“实现”了FilenameFilter接口。请注意FilenameFilter接口是多么简单：

```java
/*
 * Copyright (c) 1994, 2013, Oracle and/or its affiliates. All rights reserved.
 * ORACLE PROPRIETARY/CONFIDENTIAL. Use is subject to license terms.
 */

package java.io;

/**
 * Instances of classes that implement this interface are used to
 * filter filenames. These instances are used to filter directory
 * listings in the <code>list</code> method of class
 * <code>File</code>, and by the Abstract Window Toolkit's file
 * dialog component.
 *
 * @author  Arthur van Hoff
 * @author  Jonathan Payne
 * @see     java.awt.FileDialog#setFilenameFilter(java.io.FilenameFilter)
 * @see     java.io.File
 * @see     java.io.File#list(java.io.FilenameFilter)
 * @since   JDK1.0
 */
@FunctionalInterface
public interface FilenameFilter {
    /**
     * Tests if a specified file should be included in a file list.
     *
     * @param   dir    the directory in which the file was found.
     * @param   name   the name of the file.
     * @return  <code>true</code> if and only if the name should be
     * included in the file list; <code>false</code> otherwise.
     */
    boolean accept(File dir, String name);
}

```

DirFilter这个类存在的唯一原因就是将accept()方法。创建这个类的目的在于把accept()方法提供给list()使用，使list()可以回调accept()，进而以决定哪些文件包含在列表中。因此，这种结构常常也称为**回调**。更具体地说，这是一个**策略模式**的例子，因为list()实现了基本的功能，而且按照FillenameFilter的形式提供了这个**策略**，以便完善list()在提供服务时所需的算法。因为list()接受FilenameFilter对象作为参数，这意味着我们可以传递实现了FilenameFilter接口的任何类的对象，用以选择（甚至在运行时）list()方法的行为方式。策略的目的就是提供了代码行为的灵活性。

accept()方法必须接受一个代表某个特定文件所在目录的File对象，以及包含了那个文件名的一个String。记住一点：list()方法会为此目录对象下的每个文件名调用accept()，来判断该文件是否包含在内；判断结果由accept()返回的布尔值表示。

accept()会使用一个正则表达式的matcher对象，来查看此正则表达式regex是否匹配这个文件的名字。通过accept()，list()方法会返回一个数组。

**匿名内部类**

这个例子很适合用一个匿名内部类（第8章介绍过）进行改写。首先创建一个filter()方法，它会返回一个指向FilenameFilter的引用：

```java
package com.shenhuanjie.io;

import java.io.File;
import java.io.FilenameFilter;
import java.util.Arrays;
import java.util.regex.Pattern;

/**
 * DirList2
 * <p>
 * Uses anonymous inner classes.
 *
 * @author shenhuanjie
 * @date 2019/6/27 17:54
 */
public class DirList2 {
    /**
     * filter
     *
     * @param regex
     * @return
     */
    public static FilenameFilter filter(final String regex) {
        // Creation of anonymous inner class:
        return new FilenameFilter() {
            private Pattern pattern = Pattern.compile(regex);

            @Override
            public boolean accept(File dir, String name) {
                return pattern.matcher(name).matches();
            }
        };// End of anonymous inner class
    }

    /**
     * main
     *
     * @param args
     */
    public static void main(String[] args) {
        File path = new File(".");
        String[] list;
        if (args.length == 0) {
            list = path.list();
        } else {
            list = path.list(filter(args[0]));
        }
        Arrays.sort(list, String.CASE_INSENSITIVE_ORDER);
        for (String dirItem : list) {
            System.out.println(dirItem);
        }
    }

}

```

注意，传向filter()的参数必须是final的。这在匿名内部类中是必需的，这样它才能够使用来自该类范围之外的对象。

这个设计有所改进，因为现在FilenameFilter类紧密地和DirList2绑定在一起。然而，我们可以进一步修改该方法，定义一个作为list()参数的匿名内部类；这样一来程序会变得更小：

```java
package com.shenhuanjie.io;

import java.io.File;
import java.io.FilenameFilter;
import java.util.Arrays;
import java.util.regex.Pattern;

/**
 * DirList3
 * <p>
 * Building the anonymous inner class "in-place"
 *
 * @author shenhuanjie
 * @date 2019/6/27 18:18
 */
public class DirList3 {
    public static void main(String[] args) {
        File path = new File(".");
        String[] list;
        if (args.length == 0) {
            list = path.list();
        } else {
            list = path.list(new FilenameFilter() {
                private Pattern pattern = Pattern.compile(args[0]);

                @Override
                public boolean accept(File dir, String name) {
                    return pattern.matcher(name).matches();
                }
            });
        }
        Arrays.sort(list, String.CASE_INSENSITIVE_ORDER);
        for (String dirItem : list) {
            System.out.println(dirItem);
        }
    }
}
/**
 * .idea
 * pom.xml
 * src
 * target
 * think-in-java.iml
 */

```

既然匿名内部类直接使用args[0]，那么传递给main()方法的参数现在就是final的。

这个例子展示了匿名内部类怎样通过创建特定的、一次性的类来解决问题。此方法的一个优点就是将解决特定问题的代码隔离、聚拢于一点。而另一方面，这种方法却不易阅读，因此要谨慎使用。

### 18.1.2 目录实用工具

程序设计中一项常见的任务就是在文件集上执行操作，这些文件要么在本地目录中，要么遍布于整个目录树中。如果有一种工具能够为你产生这个文件集，那么它会非常有用。下面的实用工具类就可以通过使用local()方法产生由本地目录中的文件构成的File对象数组，或者通过使用walk()方法产生给定目录下的由整个目录树中所有文件构成的List<File>（File对象比文件名更有用，因为File对象包含更多的信息）。这些文件是基于你提供的正则表达式而被选中的。

```java
package com.shenhuanjie.util;

import java.io.File;
import java.io.FilenameFilter;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.regex.Pattern;

/**
 * Directory
 * <p>
 * Produce a sequence of File objects that match a
 * regular expression in either a local directory.
 * or by walking a directory tree.
 *
 * @author shenhuanjie
 * @date 2019/6/27 21:27
 */
public final class Directory {
    /**
     * local
     *
     * @param dir
     * @param regex
     * @return
     */
    public static File[] local(File dir, final String regex) {
        return dir.listFiles(new FilenameFilter() {
            private Pattern pattern = Pattern.compile(regex);

            @Override
            public boolean accept(File dir, String name) {
                return pattern.matcher(new File(name).getName()).matches();
            }
        });
    }

    /**
     * local
     *
     * @param path
     * @param regex
     * @return
     */
    public static File[] local(String path, final String regex) {
        //Overloaded
        return local(new File(path), regex);
    }

    /**
     * TreeInfo
     * <p>
     * A two-tuple for returning a pair of objects;
     */
    public static class TreeInfo implements Iterable<File> {
        public List<File> files = new ArrayList<File>();
        public List<File> dirs = new ArrayList<File>();

        /**
         * walk
         *
         * @param start
         * @param regex
         * @return
         */
        public static TreeInfo walk(String start, String regex) {
            //Begin recursion
            return recurseDirs(new File(start), regex);
        }

        /**
         * walk
         *
         * @param start
         * @param regex
         * @return
         */
        public static TreeInfo walk(File start, String regex) {
            //Overloaded
            return recurseDirs(start, regex);
        }

        /**
         * walk
         *
         * @param start
         * @return
         */
        public static TreeInfo walk(File start) {
            // Everything
            return recurseDirs(start, ".*");
        }

        /**
         * walk
         *
         * @param start
         * @return
         */
        public static TreeInfo walk(String start) {
            return recurseDirs(new File(start), ".*");
        }

        /**
         * recurseDirs
         *
         * @param startDir
         * @param regex
         * @return
         */
        private static TreeInfo recurseDirs(File startDir, String regex) {
            TreeInfo result = new TreeInfo();
            for (File item : startDir.listFiles()) {
                if (item.isDirectory()) {
                    result.dirs.add(item);
                    result.addAll(recurseDirs(item, regex));
                } else
                    //Regular file
                    if (item.getName().matches(regex)) {
                        result.files.add(item);
                    }
            }
            return result;
        }

        /**
         * Simple validation test;
         *
         * @param args
         */
        public static void main(String[] args) {
            if (args.length == 0) {
                System.out.println(walk("."));
            } else {
                for (String arg : args) {
                    System.out.println(walk(arg));
                }
            }
        }

        /**
         * The default iterable element is the file list:
         * <p>
         * Returns an iterator over elements of type {@code T}.
         *
         * @return an Iterator.
         */
        @Override
        public Iterator<File> iterator() {
            return files.iterator();
        }

        /**
         * addAll
         *
         * @param other
         */
        void addAll(TreeInfo other) {
            files.addAll(other.files);
            dirs.addAll(other.dirs);
        }

        /**
         * toString
         *
         * @return
         */
        @Override
        public String toString() {
            return "dirs:" + PPrint.pformat(dirs) +
                    "\n\nfiles:" + PPrint.pformat(files);
        }
    }
}

```

local()方法使用被称为listFile()的File.list()的变体来产生File数组。可以看到，它还使用了FilenameFilter。如果需要List而不是数组，你可以使用Array.asList()自己对结果进行转换。

walk()方法将开始目录的名字转换为File对象，然后调用recurseDirs()，该方法将递归地遍历目录，并在每次递归中都收集更多的信息。为了区分普通文件和目录，返回值实际上是一个对象“元组”——一个List持有所有普通文件，另一个持有目录。这里，所有的域都被有意识地设置成了public，因为TreeInfo的使命只是将对象收集起来——如果你只是返回List，那么就不需要将其设置为private，因为你只是返回一个对象对，不需要将它们设置为private。注意，TreeInfo实现了Iterable<File>，它将产生文件，使你拥有在文件列表上的“默认迭代”，而你可以通过声明“.dirs”来指定目录。

TreeInfo.toString()方法使用了一个“灵巧打印机”类，以使输出更容易浏览。容器默认的toString()方法会在单个行中打印容器中的所有元素，对于大型集合来说，这会变得难以阅读，因此你可能希望使用可替换的格式化机制。下面是一个可以添加新行并缩进所有元素的工具：

```java
package com.shenhuanjie.util;

import java.util.Arrays;
import java.util.Collection;

/**
 * PPrint
 *
 * @author shenhuanjie
 * @date 2019/6/27 21:38
 */
public class PPrint {
    /**
     * pformat
     *
     * @param objects
     * @return
     */
    public static String pformat(Collection<?> objects) {
        if (objects.size() == 0) {
            return "[]";
        }
        StringBuilder result = new StringBuilder("[");
        for (Object elem : objects) {
            if (objects.size() != 1) {
                result.append("\n  ");
            }
            result.append(elem);
        }
        if (objects.size() != 1) {
            result.append("\n");
        }
        result.append("]");
        return result.toString();
    }

    /**
     * pprint
     *
     * @param objects
     */
    public static void pprint(Collection<?> objects) {
        System.out.println(pformat(objects));
    }

    /**
     * pprint
     *
     * @param objectList
     */
    public static void pprint(Object[] objectList) {
        System.out.println(pformat(Arrays.asList(objectList)));
    }
}

```

pformat()方法可以从Collection中产生格式化的String，而pprint()方法使用pformat()来执行其任务。注意，没有任何元素和只有一个元素这两种特例进行了不同的处理。上面还有一个用于数组的pprint()版本。

Directory实用工具放在了util包中，以使其更容易地被获得。下面的例子说明了可以如何使用它的样本：

```java
package com.shenhuanjie.io;

import com.shenhuanjie.util.Directory;
import com.shenhuanjie.util.PPrint;

import java.io.File;

/**
 * DirectoryDemo
 * <p>
 * Simple use of Directory utilities.
 *
 * @author shenhuanjie
 * @date 2019/6/27 22:15
 */
public class DirectoryDemo {
    /**
     * main
     *
     * @param args
     */
    public static void main(String[] args) {
        // All directories:
        PPrint.pprint(Directory.TreeInfo.walk(".").dirs);
        // All files beginning with 'T'
        for (File file : Directory.local(".", "T.*")) {
            System.out.println(file);
        }
        System.out.println("----------");
        // All Java files beginning with 'T':
        for (File file : Directory.TreeInfo.walk(".", "T.*\\.java")) {
            System.out.println(file);
        }
        System.out.println("==========");
        // Class files containing "Z" or "z":
        for (File file : Directory.TreeInfo.walk(".", ".*[Zz].*\\.class")) {
            System.out.println(file);
        }

    }
}

```

你可能需要更新一下在第13章中学习到的有关正则表达式的知识，以理解在local()和walk()中的第二个参数。

我们可以更进一步，创建一个工具，它可以在目录中穿行，并且跟进Strategy对象来处理这些目录中的文件（这是策略设计模式的另一个示例）：

```java
package com.shenhuanjie.util;

import java.io.File;
import java.io.IOException;

/**
 * ProcessFiles
 *
 * @author shenhuanjie
 * @date 2019/6/27 22:42
 */
public class ProcessFiles {
    private Strategy strategy;
    private String ext;

    /**
     * ProcessFiles
     *
     * @param strategy
     * @param ext
     */
    public ProcessFiles(Strategy strategy, String ext) {
        this.strategy = strategy;
        this.ext = ext;
    }

    /**
     * main
     *
     * @param args
     */
    public static void main(String[] args) {
        new ProcessFiles(file -> System.out.println(file), "java").start(args);
    }

    /**
     * start
     *
     * @param args
     */
    public void start(String[] args) {
        try {
            if (args.length == 0) {
                processDirectoryTree(new File("."));
            } else {
                for (String arg : args) {
                    File fileArg = new File(arg);
                    if (fileArg.isDirectory()) {
                        processDirectoryTree(fileArg);
                    } else
                        // Allow user to leave off extension:
                        if (!arg.endsWith("." + ext)) {
                            arg += "." + ext;
                        }
                    strategy.process(new File(arg).getCanonicalFile());
                }
            }

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * processDirectoryTree
     *
     * @param root
     * @throws IOException
     */
    private void processDirectoryTree(File root) throws IOException {
        for (File file : Directory.TreeInfo.walk(root.getAbsolutePath(), ".*\\." + ext)) {
            strategy.process(file.getCanonicalFile());
        }
    }

    /**
     * Strategy
     */
    public interface Strategy {
        void process(File file);
    }
}

```

Strategy接口内嵌在ProcessFiles中，使得如果你希望实现它，就必须实现ProcessFiles.Strategy，它为读者提供了更多的上下文信息。ProcessFiles执行了查找具有特定扩展名（传递给构造器的ext参数）的文件所需的全部工作，并且当它找到匹配的文件时，将直接把文件传递给Strategy对象（也是传递给构造器的参数）。

如果你没有提供任何参数，那么ProcessFiles就假设你希望遍历当前目录下的所有目录。你也可以指定特定的文件，带不带扩展名都可以（如果必需的话，它会添加上扩展名），或者指定一个或多个目录。

在main()中，你看到了如何使用这个工具的基本示例，它可以根据你提供的命令行来打印所有的Java源代码文件的名字。