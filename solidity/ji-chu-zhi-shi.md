# 基础知识

### 前言 <a href="#undefined" id="undefined"></a>

外网高赞web3视频教程 solidity内容Lesson0-lesson7

{% embed url="https://www.youtube.com/watch?v=gyMwXuJrbJQ&t=14371s" %}

&#x20;此教程纯英，为了降低学习难度，建议大家先过一遍基础知识，跟着视频一点点码。

### 类型

memory 　

view是啥

token的概念是什么

### 函数

函数输出：**命令式返回**和**解构式赋值**

我们可以在`returns`中标明返回变量的名称，这样`solidity`会自动给这些变量初始化，并且自动返回这些函数的值，不需要加`return`。

```
    // 命名式返回
    function returnNamed() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array){
        _number = 2;
        _bool = false; 
        _array = [uint256(3),2,1];
    }
```

在上面的代码中，我们用`returns(uint256 _number, bool _bool, uint256[3] memory _array)`声明了返回变量类型以及变量名。这样，我们在主体中只需要给变量`_number`，`_bool`和`_array`赋值就可以自动返回了。

当然，你也可以在命名式返回中用`return`来返回变量：

```solidity
// 命名式返回，依然支持return
function returnNamed2() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array){
    return(1, true, [uint256(1),2,5]);
}
```

`solidity`使用解构式赋值的规则，支持读取函数的全部或部分返回值。

* 读取所有返回值：声明变量，并且将要赋值的变量用`,`隔开，按顺序排列。

```solidity
uint256 _number;
bool _bool;
uint256[3] memory _array;
(_number, _bool, _array) = returnNamed();
```

* 读取部分返回值：声明要读取的返回值对应的变量，不读取的留空。下面这段代码中，我们只读取`_bool`，而不读取返回的`_number`和`_array`：

```solidity
(, _bool2, ) = returnNamed();
```

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

### 控制流

1.  需要注意solidity中没有负数，需要控制，看下下面的例子



    ```solidity
     function insertionSortWrong(uint[] memory a) public pure returns(uint[] memory) {
            
            for (uint i = 1;i < a.length;i++){
                uint temp = a[i];
                uint j=i-1;
                while( (j >= 0) && (temp < a[j])){
                    a[j+1] = a[j];
                    j--;
                }
                a[j+1] = temp;
            }
            return(a);
        }
    ```

`solidity`中最常用的变量类型是`uint`，也就是正整数，取到负值的话，会报`underflow`错误。而在插入算法中，变量`j`有可能会取到`-1`，引起报错。

1. &#x20;solidity中不支持死循环，支持continue和break中断循环

### 构造函数与修饰器物

### 事件Event

> event是EVM上日志的抽象，俩个特点，可以通过rpc订阅和监听，事件可以在存储数据，且gas消耗低

```solidity
event Transfer(address indexed from,address indexed ot,unit256 value)
```

线上工具：[https://emn178.github.io/online-tools/keccak\_256.html](https://emn178.github.io/online-tools/keccak\_256.html)

### 继承【待补充】

`关键字`**`is`**

**`多重继承`**&#x20;

### 抽象合约和接口

### 异常

### 函数重载

solidity不允许装饰器重载

重载需要判断参数匹配，如果参数类型一致，会报错，unit8和unit256相同参数也会报错

```solidity
function f(unit8 a) public(){}
function f(unit256 a) public(){}
//会报错
```

### 库合约

和普通合约的不同的点：

1. 不能存在状态变量
2. 不能继承
3. 不能收币
4. 不能销毁

如何使用 `using A for B; 从库A到类型B，`

```solidity
using Strings for unit256
//使这个类型具备第三方的方法，去调用
function getString(unit256 _number) public pure returns(string memory){
    return _number.toHexString();
}
```

### Import

* 通过源文件相对位置导入，例子：

```solidity
文件结构
├── Import.sol
└── Yeye.sol

// 通过文件相对位置import
import './Yeye.sol';
```

* 通过源文件网址导入网上的合约，例子：

```solidity
// 通过网址引用
import 'https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Address.sol';
```

* 通过`npm`的目录导入，例子：

```solidity
import '@openzeppelin/contracts/access/Ownable.sol';
```

* 通过`全局符号`导入特定的合约， 可以参考ES6 导入，例子：

```solidity
import {Yeye} from './Yeye.sol';
import {Yeye as grandFather } from './Yeye.sol';
```

* 引用(`import`)在代码中的位置为：在声明版本号之后，在其余代码之前。

`import 特别点的：`{}里可以拿到其他合约中人和可读的变量

```solidity
//father.sol
struct Fathers{
    string name;
}
function say() pure returns(string memory){
    return "i`m your father"
}
contract Father{

}
//son.sol
import {Father,say,Father} from "./father.sol"

```

### receive和fallback的区别[​](https://www.wtf.academy/docs/solidity-102/Fallback/#receive%E5%92%8Cfallback%E7%9A%84%E5%8C%BA%E5%88%AB) <a href="#receive-he-fallback-de-qu-bie" id="receive-he-fallback-de-qu-bie"></a>

`receive`和`fallback`都能够用于接收`ETH`，他们触发的规则如下：

```
触发fallback() 还是 receive()?
           接收ETH
              |
         msg.data是空？
            /  \
          是    否
          /      \
receive()存在?   fallback()
        / \
       是  否
      /     \
receive()   fallback()
```

简单来说，合约接收`ETH`时，`msg.data`为空且存在`receive()`时，会触发`receive()`；`msg.data`不为空或不存在`receive()`时，会触发`fallback()`，此时`fallback()`必须为`payable`。

`receive()`和`payable fallback()`均不存在的时候，向合约**直接**发送`ETH`将会报错（你仍可以通过带有`payable`的函数向合约发送`ETH`）。

### 发送ETH

transfer、send、call都可以发送eth

**相同点 ：**

&#x20;都可以在合约之间相互转账，send和call都有返回值。需要在代码中判断返回值。

* addr.transfer(1 ether)、addr.send(1 ether)、addr.call.value(1 ether)的接收方都是addr。
* &#x20;如果使用addr.transfer(1 ether)、addr.send(1 ether)，addr合约中必须增加fallback回退函数！&#x20;
* 如果使用addr.call.value(1 ether)，那么被调用的方法必须添加payable修饰符，否则转账失败！&#x20;

**不同点：**

1. transfer&#x20;
   1. 如果异常会转账失败，抛出异常(等价于require(send()))（合约地址转账）&#x20;
   2. 有gas限制，最大2300&#x20;
   3. 函数原型：.transfer(uint256 amount)
2. send&#x20;
   1. 如果异常会转账失败，仅会返回false，不会终止执行（合约地址转账）&#x20;
   2. 有gas限制，最大2300&#x20;
   3. 函数原型：.send(uint256 amount) returns (bool)
3. call&#x20;
   1. 如果异常会转账失败，仅会返回false，不会终止执行（调用合约的方法并转账）&#x20;
   2. 没有gas限制
   3. 函数原型：.call(bytes memory) returns (bool, bytes memory)&#x20;

### 如何调用其他合约

````solidity
```remix-solidity
contract callContract{
    function setOneX(address _address,uint256 x)  external  payable {
        OtherContract(_address).setX(x);
    }

    function setTwoX(OtherContract _address,uint256 x)  external  payable {
        _address.setX(x);
    }

     function setThreeX(address _address,uint256 x)  external  payable {
        OtherContract oc = OtherContract(_address);
        oc.setX(x);
    }   
    //传钱进去固定为{}
    function setFourX(address _address, uint256 x) external  payable {
        OtherContract(_address).setX{value: msg.value}(x);
    }
}
```
````

Q:这个调用方式跟call有啥区别？

A: 下面三个低层的call会返回bytes和success

而直接调用会根据interface做返回，失败直接revert

* `targetAddr.call(bytes memory abiEncodeData) returns (bool, bytes memory)`
* `targetAddr.delegatecall(bytes memory abiEncodeData) returns (bool, bytes memory)`
* `targetAddr.staticcall(bytes memory abiEncodeData) returns (bool, bytes memory)`

call 改变的是被调用的storage

dilegateCall 改变的是调用的storage&#x20;

`call` 是常规调用，`delegatecall` 为委托调用，`staticcall` 是静态调用（不修改合约状态， 相当于调用 `view` 方法）。

调用和被调用的合约数据结构和顺序必须一致



