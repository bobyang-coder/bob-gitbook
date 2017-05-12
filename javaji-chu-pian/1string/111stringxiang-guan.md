**下面是面试中最容易问到的有关String的问题。**

##### 1.如何比较两个字符串？使用“==”还是equals\(\)方法？

> 简单来讲，“==”测试的是两个对象的引用是否相同，而equals\(\)比较的是两个字符串的值是否相等。除非你想检查的是两个字符串是否是同一个对象，否则你应该使用equals\(\)来比较字符串。
>
> 如果你知道interning的概念的话，那就更好了。
>
> 为什么针对安全保密高的信息，char\[\]比String更好?
>
> 因为String是不可变的，就是说它一旦创建，就不能更改了，直到垃圾收集器将它回收走。而字符数组中的元素是可以更改的（译者注：这就意味着你就可以在使用完之后将其更改，而不会保留原始的数据）。所以使用字符数组的话，安全保密性高的信息\(如密码之类的\)将不会存在于系统中被他人看到。

##### 2.我们可以针对字符串使用switch条件语句吗？

> 对于JDK 7，回答是肯定的。从JDK 7开始, 我们可以针对字符串使用switch条件语句了；在JDK 6或者之前的版本，我们则不能使用switch条件语句。

##### 3.如何将字符串转化成int?

> int n = Integer.parseInt\("10"\);
>
> 很简单，也经常使用，但经常被忽略。

##### 4.如何将字符串用空白字符分割开

> 我们可以使用正则表达式来做到分割字符。“\s”代表空白字符” “, “\t”, “\r”, “\n”.
>
> `String[] strArray = aString.split("\s+");`

##### 5.substring\(\)方法到底做了什么？

> 在JDK 6中, substring\(\)的做法是，用一个字符数组来表示现存的字符串，然后给这个字符数组提供一个“窗口”，但实际并没有创建一个新的字符数组。要创建一个新的字符串对象由新的字符串数组表示的话，你需要加上一个空字符串，如下所示：
>
> str.substring\(m, n\) + ""
>
> 这会创建一个新的字符数组，用来表示新的字符串。这种方法会让你的代码更快，因为垃圾收集器会收集不用的长字符串，而仅保存要使用的子字符串。
>
> 在Oracle JDK 7中，substring\(\)会创建新的字符数组，而不是使用现存的字符数组。点击查看JDK 6和JDK 7中substring\(\)的分别。

##### 6.String vs StringBuilder vs StringBuffer

> String vs StringBuilder: StringBuilder是可变的，这意味着它创建之后仍旧可以更改它的值。
>
> StringBuilder vs StringBuffer: StringBuffer是synchronized的,它是线程安全的的，但是比StringBuilder要慢。

##### 7.如何重复一个字符串

> 在Python中,我们可以乘一个数值来重复一个字符串。在Java中，我们可以使用Apache Commons Lang包中的StringUtils.repeat\(\)方法来重复一个字符串。
>
> String str = "abcd";
>
> String repeated = StringUtils.repeat\(str,3\);
>
> //abcdabcdabcd

##### 8.如何将字符串转换成时间

> `String str = "Sep 17, 2013";`
>
> `Date date = new SimpleDateFormat("MMMM d, yy", Locale.ENGLISH).parse(str);`
>
> `System.out.println(date);`
>
> `//Tue Sep 17 00:00:00 EDT 2013`

##### 9.如何计算一个字符串某个字符的出现次数?

> 请使用apache commons lang包中的StringUtils：
>
> int n = StringUtils.countMatches\("11112222", "1"\);
>
> System.out.println\(n\);



