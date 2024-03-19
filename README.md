# 前言
* Dian的学长，你们好呀，我是电信2311班的路文博，在大一上学期，我属于土木与水利工程学院土木类2305班，面对土木行业不景气的环境，且自身对土木学科没有一点兴趣，
我下定决心要转专业，并选择自己感兴趣的电子信息工程专业，奈何这个专业在每年转专业中全校只招收5个人，竞争激烈，因此我开始了没日没夜地学习微积分，延续高中
的作息和学习强度，我似乎忘记了我已经进入大学，买了台电脑，但平时除写C++作业外没有打开过，拙劣的打字技能和一无所知的电脑知识，我似乎跌入了一个信息茧房...
* 但是，来到电信学院后，在信息技术导论课上黑晓军老师的介绍下，我的得知了Dian团队。这里有很多优秀的学长，且在之前与我同级的其他学院的同学交流过程中，他们口
中一些令我感到陌生的名词不断冲击着我，让我深刻意识到自己与其他人之间的差距，激发了我的学习欲望，并且在此次Dian的初试过程中，嵌入式方向的任何一个东西都是
是我未曾接触过的，这一切都让我感到新颖，我很荣幸能参加此次初试，因为这让我突破了信息茧房!
*由于基础比较落后，从零开始接触这个领域，在曾宏学长的建议下，我写了这份markdown学习文档（也是现学的），记录我这几天的学习进度及心路历程，感谢Dian能给我这次机会！

# DAY 1
  # 学习科学上网，注册谷歌，创建Github仓库
在b站，知乎的各种参差不齐的教程中，我尝试了许多种方法（大多半途而废，要么收费，要么不能用）经过两天的总结和尝试学会了如何科学上网并创建了自己的谷歌账号，学习了
Github是干什么的，创建了自己的仓库。下面介绍我学习科学上网的流程：
* 1 下载软件clash for windows 
* 2 添加连接地址（一元机场）clashhttps://sub2.smallstrawberry.com/api/v1/client/subscribe?token=94c619d4e868805e1815ef8625495158
* 3 注册谷歌邮箱: 首先登录https://sms-activate.org/cn/freePrice#activation， 并在此网站注册自己的账号，充值2美元（约14元rmb），购买国外手机号(我买的England）用于注册谷歌邮箱时接受验证码
* 但此时我遇到一个问题：买的手机号大多已被注册过，或不能用，庆幸的是在几次购买后买到了能注册的手机号，并发现已经购买的手机号可以原价退款
* 4 用谷歌邮箱登陆GitHub，并创建自己的库，在此过程中了解到手机上也可直接用github软件，但国行版iPhone不能在AppStore上下载Github，只需购买外国Apple Id （davidchavezvct@hotmail.com ）
  在Applestore中切换id 即可搜索到国外软件如github ，X等。并使用shadowrocket连接vpn，然后我发现上述clash连接地址依然可以使用并且不用花任何费用！但手机上我没找到如何创建github库，只能搜索别人的远程链接并阅读
# DAY 2—4
  这几天课程较满，有实验课等，此段花费天数较多
  # 下载Visual Studio Code 配置ESP IDF开发环境 点灯
  * 1 配置vscode中文环境：进入vscode界面 在左侧栏搜索Chinese，点击install，然后重启vscode，即可显示中文
  * 2 下载ESP IDF组件，静待漫长的安装后，尝试点亮led灯来检查环境是否配置正确
  * 3 学习GPIO和Esp32s3的参数配置及主要组件
  * 4 学习点亮led灯的代码,检查开发环境是否配置成功（这一块耗费时间较长，主要是学习c语言代码）
  * 此处我碰到了许多的问题（这个地方是最耗时间的，因为有的问题很蹊跷）烧录的时候总是报错，首先是A fatal
  * error occurred: Invalid head of packet (0xXX):Possible serial noise or corruption.经过搜索得知可能
    是数据线的问题，换一根USB之后，还是报错，但错误不再是原来的那样，最后发现换数据线之后com口也要手动更变。
    经过一系列报错以及漫长的烧录等待，led灯总算亮了起来，不过我目前还有一个问题，就是led灯闪烁的周期与代码
    预期的要更长，我搜索过很多类似问题，都没有明确的答复。最后猜测可能是硬件问题，导致数据传输时有所延迟吧。
  * 点亮LED的代码（main中代码）  
```c
#include <stdio.h> //c语言中包含标准输入输出库的头文件
#include "freertos/FreeRTOS.h"//包含 FreeRTOS 实时操作系统的头文件
#include "freertos/task.h"//包含 FreeRTOS 任务处理的头文件
#include "esp_system.h"//包含 ESP 系统相关的头文件
#include "driver/gpio.h"//包含 GPIO 驱动程序的头文件

#define LED_GPIO 4//定义LED灯所连接的 GPIO 引脚号为 4（可根据需求随机定义通用引脚）

void app_main()//程序的入口函数，类似于 Arduino 中的 setup() 函数
{
    gpio_reset_pin(LED_GPIO);//将 LED_GPIO 引脚重置为默认状态


    gpio_set_direction(LED_GPIO, GPIO_MODE_OUTPUT);//设置 LED_GPIO 引脚为输出模式，
    //以便控制LED灯的亮灭。

    while(1) {
        gpio_set_level(LED_GPIO, 1);//将引脚设置为高电平，点亮
        vTaskDelay(1000);//vTaskDelay函数，单位为ms
        gpio_set_level(LED_GPIO, 0);//将引脚设置为低电平，熄灭
        vTaskDelay(1000);//再次延迟
    }
}
```
# DAY 5
  # 学习串口协议，向电脑发送Hello world
串口通信：使用发送线和接收线 用于两台设备之间进行通讯比较方便。
* 基本概念：
* 波特率（Baud Rate）：表示每秒传输的符号数，是串口通信中非常重要的一个参数。常见的波特率有9600、115200等。
* 数据位（Data Bits）：每个字符或符号所包含的位数，常见的有7位和8位。
* 停止位（Stop Bits）：用于标志字符的结束。
* 校验位（Parity Bit）：用于检验数据传输过程中是否出现错误。
* 起始位（Start Bit）：标志字符的开始。
* 主要内容：
* 帧格式：每个传输的字符或数据包都有固定的格式，通常包括起始位、数据位、校验位和停止位。
* 通信方式：串口通信可以是同步的（如USART同步异步均可以），也可以是异步的（如UART）。异步通信是串口通信中最常用的方式，
 它不需要双方时钟严格同步，数据的发送和接收通过起始位和停止位来界定字符的开始和结束。
* 代码实现(放在main中）
```c
#include <stdio.h> // c语言中包含标准输入输出库的头文件
#include <string.h> // 包含字符串处理库
#include <freertos/FreeRTOS.h> // 包含 FreeRTOS实时操作系统头文件
#include <freertos/task.h> // 包含 FreeRTOS 任务处理头文件
#include <driver/uart.h> // 包含 ESP32 UART 通信驱动程序的头文件

#define UART_NUM UART_NUM_0 // 定义使用的 UART 号码为 UART_NUM_0
#define TX_BUF_SIZE 1024 // 定义发送缓冲区大小为 1024 字节

void uart_task(void *pvParameters) { // 创建UART通信任务函数
    uart_port_t uart_num = UART_NUM; // 设置 UART 号码为定义的 UART_NUM
    uart_config_t uart_config = { // UART 配置信息结构体
        .baud_rate = 115200, // 设置波特率为 115200
        .data_bits = UART_DATA_8_BITS, // 数据位数为 8 位
        .parity = UART_PARITY_DISABLE, // 禁用校验位
        .stop_bits = UART_STOP_BITS_1, // 停止位数为 1
        .flow_ctrl = UART_HW_FLOWCTRL_DISABLE // 禁用硬件流控
    };
    uart_param_config(uart_num, &uart_config); // 配置 UART 参数
    uart_set_pin(uart_num, UART_PIN_NO_CHANGE, UART_PIN_NO_CHANGE, UART_PIN_NO_CHANGE, UART_PIN_NO_CHANGE); // 设置 UART 引脚
    uart_driver_install(uart_num, TX_BUF_SIZE, 0, 0, NULL, 0); // 安装 UART 驱动程序

    while (1) { // 循环
        uart_write_bytes(uart_num, "Hello World!\n", strlen("Hello World!\n")); // 发送 "Hello World!\n" 消息
        vTaskDelay(pdMS_TO_TICKS(1000)); // 延迟 1000 毫秒
    }
}

void app_main() { // 应用程序主函数
    xTaskCreate(uart_task, "uart_task", 4096, NULL, 5, NULL); // 调用创建的任务函数，任务名为 "uart_task"，栈大小为 4096 字节，优先级（介于0到255）为 5
}
```
# DAY 6
  # 学习IIC协议，读取MPU6050数据，学习markdown
IIC协议（Inter-Integrated Circuit）（主从模式），不同于串口通信，IIC使用时钟线（SCL）和数据线（SDA) ，空闲状态下SCL和SDA都保持高电平，下面是我自己的理解：
* 以主设备向从设备写入信息时数据帧为例：传递起始位置时，必须在时钟信号为高电平期间，通过数据信号完成由高到低的跳变即下降沿
* 然后先发送七位地址码来区分和那个从设备进行通信(每个设备地址码唯一，七位0或1的排列组合，共有128种结果）
* SCL始终保持高电平时，SDA始终保持高电平就完成了逻辑1地传输，若SDA始终保持低电平则表示逻辑0
* 接下来是读写数据位若要写入数据则将其置为0，读取数据置为1
* 紧接着为应答信号（由从机向主机发布），从机接收到则回复0，若未接收或读取已完成则回复1
* 再下面8位是设备寄存器地址（最多访问256字节）然后主设备接收从设备返回的应答信号
* 再下面8位是要写入存储器的寄存器数据，然后从机再次应答最后写入停止位（与起始位置相反，SCL高电平SDA由低到高）
* 读取数据与之类似，只是在读取寄存器地址后多读一遍设备地址 下面是示意图片（上方为读数据，下面为写数据）
 ![](https://github.com/xxxlwb/Dian/blob/main/Dian/%E8%AF%BB%E5%86%99%E6%95%B0%E6%8D%AE.jpg)
```c
//此部分代码并没有学到，我并未成功读取mpu6050的数据，还请学长见谅呀！
```
# DAY 7
  # 学会运用Markdown，写此份文档（全文手工敲打）
  # 总结反思
* 时光荏苒，回想起这7天的学习，我仿佛做了一场匆匆忙忙的梦，恍恍惚惚......
* 这几天学到了很多以前没接触过的东西比如科学上网（虽然不在要求范围内，但是学了，所以想说一下）， 它可能是我们经常挂在耳边的一个词，但我平常并没有认真去实现它，然而当我真正
去做的时候发现它并不是很简单的一件事。
   还有GitHub，学习markdown，vscode的使用等等，我认为这些并不是只能用于此次考核，掌握它们，对我以后的学习生活都有极大的帮助，Github能为我以后的编程学习提供丰富的资源。
* 在此次考核中，我认为学到的最重要的一点是它磨练了我的毅力，让我从课堂之外，书本之外的层次上深刻地审视自己，再一次让我认识到了高中学习于大学本质上的不同。同时我逐渐体会到做项目（科研)
  的路程不可能是一帆风顺的，道路很坎坷，我会遇到各种始料不及的问题，说实话在此次考核过程中也曾想过放弃，但心里还是很不甘，我很向往Dian团队，Dian团队“优秀是一种习惯”的团训也是我在学习中
  的追求，为了实现这个梦想，我未曾放弃，就像雷军说的那样，咬咬牙，再试一试，万一实现了呢!就这样，我坚持到了现在。
* 经过这几天的快速学习,我逐渐了解到了一些简单的嵌入式开发工作，具体体会到了硬件之间是怎样进行通信的，我仿佛打开了一个奇妙而又有趣的斑斓世界！但我深知自己相比于同级人关于代码基础可能更差一些，
  这也让我“知不足而奋进”！
* 最后感谢Dian的学长抽出宝贵的空闲时间来垂阅我的学习文档，希望能加入Dian团队跟学长们学习，谢谢！


