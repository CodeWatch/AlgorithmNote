##Functions In string.h
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[strcpy](#Anchor1)  
-[memcpy](#Anchor2)  
-[memset](#Anchor3)  
-[strstr](#Anchor4)  
-[strlen](#Anchor5)  
-[strcat](#Anchor6)  

-------
<a name="Anchor1" id="Anchor1"></a>
-**[strcpy]**([Back to Index](#AnchorIndex))   

原型声明：char * strcpy(char * dest, const char * src);  
头文件：#include <string.h>  
功能：把从src地址开始且含有NULL结束符的字符串复制到以dest开始的地址空间  
说明：src和dest所指内存区域不可以重叠且dest必须有足够的空间来容纳src的字符串，这些都需要调用者来保障。  
返回指向dest的指针。    

实现：
```cpp
char* strcpy(char* strDest, const char* strSrc){
    if(strDest == strSrc){
         return strDest;
    }
    assert((strDest != NULL) && (strSrc != NULL));
    char* address = strDest;
    while((*strDest++ = *strSrc++)!='\0');
    return address;
}
```

-------
<a name="Anchor2" id="Anchor2"></a>
-**[memcpy]**([Back to Index](#AnchorIndex))   

原型声明：void *memcpy(void *dest, const void *src, size_t n);  
头文件：C语言中使用#include <string.h>;  
    C++中使用#include <cstring>和#include <string.h>都可以  
功能：从源src所指的内存地址的起始位置开始拷贝n个字节到目标dest所指的内存地址的起始位置中  
说明：  
1.source和destin所指的内存区域可以重叠，但是如果source和destin所指的内存区域重叠,那么这个函数并不能够确保source所在重叠区域在拷贝之前被覆盖。而使用memmove可以用来处理重叠区域。函数返回指向destin的指针.  
2.如果目标数组destin本身已有数据，执行memcpy（）后，将覆盖原有数据（最多覆盖n）。如果要追加数据，则每次执行memcpy后，要将目标数组地址增加到你要追加数据的地址。  
注意：source和destin都不一定是数组，任意的可读写的空间均可。  

实现：
```cpp
void * memcpy(void * destaddr,void const * srcaddr, size_t len){
     char* dest = destaddr;
     const char * src = srcaddr;
     while(len-- > 0){
          *dest++ = *src++;
     }
     return destaddr; 
}
```

-------
<a name="Anchor3" id="Anchor3"></a>
-**[memset]**([Back to Index](#AnchorIndex))   

原型声明：void *memset(void *s, int ch, size_t n);  
头文件：<memory.h> or <string.h> memset <wchar.h> wmemset  
功能：将s中前n个字节 （typedef unsigned int size_t）用 ch 替换并返回 s 。  
说明：作用是在一段内存块中填充某个给定的值，它是对较大的结构体或数组进行清零操作的一种最快方法。  

实现：  
```cpp
void * memset(void *str, int c, size_t count){
     assert(str != NULL);
     void *s = str;
     while(count--){
          *(char*)s = (char) c;
          s = (char*)s + 1;
     }
     return str;
}
```
错误使用示例：  
1.
```cpp
int f(int a[]){
     memset(a,0,sizeof(a));
  }
```
a会退化为指针，导致不能按照预想的方式初始化数组。  

2.
```cpp
int f(){
     int a[5];
     memset(a,1,20);
     for(int i = 0; i < 5; i++){
          cout << a[i] << " ";
     }
  }
```
由于memset是按照字节拷贝，因此a中的每个元素都会被初始化为0x01010101而不是1。  
3.
```cpp
int f(){
     char* s = "12345";
     memset(s,'1',5); 
}
```
运行时错误，因为s指向的是常量区（不可写内存区域），对该区域的写操作会导致程序崩溃。换成char * s = new char[5];或者char s[] = "12345";则没有问题，前者是在堆上的可读写区域，后者是在栈上的可读写区域。 

-------
<a name="Anchor4" id="Anchor4"></a>
-**[strstr]**([Back to Index](#AnchorIndex))   

原型声明：char *strstr(const char *str1, const char *str2);  
头文件：<string.h>  
功能：该函数返回str2第一次在str1中的位置，如果没有找到，返回NULL  
说明：注意两个参数均是const  

实现：  
```cpp
char* strstr(const char* strSrc, const char* str){
     assert(strSrc != NULL && str != NULL);
     const char* s = strSrc;
     const char* t = str;
     for(; *strSrc != '\0'; strSrc++){
          for(s = strSrc, t = str; *t != '\0' && *s == *t; s++, t++){}
          if(*t == '\0') return (char*)strSrc;
     }
     return NULL;
}
```

-------
<a name="Anchor5" id="Anchor5"></a>
-**[strlen]**([Back to Index](#AnchorIndex))   
  
原型声明：unsigned int strlen(const char *str);  
头文件：<string.h>  
功能：返回以'\0'结尾的字符串长度，但不包括'\0'  
说明：注意strlen只能接受char*的参数，不能与sizeof混淆  

实现：  
```cpp
unsigned int strlen(const char* str){
     assert(str != NULL);
     const char * eos = str;
     while(*eos++);
     return (unsigned int)(eos-str-1);
}
```

-------
<a name="Anchor6" id="Anchor6"></a>
-**[strcat]**([Back to Index](#AnchorIndex))   
 
原型声明：char *strcat(char *dest, const char *src);  
头文件：<string.h>  
功能：把src所指字符串添加到dest结尾处(覆盖dest结尾处的'\0')并添加'\0'。  
说明：src和dest所指内存区域不可以重叠且dest必须有足够的空间来容纳src的字符串，返回指向dest的指针。  

实现：  
```cpp
char* strcat(char* dest, const char* src){
     assert((dest != NULL) && (src != NULL));
     char * ret = dest;
     while(*dest){
          dest++;
     }
     while(*dest++ = *src++){}
     return ret;
}

```