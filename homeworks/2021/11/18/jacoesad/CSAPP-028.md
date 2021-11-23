> ***2021.11.18\***

------

## 3.6 Control

机器代码提供两种基本的低级机制来实现有条件的行为：测试数据值，然后根据测试结果改变控制流或者数据流。

用jump指令可以改变代码指令的执行顺序，jump指令指定控制应该被传递到程序的某个其他部分，可能依赖于某个测试的结果。

### 3.6.1 Condition Codes

条件码（condition code）寄存器，它们用来描述最近的算数或者逻辑操作的属性。可以检测这这些寄存器来执行条件分支指令。常用的有：

- CF-进位标志：最近的操作使最高位产生了进位。可以用来检查无符号数的溢出操作。
- ZF-零标志：最近的操作得出的结果位0。
- SF-符号标志：最近的操作得到的结果为负数。
- OF-溢出标志：最近的操作导致一个补码溢出，正溢出或者负溢出。

对于整数算数操作，一般都会设置条件码：

- `leaq`，不改变任何条件码
- XOR，进位标志和溢出标志会设置位0
- 位移操作，进位标志会被设置位最后一个移出的位，溢出标志设位0
- INC和DEC会设置溢出和零标志，但是不会改变进位标志。

只设置条件码的命令：

- ```
  CMP S1,S2
  ```

  ：基于S2-S1

  - `cmpb`
  - `cmpw`
  - `cmpl`
  - `cmpq`

- ```
  TEST S1,S2
  ```

  ：基于S1&S2

  - `testb`
  - `testw`
  - `testl`
  - `testq`

### 3.6.2 Accessing the Condition Codes

条件码使用的三种方式：

1. 根据条件码的某种组合，将一个字节设为0或者1；
2. 可以跳转到程序的某个地方；
3. 可以有条件的传输数据。