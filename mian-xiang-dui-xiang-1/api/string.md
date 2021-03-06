# String

## String类

* **特点：**
  1. 字符串的内容永不可变。【重点】
  2. 正是因为字符串不可改变，所以字符串是可以共享使用的。
  3. 字符串效果上相当于是char\[\]字符数组，但是底层原理是byte\[\]字节数组。 
* **创建字符串（3+1）：**
  * `public String()`：创建一个空白字符串，不含有任何内容。
  * `public String(char[] array)`：根据字符数组的内容，来创建对应的字符串。
  * `public String(byte[] array)`：根据字节数组的内容，来创建对应的字符串。
  * 一种直接创建：`String str = "Hello"; // 右边直接用双引号`
  * ```java
    public static void main(String[] args) {
        // 使用空参构造
        String str1 = new String(); // 小括号留空，说明字符串什么内容都没有。
        System.out.println("第1个字符串：" + str1);
    
        // 根据字符数组创建字符串
        char[] charArray = { 'A', 'B', 'C' };
        String str2 = new String(charArray);
        System.out.println("第2个字符串：" + str2);
    
        // 根据字节数组创建字符串
        byte[] byteArray = { 97, 98, 99 };
        String str3 = new String(byteArray);
        System.out.println("第3个字符串：" + str3);
    
        // 直接创建
        String str4 = "Hello";
        System.out.println("第4个字符串：" + str4);
    }
    ```
* **注意事项：**
  * 字符串常量池：程序当中直接写上的双引号字符串，就在字符串常量池中。
  * 对于基本类型来说，==是进行【数值】的比较。
  * 对于引用类型来说，==是进行【地址值】的比较。
  * ```java
    public static void main(String[] args) {
        String str1 = "abc";
        String str2 = "abc";
    
        char[] charArray = {'a', 'b', 'c'};
        String str3 = new String(charArray);
    
        System.out.println(str1 == str2); // true
        System.out.println(str1 == str3); // false
        System.out.println(str2 == str3); // false
    }
    ```

![](../../.gitbook/assets/01-zi-fu-chuan-de-chang-liang-chi-.png)

* char类型new的对象不在字符串常量池内，故地址值不一样

## 字符串方法

* **字符串内容比较** 
  1. `public boolean equals(Object obj)`：参数可以是任何对象，只有参数是一个字符串并且内容相同的才会给true；否则返回false。
     * ```java
       String str1 = "Hello";
       String str2 = "Hello";
       char[] charArray = {'H', 'e', 'l', 'l', 'o'};
       String str3 = new String(charArray);

       System.out.println(str1.equals(str2)); // true
       System.out.println(str2.equals(str3)); // true
       System.out.println(str3.equals("Hello")); // true
       System.out.println("Hello".equals(str1)); // true
       ```
  2. `public boolean equalsIgnoreCase(String str)`：忽略大小写，进行内容比较。 

  * 注意事项
    1. 任何对象都能用Object进行接收。
    2. equals方法具有对称性，也就是`a.equals(b)`和`b.equals(a)`效果一样。
    3. 如果比较双方一个常量一个变量，推荐把【常量】字符串写在前面。
       * 推荐：`"abc".equals(str)` 不推荐：`str.equals("abc")`
* **字符串长度**
  * _`public int length()`_：
    * 获取字符串当中含有的字符个数，拿到字符串长度。
      * `int length = "asdasfeutrvauevbueyvb".length();` 
* **字符串拼接**
  * _`public String concat(String str)`_：
    * 将当前字符串和参数字符串拼接成为返回值新的字符串。
      * `String str3 = str1.concat(str2);` 
* **获取索引字符**
  * _`public char charAt(int index)`_：
    * 获取指定索引位置的单个字符。（索引从0开始。）
      * `char ch = "Hello".charAt(1); //e` 
* **搜索字符串位置**
  * _`public int indexOf(String str)`_：、
    * 查找参数字符串在本字符串当中首次出现的索引位置，如果没有返回-1值。
      * `String original = "HelloWorldHelloWorld";` 
      * `int index = original.indexOf("llo"); //2`  
* **截取字符串**
  * `public String substring(int index)：`
    * 截取从参数位置一直到字符串末尾，返回新字符串。
      * `String str1 = "HelloWorld"; // HelloWorld，原封不动`
      * `String str2 = str1.substring(5); // World，新字符串` 
  * `public String substring(int begin, int end)：`
    * 截取从begin开始，一直到end结束，中间的字符串。\[begin,end\)，**包含左边**，**不包含右边**。
      * `String str3 = str1.substring(4, 7); // oWo` 
* **类型转换**
  * `public char[] toCharArray()`
    * 将当前字符串拆分成为**字符数组**作为返回值。
      * `char[] chars = "Hello".toCharArray();// 转换成为字符数组`
  * `public byte[] getBytes()`
    * 获得当前字符串底层的字节数组。
    * `byte[] bytes = "abc".getBytes();` 
  * `public String replace(CharSequence oldString, CharSequence newString)`
    *  将所有出现的老字符串替换成为新的字符串，返回替换之后的结果新字符串。
    * `String str1 = "How do you do?";` 
    * `String str2 = str1.replace("o", "*");// H*w d* y*u d*?`
* **分割字符串**
  * `public String[] split(String regex)`
    * 按照参数的规则，将字符串切分成为若干部分。
    * ```java
      public static void main(String[] args) {
          String str1 = "aaa,bbb,ccc";
          String[] array1 = str1.split(",");
          for (int i = 0; i < array1.length; i++) {
              System.out.println(array1[i]);
          }
          System.out.println("===============");
          //aaa bbb ccc
      }
      ```
* 去掉前后空格
  * `public String[] trim(String trim)`
  * 字符串前后的空格移除 
* 转换为String类型
  * `String.valueOf()` 
  * 



