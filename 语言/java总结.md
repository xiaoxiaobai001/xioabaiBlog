##for-each语句
for(元素类型t 元素变量x : 遍历对象obj){
     引用了x的java语句;
}

##Java中数组复制的几种方法
<pre>
 1 /**
 2  * @author zhengbinMac
 3  */
 4 public class Test {
 5     public static void main(String[] args) {
 6         int[] array1 = {1,2,3,4,5};
 7         // 1.通过for循环
 8         int[] array2 = new int[5];
 9         for(int i = 0;i < array1.length;i++) {
10             array2[i] = array1[i];
11         }
12         for(int i = 0;i < array2.length;i++) {
13             System.out.print(array2[i]);
14         }
15         System.out.println();
16         //2.通过System.arraycopy()
17         int[] array3 = new int[5];
18         System.arraycopy(array1, 0, array3, 0, 5);
19         for (int i = 0; i < array3.length; i++) {
20             System.out.print(array3[i]);
21         }
22         System.out.println();
23         //3.通过Arrays.copyOf()
24         int[] array4 = new int[5];
25         array4 = Arrays.copyOf(array1, 5);
26         for (int i = 0; i < array4.length; i++) {
27             System.out.print(array4[i]);
28         }
29         System.out.println();
30         //4.通过Object.clone()
31         int[] array5 = new int[5];
32         array5 = array4.clone();
33         for (int i = 0; i < array5.length; i++) {
34             System.out.print(array5[i]);
35         }
36     }
37 }
</pre>
复制代码
1.for循环方法：
　　代码灵活，但效率低。

2.System.arraycopy()方法：
　　通过源码可以看到，其为native方法，即原生态方法。自然效率更高。

+ View code
1 public static native void arraycopy(Object src,  int  srcPos,
2                                         Object dest, int destPos,
3                                         int length);
3.Arrays.copyOf()方法：
　　同样看源码，它的实现还是基于System.arraycopy()，所以效率自然低于System.arraycpoy()。

复制代码+ View code
1 public static int[] copyOf(int[] original, int newLength) {
2 　　int[] copy = new int[newLength];
3 　　System.arraycopy(original, 0, copy, 0,
4 　　　　Math.min(original.length, newLength));
5 　　return copy;
6 }
复制代码
4.Object.clone()方法：
　　从源码来看同样也是native方法，但返回为Object类型，所以赋值时将发生强转，所以效率不如之前两种。

+ View code
1 protected native Object clone() throws CloneNotSupportedException;



##java数组、字符串和整型间相互转换

字符串数组转字符串(只能通过for循环):

String[] str = {"abc", "bcd", "def"};

StringBuffer sB = new StringBuffer();

for(int i = 0; i < str.length;i++)

{

　　sB.append(str[i]);

}

String s = sB.toString();

字符数组转字符串可以通过下面的方式:

char[] data = {"abc","bcd","def"};

String s = new String(data);

字符串到字符数组:

String str = "123abc";

char[] ar = str.toCharArray();　　//char数组

for(int i =0;i<ar.length;i++)

{

　　System.out.println(ar[i]);　　//1 2 3 a b c
}
 
String[] arr = str.split("");
for(int i =0;i<arr.length;i++)　　//String数组，不过arr[0]为空
{
      System.out.println(arr[i]);　　//空  1 2 3 a b c
}
整形到字符串:
String str = Integer.toString(i);
字符串到整形:
int i = Integer.valueOf(str).intValue();

##Java中对List集合的常用操作
https://www.cnblogs.com/epeter/p/5648026.html

list中添加，获取，删除元素；
list中是否包含某个元素；
list中根据索引将元素数值改变(替换)；
list中查看（判断）元素的索引；
根据元素索引位置进行的判断；
利用list中索引位置重新生成一个新的list（截取集合）；
对比两个list中的所有元素；
判断list是否为空；
返回Iterator集合对象；
将集合转换为字符串；
将集合转换为数组；
集合类型转换；
去重复；
 

备注：内容中代码具有关联性。

1.list中添加，获取，删除元素；

　　添加方法是：.add(e)；　　获取方法是：.get(index)；　　删除方法是：.remove(index)； 按照索引删除；　　.remove(Object o)； 按照元素内容删除；

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
List<String> person=new ArrayList<>();
            person.add("jackie");   //索引为0  //.add(e)
            person.add("peter");    //索引为1
            person.add("annie");    //索引为2
            person.add("martin");   //索引为3
            person.add("marry");    //索引为4
             
            person.remove(3);   //.remove(index)
            person.remove("marry");     //.remove(Object o)
             
            String per="";
            per=person.get(1);
            System.out.println(per);    ////.get(index)
             
            for (int i = 0; i < person.size(); i++) {
                System.out.println(person.get(i));  //.get(index)
            }
 

2.list中是否包含某个元素；

　　方法：.contains（Object o）； 返回true或者false

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
List<String> fruits=new ArrayList<>();
            fruits.add("苹果");
            fruits.add("香蕉");
            fruits.add("桃子");
            //for循环遍历list
            for (int i = 0; i < fruits.size(); i++) {
                System.out.println(fruits.get(i));
            }
            String appleString="苹果";
            //true or false
            System.out.println("fruits中是否包含苹果："+fruits.contains(appleString));
             
            if (fruits.contains(appleString)) {
                System.out.println("我喜欢吃苹果");
            }else {
                System.out.println("我不开心");
            }
 

3.list中根据索引将元素数值改变(替换)；

　　注意 .set(index, element); 和 .add(index, element); 的不同；

1
2
3
4
5
6
7
8
9
10
11
12
String a="白龙马", b="沙和尚", c="八戒", d="唐僧", e="悟空";
            List<String> people=new ArrayList<>();
            people.add(a);
            people.add(b);
            people.add(c);
            people.set(0, d);   //.set(index, element);     //将d唐僧放到list中索引为0的位置，替换a白龙马
            people.add(1, e);   //.add(index, element);     //将e悟空放到list中索引为1的位置,原来位置的b沙和尚后移一位
             
            //增强for循环遍历list
            for(String str:people){
                System.out.println(str);
            }
 

4.list中查看（判断）元素的索引；　　

　　注意：.indexOf（）； 和  lastIndexOf（）的不同；

1
2
3
4
5
6
7
8
9
10
List<String> names=new ArrayList<>();
            names.add("刘备");    //索引为0
            names.add("关羽");    //索引为1
            names.add("张飞");    //索引为2
            names.add("刘备");    //索引为3
            names.add("张飞");    //索引为4
            System.out.println(names.indexOf("刘备"));
            System.out.println(names.lastIndexOf("刘备"));
            System.out.println(names.indexOf("张飞"));
            System.out.println(names.lastIndexOf("张飞"));
 

5.根据元素索引位置进行的判断;

1
2
3
4
5
6
7
if (names.indexOf("刘备")==0) {
    System.out.println("刘备在这里");
}else if (names.lastIndexOf("刘备")==3) {
    System.out.println("刘备在那里");
}else {
    System.out.println("刘备到底在哪里？");
}
 

6.利用list中索引位置重新生成一个新的list（截取集合）；

　　方法： .subList(fromIndex, toIndex)；　　.size() ； 该方法得到list中的元素数的和

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
List<String> phone=new ArrayList<>();
            phone.add("三星");    //索引为0
            phone.add("苹果");    //索引为1
            phone.add("锤子");    //索引为2
            phone.add("华为");    //索引为3
            phone.add("小米");    //索引为4
            //原list进行遍历
            for(String pho:phone){
                System.out.println(pho);
            }
            //生成新list
            phone=phone.subList(1, 4);  //.subList(fromIndex, toIndex)      //利用索引1-4的对象重新生成一个list，但是不包含索引为4的元素，4-1=3
            for (int i = 0; i < phone.size(); i++) { // phone.size() 该方法得到list中的元素数的和
                System.out.println("新的list包含的元素是"+phone.get(i));
            }
 

7.对比两个list中的所有元素；

　　//两个相等对象的equals方法一定为true, 但两个hashcode相等的对象不一定是相等的对象

1
2
3
4
5
6
7
8
9
10
11
//1.<br>if (person.equals(fruits)) {
    System.out.println("两个list中的所有元素相同");
}else {
    System.out.println("两个list中的所有元素不一样");
}
//2.       
if (person.hashCode()==fruits.hashCode()) {
    System.out.println("我们相同");
}else {
    System.out.println("我们不一样");
}
 

8.判断list是否为空；

　　//空则返回true，非空则返回false

1
2
3
4
5
if (person.isEmpty()) {
    System.out.println("空的");
}else {
    System.out.println("不是空的");
}
 

9.返回Iterator集合对象；

1
System.out.println("返回Iterator集合对象:"+person.iterator());
 

1+0.将集合转换为字符串；

1
2
3
String liString="";
liString=person.toString();
System.out.println("将集合转换为字符串:"+liString);
 

11.将集合转换为数组；

1
System.out.println("将集合转换为数组:"+person.toArray());
 

12.集合类型转换；

1
2
3
4
5
6
7
8
9
10
//1.默认类型
List<Object> listsStrings=new ArrayList<>();
　　for (int i = 0; i < person.size(); i++) {
    listsStrings.add(person.get(i));
}
//2.指定类型
List<StringBuffer> lst=new ArrayList<>();
　　for(String string:person){
　　lst.add(StringBuffer(string));
}
 

13.去重复；

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
List<String> lst1=new ArrayList<>();
            lst1.add("aa");
            lst1.add("dd");
            lst1.add("ss");
            lst1.add("aa");
            lst1.add("ss");
 
                   //方法 1.
            for (int i = 0; i <lst1.size()-1; i++) {
                for (int j = lst1.size()-1; j >i; j--) {
                    if (lst1.get(j).equals(lst1.get(i))) {
                        lst1.remove(j);
                    }
                }
            }
            System.out.println(lst1);
             
                   //方法 2.
            List<String> lst2=new ArrayList<>();
            for (String s:lst1) {
                if (Collections.frequency(lst2, s)<1) {
                    lst2.add(s);
                }
            }
            System.out.println(lst2);
 

 

附完整代码：

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
81
82
83
84
85
86
87
88
89
90
91
92
93
94
95
96
97
98
99
100
101
102
103
104
105
106
107
108
109
110
111
112
113
114
115
116
117
118
119
120
121
122
123
124
125
126
127
128
129
130
131
132
133
134
135
136
137
138
139
140
141
142
143
144
145
146
147
148
149
150
151
152
153
154
155
156
157
158
package MyTest01;
 
import java.util.ArrayList;
import java.util.List;
 
public class ListTest01 {
 
    public static void main(String[] args) {
         
            //list中添加，获取，删除元素
            List<String> person=new ArrayList<>();
            person.add("jackie");   //索引为0  //.add(e)
            person.add("peter");    //索引为1
            person.add("annie");    //索引为2
            person.add("martin");   //索引为3
            person.add("marry");    //索引为4
             
            person.remove(3);   //.remove(index)
            person.remove("marry");     //.remove(Object o)
             
            String per="";
            per=person.get(1);
            System.out.println(per);    ////.get(index)
             
            for (int i = 0; i < person.size(); i++) {
                System.out.println(person.get(i));  //.get(index)
            }
             
             
         
            //list总是否包含某个元素
            List<String> fruits=new ArrayList<>();
            fruits.add("苹果");
            fruits.add("香蕉");
            fruits.add("桃子");
            //for循环遍历list
            for (int i = 0; i < fruits.size(); i++) {
                System.out.println(fruits.get(i));
            }
            String appleString="苹果";
            //true or false
            System.out.println("fruits中是否包含苹果："+fruits.contains(appleString));
             
            if (fruits.contains(appleString)) {
                System.out.println("我喜欢吃苹果");
            }else {
                System.out.println("我不开心");
            }
             
            //list中根据索引将元素数值改变(替换)
            String a="白龙马", b="沙和尚", c="八戒", d="唐僧", e="悟空";
            List<String> people=new ArrayList<>();
            people.add(a);
            people.add(b);
            people.add(c);
            people.set(0, d);   //.set(index, element)      //将d唐僧放到list中索引为0的位置，替换a白龙马
            people.add(1, e);   //.add(index, element);     //将e悟空放到list中索引为1的位置,原来位置的b沙和尚后移一位
             
            //增强for循环遍历list
            for(String str:people){
                System.out.println(str);
            }
             
            //list中查看（判断）元素的索引
            List<String> names=new ArrayList<>();
            names.add("刘备");    //索引为0
            names.add("关羽");    //索引为1
            names.add("张飞");    //索引为2
            names.add("刘备");    //索引为3
            names.add("张飞");    //索引为4
            System.out.println(names.indexOf("刘备"));
            System.out.println(names.lastIndexOf("刘备"));
            System.out.println(names.indexOf("张飞"));
            System.out.println(names.lastIndexOf("张飞"));
             
            //根据元素索引位置进行的判断
            if (names.indexOf("刘备")==0) {
                System.out.println("刘备在这里");
            }else if (names.lastIndexOf("刘备")==3) {
                System.out.println("刘备在那里");
            }else {
                System.out.println("刘备到底在哪里？");
            }
             
            //利用list中索引位置重新生成一个新的list（截取集合）
            List<String> phone=new ArrayList<>();
            phone.add("三星");    //索引为0
            phone.add("苹果");    //索引为1
            phone.add("锤子");    //索引为2
            phone.add("华为");    //索引为3
            phone.add("小米");    //索引为4
            //原list进行遍历
            for(String pho:phone){
                System.out.println(pho);
            }
            //生成新list
            phone=phone.subList(1, 4);  //.subList(fromIndex, toIndex)      //利用索引1-4的对象重新生成一个list，但是不包含索引为4的元素，4-1=3
            for (int i = 0; i < phone.size(); i++) { // phone.size() 该方法得到list中的元素数的和
                System.out.println("新的list包含的元素是"+phone.get(i));
            }
             
            //对比两个list中的所有元素
            //两个相等对象的equals方法一定为true, 但两个hashcode相等的对象不一定是相等的对象
            if (person.equals(fruits)) {
                System.out.println("两个list中的所有元素相同");
            }else {
                System.out.println("两个list中的所有元素不一样");
            }
             
            if (person.hashCode()==fruits.hashCode()) {
                System.out.println("我们相同");
            }else {
                System.out.println("我们不一样");
            }
             
             
            //判断list是否为空
            //空则返回true，非空则返回false
            if (person.isEmpty()) {
                System.out.println("空的");
            }else {
                System.out.println("不是空的");
            }
             
            //返回Iterator集合对象
            System.out.println("返回Iterator集合对象:"+person.iterator());
             
            //将集合转换为字符串
            String liString="";
            liString=person.toString();
            System.out.println("将集合转换为字符串:"+liString);
             
            //将集合转换为数组，默认类型
            System.out.println("将集合转换为数组:"+person.toArray());
             
            ////将集合转换为指定类型（友好的处理）
            //1.默认类型
            List<Object> listsStrings=new ArrayList<>();
            for (int i = 0; i < person.size(); i++) {
                listsStrings.add(person.get(i));
            }
            //2.指定类型
            List<StringBuffer> lst=new ArrayList<>();
            for(String string:person){
                lst.add(StringBuffer(string));
            }
             
             
             
             
    }
 
    private static StringBuffer StringBuffer(String string) {
        return null;
    }
 
 
    }