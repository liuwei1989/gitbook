<!-- TOC -->

- [什么是 TDD](#什么是-tdd)
- [为什么要 TDD](#为什么要-tdd)
  - [TDD 编码方式](#tdd-编码方式)
  - [TDD 好处](#tdd-好处)
- [如何 TDD](#如何-tdd)
  - [TDD 规则](#tdd-规则)
- [实例](#实例)
- [idea 测试驱动开发](#idea-测试驱动开发)

<!-- /TOC -->

[深度解读 - TDD（测试驱动开发）](https://www.jianshu.com/p/62f16cd4fef3)

# 什么是 TDD

狭义上的 TDD，也就是「单元测试驱动开发」。

> TDD 是敏捷开发中的一项核心实践和技术，也是一种设计方法论。TDD 的原理是在开发功能代码之前，先编写单元测试用例代码，测试代码确定需要编写什么产品代码。

TDD 的三层含义：

- Test-Driven Development，测试驱动开发。
- Task-Driven Development，任务驱动开发，要对问题进行分析并进行任务分解。
- Test-Driven Design，测试保护下的设计改善。TDD 并不能直接提高设计能力，它只是给你更多机会和保障去改善设计。

# 为什么要 TDD

## TDD 编码方式

**TDD 编码方式**

- 先分解任务，分离关注点
- 列 Example，用实例化需求，澄清需求细节
- 写测试，只关注需求，程序的输入输出，不关心中间过程
- 写实现，不考虑别的需求，用最简单的方式满足当前这个小需求即可
- 重构，用手法消除代码里的坏味道
- 写完，手动测试一下，基本没什么问题，有问题补个用例，修复
- 转测试，小问题，补用例，修复
- 代码整洁且用例齐全，信心满满地提交

**基本流程**

- 写一个测试用例
- 运行测试
- 写刚好能让测试通过的实现
- 运行测试
- 识别坏味道，用手法修改代码
- 运行测试

## TDD 好处

- 降低开发者负担
- 对产品代码提供了一个保护网，让我们可以轻松地迎接需求变化或改善代码的设计。
- 提前澄清需求  
  先写测试可以帮助我们去思考需求，并提前澄清需求细节
- 快速反馈

# 如何 TDD

## TDD 规则

**三条规则**

1. 除非是为了使一个失败的 unit test 通过，否则不允许编写任何产品代码。

   先编写了产品代码，则不能确定这段代码具体是为了实现什么需求，也不能确保它实现的需求真的实现了

2. 在一个单元测试中，只允许编写刚好能够导致失败的内容（编译错误也算失败）

   写了多个失败的测试，如果测试长时间不能通过，会增加开发者的压力，另外，测试可能会被重构，这时会增加测试的修改成本。

3. 只允许编写刚好能够使一个失败的 unit test 通过的产品代码。

   产品代码实现了超出当前测试的功能，那么这部分代码就没有测试的保护，不知道是否正确，需要手工测试。可能这是不存在的需求，那就凭空增加了代码的复杂性。如果是存在的需求，那后面的测试写出来就会直接通过，破坏了 TDD 的节奏感。

**本质**

**分离关注点**，一次只戴一顶帽子。

在我们编程的过程中，有三个关注点：**需求，实现，设计**。

- 红：写一个失败的测试，它是对一个小需求的描述，只需要关心输入输出，这个时候根本不用关心如何实现。
- 绿：专注在用最快的方式实现当前这个小需求，不用关心其他需求，也不要管代码的质量多么惨不忍睹。
- 重构：既不用思考需求，也没有实现的压力，只需要找出代码中的坏味道，并用一个手法消除它，让代码变成整洁的代码。

  ![image](https://upload-images.jianshu.io/upload_images/279826-49f2f708aefb567f?imageMogr2/auto-orient/)

# 实例

写一个程序来计算一个文本文件 words.txt 中每个单词出现的频率。

新手拿到这样的需求呢，就会把所有代码写到一个 main() 方法里，伪代码如下：

```c++
main() {
    // 读取文件
    ...
    // 分隔单词
    ...
    // 分组
    ...
    // 倒序排序
    ...
    // 拼接字符串
    ...
    // 打印
    ...
}
```

这样的代码存在什么样的问题呢?

- 不可测试
- 不可重用
- 难以定位问题

演化，有注释的地方都可以抽取方法，用方法名来代替注释：

```c++
main() {
    String words = read_file('words.txt')
    String[] wordArray = split(words)
    Map<String, Integer> frequency = group(wordArray)
    sort(frequency)
    String output = format(frequency)
    print(output)
}
```

**不做提前设计，选一个最简单的用例，直接开写，用最简单的代码通过测试。逐渐增加测试，让代码变复杂，用重构来驱动出设计**

大概可以想到如下的用例：

- "" => "" ，文件内容为空，输出也为空。
- "he" => "he 1"，一个单词，驱动出格式化字符串的代码
- "he is" => "he 1\r\nis 1"，两个不同单词，驱动出分割单词的代码
- "he he is" => "he 2\r\nis 1"，有相同单词，驱动出分组代码
- "he is is" => "is 2\r\nhe 1"，驱动出分组后的排序代码
- "he is" => "he 1\r\nis 1"，多个空格，完善分割单词的代码

有了重构这个工具后，做设计的压力小了很多，因为有测试代码保护，我们可以随时重构实现了。但这并不代表我们不需要做提前设计了，提前设计可以让我们可以和他人讨论，可以先迭代几次再开始写代码，在纸上迭代总比改代码要快。

更理想的设计：

```c++
main() {
    String words = read_file('words.txt')
    String output = word_frequency(words)
    print(output)
}

word_frequency(words) {
    String[] wordArray = split(words)
    Map<String, Integer> frequency = group(wordArray)
    sort(frequency)
    return format(frequency)
}
```

# idea 测试驱动开发
