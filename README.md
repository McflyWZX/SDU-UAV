# SDU-UAV
# 山东大学电赛无人机方案和资料
四轴飞行器基本原理介绍：[四轴飞行器入门——基础知识-CSDN博客](https://blog.csdn.net/pix_csdn/article/details/81096324)
## 硬件简介
### 构成
最基本的四旋翼无人机由桨叶、电机、电调、电池、飞控和机架构成。当需要接收遥控信号时，还需要遥控器接收机。当需要执行自动控制任务时，还需要集成在飞控中的上层应用程序或独立的上层控制计算机，如STM32、TI单片机或树莓派香橙派等。可能还会需要一些传感器做闭环控制，下面将进行细致的介绍。
#### 电机
电机的常见规格是它的直径、高度和KV值，如2208 2300KV电机，它的直径就是22mm，高度是8mm，KV值是2300。直径和高度往往决定了它的输出功率和拉力，KV值则决定了输入电压的变化与输出转速的关系，比如升高相同的电压，2000KV的电机将比1000KV的电机转速上升更多。
一般来讲大KV值代表高机动和低力效，一般带多叶桨，常用在穿越机上。小KV值代表高力小长续航，一般带两叶桨。常用的KV值是1300和2300。
航模用的无刷电机有三条线，上面通过的是三相交流电流，当需要使电机反转时，只需调换任意两条线即可。
#### 电调
电调是电机的驱动器，它将电池的直流电转化成能够驱动电机的交流电，并且通过一条PWM控制线来控制电机的转速。它的主要参数是最大电流的支持的电池S数，一般用25-30A，3-4S电调。当最大电流低于电机需要电流时，电调可能被烧毁。
电调还分为独立电调（一个电机一个）和4和一电调，建议电赛用独立电调，更好更换和维护。
#### 电池
电池参数有容量、S数和倍率（C数），容量就是我们常见的mah。S数是电池的级数，航模锂电池是多级锂电池的串联，比如说3S电池就是3节电池串联，它决定了电池的总电压，S数越大同等电流下输出功率就越大。C数也就是放电倍率，决定了电池的最大放电电流，比如2300mah 25C电池，最大电流就是$2.3A\times25=57.5A$，确保电池的放电倍率足够。
动力锂电池单节满电电压是4.2V，一般用到3.6V就需要充电了，需要常备BB响来检测电池电压，避免过放。锂电池总电压一般可以通过$S数 \times 3.8V$来计算。
电赛一般配3-4S电池即可，我们当时的配置是3S 4500mah，能飞6分钟。
#### 飞控
飞控的基本功能是根据当前姿态和指令，通过调整电机的转速输出来控制飞机达到期望姿态。就平衡飞行来说，如果只是简单的给4个电机输出相同的值，是无法保持飞机的平衡的。这是由于各个电机和电调的微小差异，还有外部风扰造成的。这里一定要用到一个反馈控制器，也称闭环控制器，来修正飞机的姿态，这就是飞控。飞控根据自己的IMU（姿态传感器）来修正电机的输出来控制飞机的姿态。
一般来讲，飞控还接收接收机的信号，这样我们就可以通过遥控器来给飞控下达指令来控制飞机了。在电赛要求里，飞机需要自动起飞并完成任务，这时需要一个上层控制器来给飞控指令。可以直接模仿遥控器的信号输出给飞控（参考： [OPENMV结合PIX飞控实现四轴定点 循迹 2017电赛_Kevincoooool的博客-CSDN博客](https://blog.csdn.net/xiangkezhi167810/article/details/78618798) ），也可以使用PX4固件的专有通信协议mavlink。
#### 机架
机架即飞机的机身，它的参数就是轴距，比如330机架
