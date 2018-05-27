没有指针常量，因为每个变量或者函数都是在内存的地址都是未知的

C语言没有字符串类型，只能使用字符数组处理字符串

操作字符串都是在操作一个指向该字符串的指针，所以可以将这个字符串赋值给一个`指向字符的指针`而不能直接赋值给另外一个`字符数组`

C只有四种基本类型，整型，浮点型，指针和聚合类型（结构体，枚举等)，没有bool类型，可以用整型判断

函数如果默认不显视的声明返回类型，则默认返回为整型

不能简单的根据一个值得位判断它的类型，必须根据具体的使用情况


`*是间接访问符或者解引用符`
```
int i = 10; // 假设地址是1000

int *j = &i;

则
j == 1000, 为 j 的内存中储存的值
*j == 10, 解引用后为其指向的那个值得值

```

`int *j = &i;`

```
应该这样理解

int i = 10; // 假设地址是1000
int *j;
j = &i;

声明一个指向int类型的指针

使用 & 取出 i 的地址1000，然后将这个为 1000 地址储存到 j 内存中
```



如果你想将55赋值给一个内存地址为1000指针所指向的变量，会想到这样写
`*1000 = 55`

但是这里的1000是整型，需要先将1000转换成一个指向int类型的指针类型  `(int *)1000` 然后使用`*`解引用至原变量

`*(int *)1000 = 55`

这句话的意思是 将55强制赋值给内存编号为1000的一个指向int类型的一个指针所指向的变量


定义一个结构如下
struct struct_samp {
    float f;
    int a[20];
    long *lp;
    struct struct_samp2 s;
    struct struct_samp2 sa[2];
    struct struct_samp2 *sp;
}
struct struct_samp samp;

则
(samp.s).a 或 samp.s.a
(samp.sa[1]).a 或 samp.sa[1].a

struct struct_samp *sampp;
·(*sampp).f 或 sampp->f
括号不能省略，因为.的优先级比*高
->用于一个指向结构体的指针



结构体自引用

struct struct_samp {
    float f;
    int a[20];
    long *lp;
    struct struct_samp s;
}
错误, 因为成员 s 是个完整的结构，成员s 的内部还包含有另一个完整的成员s,循环引用

struct struct_samp {
    float f;
    int a[20];
    long *lp;
    struct struct_samp *s;
}

正确， 成员结构改为指针则可以定义

typedef struct {
    float f;
    int a[20];
    long *lp;
    struct struct_samp_name *s;
} struct_samp_name;

错误， struct_samp_name 还没定义好不能直接使用

typedef struct struct_samp {
    float f;
    int a[20];
    long *lp;
    struct struct_samp *s;
} struct_samp_name;

正确，可以设置一个 别名 struct_samp 使用


将结构体指针当做形参传递比把整个结构体传递效率要高的多，所以几乎所有情况都将结构体指针进行传递

函数的调用都会进行形参到实参的copy，指针也会copy，但是指针指向的内容不会copy

```c
union {
    int i;
    float j;
} ij; //i和j会在同一个内存位置储存

ij.j = 3.14; //当前内存储存为3.14
printf("%d", ij.i); //对当前内容按整型读取内存并打印
```
联合的长度取决于成员中最长的成员。如果联合中内个成员的长度相差悬殊，当储存较短的成员时，会有很大的空间浪费，这种情况下，联合中储存的应该是成员的指针而不是成员本身。

结构名是标量，都能作为左值或右值，都表示结构体本身
数组名是指针常量，作为右值时，代表当前数组的首位置，不能作为左值
