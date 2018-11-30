####**指令中符号解释：R代表寄存器  　Ｍ代表内存单元　Ｉ代表立即数**
*** 
1. 运算指令:
 ADD  R/M , R/M/I   &emsp;&emsp; 加法指令(减法类似)
 XCHG R/M ,R          &emsp;&emsp;  两数交换指令
 INC   R/M &emsp;&emsp;自增指令(自减类似)
 CMP R/M, R/M/I  &emsp;&emsp; 比较指令
 NEG R/M  &emsp;&emsp; 求补指令
 MUL  R/M &emsp;&emsp; 无符号乘法指令
 DIV R/M &emsp; &emsp; 无符号除法指令
 IMUL R/M &emsp;&emsp; 有符号乘法（有符号除法IDIV）
 ***
  2.逻辑指令
    NOT R/M &emsp; &emsp; 取反指令
   AND R/M，R/M/I &emsp;&emsp; 与指令（或指令类似OR）
  TEST（类似于AND 指令只是没有存放结果的AND指令）
  XOR R/M,R/M/I &emsp;&emsp; 异或指令
  ***
  3.跳转指令
  JMP R/M/I
  跳转指令分为短跳转与长跳转，其中短跳转分为NEAR 与SHORT， 其中NEAR的跳转范围为-32K~32K-1，SHORT跳转长度为 -128~127

**附加**

关于跳转指令中的条件跳转指令
对于条件跳转指令首先要弄清几个关键的单词首字母，其实条件跳转指令就是按照指令的中文意思执行的。
E （equal 相等）
N（not 不）
用于无符号的两个常见单词首字母
A（above 大于）
B （below 小于 ）
用于有符号的两个常见单词首字母
G（greater 大于）
L（less 小于）

JA/JNBE &emsp; 比较结果大于时跳转 （CF=0 且 ZF=0）
JAE/JNB &emsp; 大于等于    （CF =0 或 ZF =1）
JB/JNAE  &emsp; 小于 （CF=1 且 ZF=0）
JBE/JNA &emsp; 小于等于 （CF= 1 或 ZF = 1）

JG/JNLE &emsp; 比较结果大于时跳转 （SF=OF 且 ZF=0）
JGE/JNL &emsp; 大于等于    （SF=OF  或 ZF =1）
JL/JNGE  &emsp; 小于 （SF！=OF 且 ZF=0）
JLE/JNG&emsp; 小于等于 （SF！=OF 或 ZF = 1）

JC ,JNC,JZ/E,JNZ/E ,JS,JNS 等指令都是更具标志为来进行判断并进行转移   
 

