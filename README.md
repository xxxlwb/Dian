# 前言
Dian的学长，你们好呀，我是电信2311班的路文博，在大一上学期，我属于土木与水利工程学院土木类2305班，面对土木行业不景气的环境，且自身对土木学科没有一点兴趣，
我下定决心要转专业，并选择自己感兴趣的电子信息工程专业，奈何这个专业在每年转专业中全校只招收5个人，竞争激烈，因此我开始了没日没夜地学习微积分，延续高中
的作息和学习强度，我似乎忘记了我已经进入大学，买了台电脑，但平时除写C++作业外没有打开过，拙劣的打字技能和一无所知的快捷键，我似乎跌入了一个信息茧房...
但是，来到电信学院后，在信息技术导论课上黑晓军老师的介绍下，我的得知了Dian团队，这里有很多优秀的学长，且在之前与我同级的其他学院的同学交流过程中，他们口
中一些令我感到陌生的名词不断冲击着我，让我深刻意识到自己与其他人之间的差距，激发了我的学习欲望，并且在此次Dian的初试过程中，嵌入式方向的任何一个东西都是
是我未曾接触过的，这一切都让我感到新颖，我很荣幸能参加此次初试，因为这让我突破了信息茧房!
由于基础比较落后，从零开始接触这个领域，在曾宏学长的建议下，我写了这份markdown学习文档（也是现学的），记录我这几天的学习进度及心路历程，感谢Dian能给我这次机会！

# DAY 1-2
# 学习科学上网，注册谷歌，创建github仓库
在b站，知乎的各种参差不齐的教程中，我尝试了许多种方法（大多半途而废，要么收费，要么不能用）经过两天的总结和尝试学会了如何科学上网并创建了自己的谷歌账号，学习了github是干什么的，创建了自己的仓库。下面介绍我学习科学上网的流程：
* 1 下载软件clash for windows 
* 2 添加连接地址（一元机场）clashhttps://sub2.smallstrawberry.com/api/v1/client/subscribe?token=94c619d4e868805e1815ef8625495158
* 3 注册谷歌邮箱: 首先登录https://sms-activate.org/cn/freePrice#activation， 并在此网站注册自己的账号，充值2美元（约14元rmb），购买国外手机号(我买的England）用于注册谷歌邮箱时接受验证码。
* 但此时我遇到一个问题：买的手机号大多已被注册过，或不能用，庆幸的是在几次购买后买到了能注册的手机号，并发现已经购买的手机号可以原价退款。
* 4 用谷歌邮箱登陆GitHub，并创建自己的库，在此过程中了解到手机上也可直接用github软件，但国行版iPhone不能在AppStore上下载Github，只需购买外国Apple Id （davidchavezvct@hotmail.com ）在Applestore中切换id 即可搜索到国外软件如github ，X等。并使用shadowrocket连接vpn，然后我发现上述clash连接地址依然可以使用并且不用花任何费用！但手机上我没找到如何创建github库，只能搜索别人的远程链接并阅读。
  # DAY 3—5
  这几天课程较满，有实验课等，此段花费天数较多
  # 下载Visual Studio Code 配置ESP IDF开发环境 点灯
  * 1 配置vscode中文环境：进入vscode界面 在左侧栏搜索Chinese，点击install，然后重启vscode，即可显示中文
  * 2 下载ESP IDF组件，静待漫长的安装后，尝试点亮led灯来检查环境是否配置正确
  * 3 学习GPIO和Esp32s3的参数配置及主要组件（按自己学习的理解来编写的）
|名称|功能|
|:---|---:|
|3v3|3.3v电源|
|USB|开发板供电，可烧录固件至芯片，也可作为通信接口|
|Boot Button（Boot 键）|按住 Boot 的同时按一下 Reset 进入“固件下载”（学的网上教程）|
|Reset Button（Reset 键）|复位按键|
|RGB LED| 发光二极管，由 GPIO38 驱动|
|PWR|开发板接电源后该红灯就会亮|

  * 4 学习点亮led灯的代码（这一块耗费时间较长，主要是学习c语言代码）
```c
#include <stdio.h>
#include "freertos/FreeRTOS.h"//包含 FreeRTOS 实时操作系统的头文件
#include "freertos/task.h"//包含 FreeRTOS 任务相关的头文件
#include "esp_system.h"//包含 ESP 系统相关的头文件
#include "driver/gpio.h"//包含 GPIO 驱动程序的头文件

#define LED_GPIO 4//定义LED灯所连接的 GPIO 引脚号为 4

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
    
  
