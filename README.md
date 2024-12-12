
# Malbolge


除了我们日常使用的Python、Java、C等主流编程语言外，还存在这么一类极为晦涩难懂的编程语言，被称为**深奥的编程语言**（**Esoteric programming language**，简称**Esolang**）。它们被设计用于测试计算机语言表达的极限，所以不会考虑它们的实用性。今天我们来看其中一个非常典型的例子：Malbolge。


Malbolge由Ben Olmstead 在1998年发明,其名字来自于但丁的《神曲》中的第八层地狱“Malebolge”，意大利语中意为“邪恶的沟渠”（male bolge）。


![请添加图片描述](https://i-blog.csdnimg.cn/direct/cf3941445792493085dcb3f7a55fe8a5.jpeg)


# Hello World


下面这段Malbolge代码会输出“Hello, World.”



```
(=<`#9]~6ZY327Uv4-QsqpMn&+Ij"'E%e{Ab~w=_:]Kw%o44Uqp0/Q?xNvL:`H%c#DD2^WV>gY;dts76qKJImZkj

```

而这段代码则会输出“Hello, World!”



```
('&%:9]!~}|z2Vxwv-,POqponl$Hjihf|B@@>,=
```

我们可以看到，光是一个标点符号的改变，就导致代码发生了天翻地覆的变化。


# CTF


大多数涉及 Malbolge 的基础 CTF 题目会提供一段看似乱码的内容。此时，需要识别出这实际上是 Malbolge 代码，并通过编译器将其编译出来。


比如说这段代码会输出“flag{this\_is\_a\_flag}”



```
D'`;qLo=I;|XyhCwStcr=NL-,I$)"XW21A/c>,v_)\xqYonsrqj0hPlkdcb(`Hd]#a`_A@VzZY;QuUTSRQJImGLEJIBAeED&B;_9>7<;:921U54ts10)M'&Jkj"F&%|#z@~}vu;y[Zvo5Vlkjongf,Miha'Hd]\[ZY}W?UZYRQuU7SLp]

```

![请添加图片描述](https://i-blog.csdnimg.cn/direct/301615641d4c47c5930c0c7c42360993.png)
有些题目可能需要对编译后的结果再进行一次处理，比如使用 base64 解码等。
但是这些内容都不需要了解任何 Malbolge 语言的特性，只需要找到一个编译器即可。因此，接下来我会介绍一些 Malbolge 的特点，并分享一道较难的 CTF 比赛真题。


# Malbolge的特点


首先，Malbolge会使用 三个寄存器（register），分别是 **a**、**c** 和 **d**。
**a**：Accumulator，主要用于存储计算结果和输入/输出数据。
**c**：Code Pointer / Instruction Pointer，用于指向当前正在执行的代码的位置（即指令指针）。
**d**：Data Pointer，用于指向内存中的数据位置。


其次，基于这些寄存器，Malbolge提供了八条指令，分别为：**jmp**，**out**，**in**，**rotr**，**mov**，**crz**，**nop**，**end**。我们可以利用某些在线编译网站提供的规范化（Normalization）功能，将代码转换为由一组固定字符组成的指令字符集，从而更方便地进行调试（debug）。


更多详细信息可以参考
[https://en.wikipedia.org/wiki/Malbolge](https://github.com)


# 例题


题目来源：Platypwn 2024 CTF
链接：[https://platypwn.ctf.platypwnies.de/](https://github.com)
题面：
![请添加图片描述](https://i-blog.csdnimg.cn/direct/58803e7bdbb84aa5a882e1cbc6f4c6e8.png)
下载下来的文件内容为：



```
D'`__LK!mY:jiy6Be3cPa)onKI[#j4&DUBzcx>_;):'Zputml21onPlkd*hJIedcb[!YXW{[TYXWVUNrRQPImGFEDIBfFED=<;:?8\<;432V65ut,P*/('&J*j(!~}C#cy~}vb<$:?876ZG

```

这段内容看似是乱码，但根据题面的提示“the worst of esoteric programming languages” （正如前文所提到的），我们可以判断出它实际上是一段Malbolge的代码。
直接编译这段代码无法得到任何结果，因此我们可以选择使用某个在线编译工具对其进行规范化处理，使代码转换为更标准的指令字符集，从而便于调试（debug）：
![请添加图片描述](https://i-blog.csdnimg.cn/direct/f4c35f8eddc34b4fab0f5365f09599f7.png)


![请添加图片描述](https://i-blog.csdnimg.cn/direct/2a54253657cd4870ba65cf2f81d63ab7.png)


normalized 之后的结果为：



```
ojii
```

当我们运行这段代码时，会发现它在执行到中间某处时意外停止了：


![请添加图片描述](https://i-blog.csdnimg.cn/direct/7d98c6132c6e4e5b97850be300a0d883.png)


大概是在这个位置：
![请添加图片描述](https://i-blog.csdnimg.cn/direct/d3416f57b9a8468b9954796df9a1dad8.png)
所以我们猜测（尝试）需要将这一项改成其他的命令。
而在将其改正为p了之后会得到：
![请添加图片描述](https://i-blog.csdnimg.cn/direct/8345fb2959614662a76faafc2aa50472.png)


订正后的内容



```
ojii
```


```
D'`__LK!mY:jiy6Be3cPa)onKI[#j4&DUBzcx>_;):'Zputml21onPlkd*hJIedcb[!YXW{[TYXWVUNrRQPImGFEDIBfFED=<;:?8\<;432V65ut,P*/('&J*j(!~}C#cy~}vb<$:?876ZG

```

具体改动：
![请添加图片描述](https://i-blog.csdnimg.cn/direct/7eda6e29496441799434e88fbfb6df53.png)
这样一来我们就成功获取到了flag。


## 其他办法


### 1\. 暴力破解


当然，如果我们知道（或猜测）这段代码中只有一个地方存在问题，可以尝试使用暴力破解（brute force）的方法进行修正。这种方法的核心是将每条指令逐一修改为其他可能的指令，并观察编译结果。由于不需要理解 Malbolge 的具体特性，因此这种方法非常简单。


为实现这一目标，我们可以利用支持在线编译 Malbolge 的网站，以及 Python 中的 Selenium 库。Selenium 提供了浏览器自动化操作功能，能够帮助我们完成网页上的勾选、输入操作，并提取输出内容，从而实现自动化调试。



```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

# 初始化浏览器（确保你有对应的浏览器驱动）
# driver = webdriver.Chrome()  # Chrome
driver = webdriver.Firefox()  # Firefox
try:
    # 打开目标网页
    driver.get("https://lutter.cc/malbolge/debugger.html")

    # 等待页面加载
    wait = WebDriverWait(driver, 30)

    # 勾选所有复选框
    checkboxes_ids = ['until_in', 'until_out', 'until_crz', 'until_rotr', 'until_jmp', 'until_mov', 'until_nop']
    for checkbox_id in checkboxes_ids:
        checkbox = wait.until(EC.element_to_be_clickable((By.ID, checkbox_id)))
        if not checkbox.is_selected():
            checkbox.click()

    # 勾选Normalized
    normalized_checkbox = wait.until(EC.element_to_be_clickable((By.ID, 'normalizedcode')))
    if not normalized_checkbox.is_selected():
        normalized_checkbox.click()

    # 原始Malbolge代码
    original_code = "ojii

    # 替换的字符列表
    replace_chars = ['i', 'j', 'o', 'p', '/', '*']

    # 标志变量，用于退出循环
    found_flag = False

    # 循环替换并执行代码
    for i in range(1, len(original_code)):
        if found_flag:  # 如果找到结果，退出外层循环
            break
        for char in replace_chars:
            modified_code = list(original_code)
            modified_code[i] = char  # 替换第i个字符
            modified_code = ''.join(modified_code)


            # 输入修改后的Malbolge代码
            program_textarea = driver.find_element(By.ID, "program")
            program_textarea.clear()
            program_textarea.send_keys(modified_code)

            # 点击Load/Reset按钮
            load_button = driver.find_element(By.ID, "load")
            load_button.click()

            # 点击Execute按钮
            execute_button = wait.until(EC.element_to_be_clickable((By.ID, "run")))
            execute_button.click()

            # 等待执行完成并获取Output内容
            time.sleep(1)  # 根据需要调整等待时间
            output_div = driver.find_element(By.ID, "output")
            output_content = output_div.text

            # 仅当output内容包含"flag"时打印结果
            if "pp{" in output_content.lower():
                print(f"将第 {i + 1} 位修改成 '{char}' 后成功编译出flag。")
                print(f"编译成功的代码: {modified_code}")
                found_flag = True  # 设置标志变量，标记已找到结果
                break  # 退出内层循环

finally:
    # 关闭浏览器
    driver.quit()

# 运行成功后会得到：
"""将第 43 位修改成 'p' 后成功编译出flag。
编译成功的代码: ojii

```

### 2\. 修改解释器源代码


有一位参赛选手分享了一个非常巧妙的解法，具体如下：
首先他找到了一个用 C 语言编写的原始 Malbolge 解释器（[https://github.com/bipinu/malbolge）。接着，他将第](https://github.com) 131 行的return 改为 break，以避免 exec() 函数提前结束。
最后，他使用修改后的解释器运行题目中提供的 Malbolge 代码，成功得到了 flag。


# 工具（网站）


最后附上一些网页，可以用来生成，编译，或者debug。


[https://lutter.cc/malbolge/debugger.html](https://github.com):[FlowerCloud机场节点订阅](https://dahelaoshi.com)
![请添加图片描述](https://i-blog.csdnimg.cn/direct/fedb0d16cf184f2999aa3fa08f4195af.png)


[https://zb3\.me/malbolge\-tools/\#generator](https://github.com)
![请添加图片描述](https://i-blog.csdnimg.cn/direct/2d70ded4500f42e1872bbeedfdd05cda.png)


[https://tio.run/\#\#y03MScrPSU/9/19DXU3VyjJWsa62psoorKK8TFcnwL@wID8vR8UjKzMjrcbJwcFOx9bG18pSzTxSOSzMKCQ4T88/VSvZWsNTTVVFWSnX2cnRvqqiokwrIMkioTDfxCg3yk2v2rNSyyHFRd3GOt5eMaYWYnpwcOD//wA](https://github.com)
![请添加图片描述](https://i-blog.csdnimg.cn/direct/60f456dd957440d5adee8753c48ad361.png)


