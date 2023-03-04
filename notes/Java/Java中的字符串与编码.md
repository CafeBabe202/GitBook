# 😀 java中的字符与编码

想要弄清楚java中的字符与字符串编码，首先要弄清楚Unicode编码、UTF-8编码和UTF-16编码

### Unicode

在Unicode出现之前，已经存在很多种不同的字符编码标准：美国的ASCII编码，西欧语言中的ISO 8859-1编码，俄罗斯的KOI-8编码，中国的GB 180550和BIG-5编码等等。这样就出现了一个问题，那就同样的数值在不同编码中表示的字符是不一样的，这就是为什么有时候我们的文档或者是程序会乱码。在20世纪80年代开始启动统一工作来节解决这一问题，Unicode也就诞生了。工作开始时，人们认为两个字节的代码宽度足以对世界上各种语言的所有字符进行编码，并且有足够的空间口给未来扩展，当时所有的人都是这么向的。在1991年发布了Unicode 1.0，当时仅占用了65536个代码值不到一半的部分。

Unicode编码就好像是一个本字典，规定了每一个字符所对应的编码值，**但是Unicode并没有规定编码的存储方式**。也就是说你可以使用任何形式来储存你的二进制编码只要你自己懂得。当然这里所说的储存形式并不是储存介质，打个比方，汉字中的“汉”字的Unicode 码点是 0x6c49，对应的二进制数是 110110001001001，二进制数有 15 位，这也就说明了它至少需要 2 个字节来表示。可以想象的到，以后随着更多字符的加入，就可能需要3个字节或者更多的字节来存储，但是这样就会出现一个问题，就是那些之多字节的符号出现的概率可能会很小，甚至说根本用不到，那些1字节或者2字节的符号又会填充成制定的长度，这就浪费2倍甚至更多的空间，这是不能被人们所接受的。

### UTF-8

为了解决Unicode编码的空间问题，就诞生了这个被广泛使用的UTF-8比编码。UTF-8编码不仅实现了ASCII编码的兼容性，而且解决了Unicode的空间浪费问题。

UTF-8编码之所以能够解决空间浪费的问题，是因为他采用了可变长度的编码方式。编码规则如下：

> 1. 对于单个字节的字符，第一位设为 0，后面的 7 位对应这个字符的 Unicode 码点。因此，对于英文中的 0 - 127 号字符，与 ASCII 码完全相同。这意味着 ASCII 码那个年代的文档用 UTF-8 编码打开完全没有问题。
> 2. 对于需要使用 N 个字节来表示的字符（N > 1），第一个字节的前 N 位都设为 1，第 N + 1 位设为0，剩余的 N - 1 个字节的前两位都设位 10，剩下的二进制位则使用这个字符的 Unicode 码点来填充。

| Unicode十六进制码点范围       | UTF-8二进制编码范围                        |
| --------------------- | ----------------------------------- |
| 0000 0000 - 0000 007F | 0xxxxxxx                            |
| 0000 0080 - 0000 07FF | 110xxxxx 10xxxxxx                   |
| 0000 0800 - 0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx          |
| 0001 0000 - 0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx |

根据上面的编码表很容易就能够通过Unicode码点计算出对应的UTF-8编码值。

### UTF-16

UTF-16也是一种可变长度的编码，但是他不像UTF-8那样有1字节、2字节、3字节、4字节，他只有2字节和4字节的长度，同样是采用了Unicode的编码标准。

要了解UTF-16的编码方式首先要了解平面的概念。Unicode就像是一本很厚的字典，包含了很多字符，这么多的字符并不是一次性定义的，而是分区定义的，每一个区有65536个字符，成为一个平面，如今一共定义了17个平面，也就是说，Unicode字符集的大小为17\*65536。第一个平面称之为基本平面，其他的平面称之为辅助平面，但是基本平面内从 **U+D800** 到 **U+DFFF** 是一个空段，是不包含任何字符的。

UTF-16编码就是很巧妙的利用了基本平面中的空段来实现的，UTF-16编码的规则如下：

> 1. 如果字符在基本平面内，就直接使用两个字节来表示字符的码点
> 2. 如果字符在辅助平面内，使用四字节，将字符的高位映射到前两个字节的U+D800到U+DFFF区间，将地位映射到后两个字节的 U+D800到U+DFFF区间。

UTF-16同样解决了Unicode大量字节浪费的问题，但是他并没有向下兼容ASCII，因为ASCII是单字节，而UTF-16至少也需要两个字节。

### java中的编码

java设计之初，即采用了16位的Unicode编码，但是随大量中文，日文和其他语言符号的加入，16位的Unicode并不能表示所有的字符，从java 5开始，虽然char类型依然是两个字节，但是String类型的变成了UTF-16编码。这样就会出像一个问题，就是char类型并不能表示所有的字符，因为有些字符使用两个字节并不能存储，所以，**除非你确定你要处理代码单元，否则不建议使用char类型**，他太底层了。

### java中的char类型

java中的char类型并不是一个字节，而是两个字节，他表示UTF-16编码中的一个**代码单元**，而不是一个码点。可以使用`boolean java.lang.Character.isSupplementaryCodePoint(int codePoint)`来判断一个char类型是一个单独的字符还是一个码点的一部分。

### java中的String类型

java中的String编码类型使用了UTF-16编码类型，这意味着在java的字符串中可以使用任何符号，emoji表情符号也是没有问题的。

正是因为java中使用了UTF-16，所以导致很多小白对String类中的一些方法有一些理解上的错误。

`char charAt(int index)`：返回指定位置的代码单元。并不是指定位置的字符。

`int length()`：返回字符串的代码单元的个数。并不是字符个数。

`String substring(int beginIndex,int endIndex)`：返回一个字符串。这个字符串包含原始字符串中从beginIndex到字符串末尾或endIndex-1的所有**代码单元**。并不是从beginIndex到endIndex-1的所有字符。

```java
package cn.happyonion801.demo;

import org.junit.Test;

/**
 * time : 2021/1/11
 * author : HappyOnion801
 */
public class DemoString {
    @Test
    public void String() {
        //happyOnion801😘😘😘，IDEA居然把表情符号转译了
        String str = "happyOnion801\uD83D\uDE18\uD83D\uDE18\uD83D\uDE18";
        System.out.println(str.charAt(14));//输出的“?”乱码，并不是😘
        System.out.println(str.length());//输出的是19，并不是16
        System.out.println(str.substring(13, 15));//输出😘，并不是😘😘
    }

    /**
     * 将字符串拆分成单个字符。
     * codePointAt(int index): Returns the code point at the given index of the char array. If the char value at the given index in the char array is in the high-surrogate range, the following index is less than the length of the char array, and the char value at the following index is in the low-surrogate range, then the supplementary code point corresponding to this surrogate pair is returned. Otherwise, the char value at the given index is returned.
     * 这是jdk11中官方API文档给的解释。
     */
    @Test
    public void print() {
        String str = "happyOnion801\uD83D\uDE18\uD83D\uDE18\uD83D\uDE18";
        for (int i = 0; i < str.length(); i++) {
            if (Character.isSupplementaryCodePoint(str.codePointAt(i))) {
                System.out.println(str.substring(i, i + 2));
                i++;
            } else {
                System.out.println(str.charAt(i));
            }
        }
    }
}

```

***

> 参考文献
>
> [彻底弄懂 Unicode 编码](https://www.jianshu.com/p/6bdc0d52620a)
>
> 《Java核心技术 卷I》第11版
