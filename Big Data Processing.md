##Big Data Processing

大数据处理往往因数据量过大导致数据无法全部加载入内存，又或即便能够加载入内存，处理速度无法保证。此外，大数据问题又被许多因素制约，比如网络传输带宽，内存空间限制，响应时间，因此产生了不少有趣的问题，而这些问题的解决离不开分治思想，内存排序-外存合并以及哈希映射的设计与实现。

<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Find median among numbers spread in distributed system](#Anchor1)   
-[Find frequencies of each IPs](#Anchor2)  
-[Bit-map](#Anchor3)  

-------
<a name="Anchor1" id="Anchor1"></a>
-**Find median among numbers spread in distributed system**([Back to Index](#AnchorIndex))   

给定1台主机和100台从机，从机只能与主机通信，且通信速率仅有1KB/s。每台从机上有2G个INT型数据，要求在合理的时间范围内求出所有数据的中位数。

此题的思考思路如下。通信速率1KB/s说明通信速率非常低，基本不可能用于传输大量的数据，1s也只能传输256个INT数字，2G的传输不可能在合理时间内完成，通信带宽只能用于传输统计信息，如最大最小值，自身的中位数，平均值等信息。题目要求求出所有数据的中位数。  

考虑中位数的性质，对于一个长度为N的数组，其中位数左侧和右侧各有(N-1/2)个数字，从左侧和右侧分别剔除x个数不会影响原中位数的性质。  

对于题目给出的无序数据，先进行内存排序（如快排），再进行区块划分。假设到INT数据的范围为D，划分的区块长度为L（如0-200,200-400...），则区块数量为D/L。此时统计每个区块的包含数字的个数得到(N<sub>1</sub>,N<sub>2</sub>..N<sub>(D/L)</sub>)，向主机发送这个信息并接收其他所有从机的同类信息，可获得如下信息矩阵  

    从机1：(N<sub>1</sub>,N<sub>2</sub>..N<sub>(D/L)</sub>)  
    从机2：(N<sub>1</sub>,N<sub>2</sub>..N<sub>(D/L)</sub>)  
    .  
    .  
    .  
    从机100：(N<sub>1</sub>,N<sub>2</sub>..N<sub>(D/L)</sub>)   

对应列相加后得到  

    (TN<sub>1</sub>,TN<sub>2</sub>..TN<sub>(D/L)</sub>)

此时容易知道要求中位数为第Mid = (2G/4)/2 = 256M个，从TN数组的左侧开始做减法，即Mid -= TN<sub>k</sub>直到Mid小于0，即可知道中位数所在区间。  

如果该区间还是过大，则可递归进行上述过程，再次分块并求Mid' = Mid - ΣTN<sub>k</sub>，直到求出最后结果。

-------
<a name="Anchor2" id="Anchor2"></a>
-**Find frequencies of each IPs**([Back to Index](#AnchorIndex))   
给定100G条IP数据以及一张划分IP域的表，使用最快的方法统计出每个IP域的IP个数。  

IP由32位组成，共4G个，因此100G条IP必然存在重复，考虑使用一个MAP进行压缩，压缩比达到25:1。又因为划分IP域的表的是单调递增的，如果使用一个有序的MAP，则可以在遍历MAP的同时统计出所有结果。  

-------
<a name="Anchor3" id="Anchor=3"></a>
-**Bit-map**([Back to Index](#AnchorIndex))  

所谓的Bit-map就是用一个bit位来标记某个元素对应的Value，而Key即是该元素。由于采用了Bit为单位来存储数据，因此在存储空间方面，可以大大节省空间  
```cpp
//定义每个Byte中有8个Bit位
#include <memory.h>
#define BYTESIZE 8

void SetBit(char *p, int posi) {
    for (int i = 0; i < (posi / BYTESIZE); i++) {
        p++;
    }

    *p = *p | (0x01<<(posi % BYTESIZE)); //将该Bit位赋值1
    return;
}

void BitMapSortDemo() {
    //为了简单起见，我们不考虑负数
    int num[] = {3, 5, 2, 10, 6, 12, 8, 14, 9};

    //BufferLen这个值是根据待排序的数据中最大值确定的
    //待排序中的最大值是14，因此只需要2个Bytes(16个Bit)
    //就可以了。
    const int BufferLen = 2;
    char *pBuffer = new char[BufferLen];

    //要将所有的Bit位置为0，否则结果不可预知。
    memset(pBuffer, 0, BufferLen);
    for (int i = 0; i＜9; i++) {
        //首先将相应Bit位上置为1
        SetBit(pBuffer, num[i]);
    }

    //输出排序结果
    for (int i = 0; i<BufferLen; i++)//每次处理一个字节(Byte)
    {
        for (int j = 0; j<BYTESIZE; j++)//处理该字节中的每个Bit位
        {
            //判断该位上是否是1，进行输出，这里的判断比较笨。
            //首先得到该第j位的掩码（0x01＜＜j），将内存区中的
            //位和此掩码作与操作。最后判断掩码是否和处理后的
            //结果相同
            if ((*pBuffer & (0x01<<j)) == (0x01<<j)) {
                printf("%d ", i * BYTESIZE + j);
            }
        }
        pBuffer++;
    }
}

int main() {
    BitMapSortDemo();
    return 0;
}

```