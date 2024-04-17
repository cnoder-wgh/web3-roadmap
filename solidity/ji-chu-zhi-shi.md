# 基础知识

### 前言 <a href="#undefined" id="undefined"></a>

外网高赞web3视频教程 solidity内容Lesson0-lesson7

{% embed url="https://www.youtube.com/watch?v=gyMwXuJrbJQ&t=14371s" %}

&#x20;此教程纯英，为了降低学习难度，建议大家先过一遍基础知识，跟着视频一点点码。

### 类型

memory 　

view是啥

### 函数

函数输出：**命令式返回**和**结构式赋值**

函数输入重点知识点：address **payable**

extral 外部可见内部不可见

internal 内部可见外部不可见

returns 和return 应用

### 变量数据存储和作用

引用类型（Reference Type）：数组，结构体struct，mapping ，string，所以这些需要声明数据存储的位置 那么什么是数据存储的位置 :tada:&#x20;

**数据存储的位置**

solidity 有三个，**storage** ，**memory**, **calldata**

storage 类似硬盘且上链，gas消耗高，memory和calldata 类似内存，gas消耗少

* storage: 合约里的状态变量默认都是storage
* memory： 函数里的参数和临时变量一般在用memory ，在内存中，不上链
* calldata： 和memory不同的点在于calldata 变量不能修改

storage赋值给memory，会创建独立的副本，所以修改memory不会影响到链上的数据

memory 赋值给memory，就会改变

### 作用域

**状态变量**

状态变量就是存储在链上的数据，所有合约内的函数都可以访问，gas消耗高

**局部变量**

函数体的变量，存在内存，不上链路，gas，函数退出后就回收了

**全局变量**

全局变量是全局范围工作的变量，不需要声明直接获取的变量或者函数，下面列举的红色区块为常用

* blockhash(uint blockNumber):(bytes32)指定区块的哈希值
* block.coinbase:(address  payable) 当前区块矿工地址
* block.gaslimit:(unint) 当前区块的gaslimit
* block.number:(unit) 当前区块的number
* <mark style="color:red;">block.timestamp:(unit)当前区块的时间戳</mark>
* gasleft():(unit256) 剩余gas
* msg.data:(btyes calldata)
* <mark style="color:red;">msg.sender:(address payable) 消息者</mark>
* msg.sig:(bytes4),calldata的前4位
* msg.value:(unit)当前交易发送的**wei**值

### 引用类型

数组：bytes是byte的array，其他后面加上【】

固定长度的数组不能用push只用array\[0]=xxx这样赋值

### Mapping

没有什么特别的参考js的Map

## 变量初始值

**delete 不是清空，是变成初始值‼️**

### 常数

constant【常量】和immutable【 不可变值】

区别：constant 修饰的变量需要在**编译期确定值**, 链上不会为这个变量分配存储空间, 它会在编译时用具体的值替代, 因此, constant常量是不支持使用运行时状态赋值的(例如: `block.number` , `now` , `msg.sender` 等 )immutable 修饰的变量是在**部署的时候确定变量的值**, 它在构造函数中赋值一次之后,就不在改变, 这是一个运行时赋值, 就可以解除之前 constant 不支持使用运行时状态赋值的限制.

immutable不可变量同样不会占用状态变量存储空间, 在部署时,变量的值会被追加的运行时字节码中, 因此它**比使用状态变量便宜的多**, 同样带来了更多的安全性(确保了这个值无法在修改).

\




