malloc 函数
void *malloc( size_t size );//size 的单位为字节 BYTE

calloc 函数
void *calloc( size_t num, size_t size );



- malloc中遇到的问题：
![](https://i.imgur.com/HTqb8Mb.png)

- 面试题：
下面的代码片段输出是什么？为什么？
<pre>
char *ptr;
if((ptr = (char *)malloc(0))==NULL)
    puts("Got a null pointer");
else
    puts("Got a valid pointer");
</pre>

 解析：......故意把0值传给了函数malloc，得到了一个合法的指针，这就是上面的代码，该代码的输出是"Got a valid pointer"。

将程序改成：
<pre>
char *ptr;
if(int pp = (strlen(ptr=(char *)malloc(0))) == 0)
    puts("Got a null pointer");
else
    puts("Got a valid pointer");</pre>
或者
<pre>
char *ptr;
if(int pp = (sizeof(ptr=(char *)malloc(0))) == 4)
    puts("Got a null pointer");
else
    puts("Got a valid pointer");
</pre>

如果求ptr的strlen的值和sizeof的值，该代码的输出是"Got a null pointer"。

- 野指针与悬空指针

1. 野指针：(wild pointer)就是没有被初始化过的指针。
　程序里定义了一个指针而又没有给这个指针一个具体的地址指向时，这样的指针就是一个野指针。野指针不是NULL指针,是指向“垃圾”内存的指针。人们一般不会错用NULL指针，因为用if语句很容易判断。但是“野指针”是很危险的，if语句对它不起作用。

2. 悬空指针：是指针最初指向的内存已经被释放了的一种指针。 
　它指向不再存在的对象。虽然指针变量仍包含有效的内存地址，但该地址中的数据不再有效，原因通常是该地址已通过调用free()而释放。实际上,这一存储单元可能已被重新分配作其他用途。悬空指针的存在所带来的固有风险是，该存储单元中的新数据可能会被旧指针的拥有者破坏。