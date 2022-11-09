# Arduino太极创客学习笔记（以Arduino R3 Uno开发板为例）
[TOC]
## 基本概述 Arduino介绍
### Arduino是什么？
Arduino是一个意大利品牌。Arduino是一个开放源码电子原型平台，拥有灵活、易用的硬件(各种开发板)和软件（arduino IDE也就是编程器）。Arduino专为设计师，工艺美术人员，业余爱好者，以及对开发互动装置或互动式开发环境感兴趣的人而设的。

Arduino能通过各种各样的传感器来感知环境，通过控制灯光、马达和其他的装置来反馈、影响环境。板子上的微控制器可以通过Arduino的编程语言来编写程序，编译成二进制文件，烧录进微控制器 对Arduino的编程是利用 Arduino编程语言 (基于 Wiring)和Arduino开发环境(based on Processing)来实现的。基于Arduino的项目，可以只包含Arduino，也可以包含Arduino和其他一些在PC上运行的软件，他们之间进行通信 (比如 Flash, Processing, MaxMSP)来实现。

### 如何学习arduino
Arduino近几年在国际发展火热，教程也是五花八门。如果您英语顶呱呱，那推荐到arduino官方网站学习www.arduino.cc，英语不好，或者喜欢看中文教程的，就可以在论坛阅读中文教程（传送门：http://www.arduino.cn/thread-1066-1-1.html）

### 认识Arduino UNO

Arduino UNO是Arduino入门的最佳选择，在编著本书时，其最新的版本为UNO R3，本书大部分内容都是基于Arduino UNO R3写成的。Arduino UNO的详细组成信息如下图所示。


##### 电源（Power）
Arduino UNO有三种供电方式：
● 通过USB接口供电，电压为5V；
● 通过DC电源输入接口供电，电压要求7～12V；
● 通过电源接口处5V或者VIN端口供电，5V端口处供电必须为5V，VIN端口处
   供电为7～12V。

##### 指示灯（LED）
Arduino UNO带有4个LED指示灯，作用分别是：
● ON，电源指示灯。当Arduino通电时，ON灯会点亮。
● TX，串口发送指示灯。当使用USB连接到计算机且Arduino向计算机传输数据
   时，TX灯会点亮。
● RX，串口接收指示灯。当使用USB连接到计算机且Arduino接收计算机传来的
   数据时，RX灯会点亮。
● L，可编程控制指示灯。该LED通过特殊电路连接到Arduino的13号引脚，当
   13号引脚为高电平或高阻态时，该LED会点亮；当为低电平时，不会点亮。因
   此可以通过程序或者外部输入信号来控制该LED的亮灭。
##### 复位按键（Reset Button）
按下该按键可以使Arduino重新启动，从头开始运行程序。
##### 存储空间（Memory）
Arduino的存储空间即是其主控芯片所集成的存储空间。也可以通过使用外设芯片的方式来扩展Arduino的存储空间。Arduino UNO的存储空间分三种：
● Flash，容量为32KB。其中0.5KB作为BOOT区用于储存引导程序，实现通过串
   口下载程序的功能；另外的31.5KB作为用户储存的空间。相对于现在动辄几百
   GB的硬盘，可能觉得32KB太小了，但是在单片机上，32KB已经可以存储很大
   的程序了。
● SRAM，容量为2KB。SRAM相当于计算机的内存，当CPU进行运算时，需要
   在其中开辟一定的存储空间。当Arduino断电或复位后，其中的数据都会丢失。
● EEPROM，容量为1KB。EEPROM的全称为电可擦写的可编程只读存储器，是
   一种用户可更改的只读存储器，其特点是在Arduino断电或复位后，其中的数据
   不会丢失。
##### 输入/输出端口（Input/Output Port）
如图1-20所示，Arduino UNO有14个数字输入/输出端口，6个模拟输入端口。其中一些带有特殊功能，这些端口如下：
● UART通信，为0（RX）和1（TX）引脚，被用于接收和发送串口数据。这两个
   引脚通过连接到ATmega16U2来与计算机进行串口通信。
● 外部中断，为2和3引脚，可以输入外部中断信号。
● PWM输出，为3、5、6、9、10和11引脚，可用于输出PWM波。
● SPI通信，为10（SS）、11（MOSI）、12（MISO）和13（SCK）引脚，可用于
   SPI通信。
● TWI通信，为A4（SDA）、A5（SCL）引脚和TWI接口，可用于TWI通信，兼
   容IIC通信。
● AREF，模拟输入参考电压的输入端口。
● Reset，复位端口。接低电平会使Arduino复位。当复位键被按下时，会使该端口
接到低电平，从而使Arduino复位。


## 基本实操例程

### 1.点亮LED灯

#### 相关指令

##### pinMode()
>**pinMode()说明:**
通过pinMode()函数，你可以将Arduino的引脚配置为以下三种模式：
1.输出(OUTPUT)模式
2.输入(INPUT)模式
3.输入上拉（INPUT_PULLUP）模式 （仅支持Arduino 1.0.1以后版本）
在输入上拉（INPUT_PULLUP）模式中，Arduino将开启引脚的内部上拉电阻，实现上拉输入功能。一旦将引脚设置为输入（INPUT）模式，Arduino内部上拉电阻将被禁用。
**设置Arduino引脚为输出(OUTPUT)模式**
当引脚设置为输出（OUTPUT）模式时，引脚为低阻抗状态。这意味着Arduino可以向其它电路元器件提供电流。也就是说，Arduino引脚在输出（OUTPUT）模式下可以点亮LED或者驱动电机。（如果被驱动的电机需要超过40mA的电流，Arduino将需要三极管或其它辅助元件来驱动他们。）
获得更多关于如何设置Arduino引脚为输出(OUTPUT)的信息，请参阅：OUTPUT
**设置引脚为输入(INPUT)模式**
当引脚设置为输入（INPUT）模式时，引脚为高阻抗状态（100兆欧）。此时该引脚可用于读取传感器信号或开关信号。
注意：当Arduino引脚设置为输入（INPUT）模式或者输入上拉（INPUT_PULLUP）模式，请勿将该引脚与负压或者高于5V的电压相连，否则可能会损坏Arduino控制器。
获得更多关于如何设置Arduino引脚为输入(INPUT)的信息，请参阅：INPUT
**设置引脚为输入上拉（INPUT_PULLUP）模式**
Arduino 微控制器自带内部上拉电阻。如果你需要使用该内部上拉电阻，可以通过pinMode()将引脚设置为输入上拉（INPUT_PULLUP）模式。
注意：当Arduino引脚设置为输入（INPUT）模式或者输入上拉（INPUT_PULLUP）模式，请勿将该引脚与负压或者高于5V的电压相连，否则可能会损坏Arduino控制器。
获得更多关于如何设置Arduino引脚为输入上拉(INPUT_PULLUP)的信息，请参阅：INPUT_PULLUP
相关阅读

##### digitalWrite()
>**digitalWrite()说明:**
将数字引脚写HIGH（高电平）或LOW（低电平）
如果该引脚通过pinMode()设置为输出模式（OUTPUT），您可以通过digitalWrite()语句将该引脚设置为HIGH（5伏特）或LOW（0伏特/GND.
如果该引脚通过pinMode()设置为输入模式（INPUT），当您通过digitalWrite()语句将该引脚设置为HIGH时，
这与将该引脚将被设置为输入上拉(INPUT_PULLUP)模式相同。
获得更多关于输入上拉(INPUT_PULLUP)模式信息请参阅：INPUT_PULLUP
请注意: 比起其它数字引脚，数字引脚13由于内部串联了一个LED并焊接了一个限流电阻，所以该引脚比其他引脚更不易用来实现数字输入功能。如果将数字引脚13设置为输入上拉(INPUT_PULLUP)模式，该引脚将会悬在1.7伏特而不是正常的高电平5伏特。如果必须使用引脚13做为数字输入，请将该引脚配合外部下拉电阻使用。
**语法**
digitalWrite(pin, value)
**参数**
pin：引脚号码
value: HIGH 或 LOW
**返回值**
无

#### 示例程序：
这里我们用5号引脚输出高电平，点亮串联在5号引脚和GND之间的LED灯：电路图参考：https://www.bilibili.com/video/BV164411J7GE?p=13
效果是外接的灯闪烁（注意使用保护电阻）
``` c
// the setup function runs once when you press reset or power the board
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(5, OUTPUT);//设定为输出模式
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(5, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);                       // wait for a second
  digitalWrite(5, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);                       // wait for a second
}
```

### 2.按键开关
开关提供数字信号（开/关），使用arduino接受信号
当引脚设置为输入（INPUT）模式时。此时该引脚可用于读取传感器信号或开关信号。可以识别两种状态：HIGH/LOW
当这个引脚没接东西时（引脚悬空），读取到的状态（HIGH/LOW）是随机的。

#### 相关指令

##### digitalRead()
>**digitalRead()说明**
读取数字引脚的 HIGH(高电平：1）或 LOW（低电平：0）。
**语法**
digitalRead(pin)
**参数**
pin：被读取的引脚号码
**返回值**
HIGH 或 LOW
#### 示例程序：
电路图参考：https://www.bilibili.com/video/BV164411J7GE?p=14&spm_id_from=pageDriver
示例程序里的**Serial**是对串口进行调用，先不做解释。上拉电阻必须要有，不然烧板子。
在串口监视器里可以看到传回的低电平000，按下后出现111。

``` c
/*
  DigitalReadSerial
  Reads a digital input on pin 2, prints the result to the Serial Monitor
  This example code is in the public domain.
*/

// digital pin 2 has a pushbutton attached to it. Give it a name:
int pushButton = 2;

// the setup routine runs once when you press reset:
void setup() {
  // initialize serial communication at 9600 bits per second:
  //以每秒 9600 位的速度初始化串行通信： 
  Serial.begin(9600);
  // make the pushbutton's pin an input:
  pinMode(pushButton, INPUT);
}
// the loop routine runs over and over again forever:
void loop() {
  // read the input pin:
  int buttonState = digitalRead(pushButton);
  // print out the state of the button:
  Serial.println(buttonState);
  delay(1);        // delay in between reads for stability
}
```
### 3.逻辑控制（按钮控制LED灯）
实现效果：按下按钮灯亮
使用设置引脚为输入上拉（INPUT_PULLUP）模式
Arduino 微控制器自带内部上拉电阻。如果你需要使用该内部上拉电阻，可以通过pinMode()将引脚设置为输入上拉（INPUT_PULLUP）模式。
注意：当Arduino引脚设置为输入（INPUT）模式或者输入上拉（INPUT_PULLUP）模式，请勿将该引脚与负压或者高于5V的电压相连，否则可能会损坏Arduino控制器。

电路接线：https://www.bilibili.com/video/BV164411J7GE?p=16&spm_id_from=pageDriver

#### 示例程序
``` c  
void setup() {
  //start serial connection
  Serial.begin(9600);
  //configure pin 2 as an input and enable the internal pull-up resistor
  //将引脚2设置为输入上拉模式
  pinMode(2, INPUT_PULLUP);
  pinMode(13, OUTPUT);

}

void loop() {
  //read the pushbutton value into a variable
  int sensorVal = digitalRead(2);
  //print out the value of the pushbutton
  Serial.println(sensorVal);

  // Keep in mind the pull-up means the pushbutton's logic is inverted. It goes
  // HIGH when it's open, and LOW when it's pressed. Turn on pin 13 when the
  // button's pressed, and off when it's not:
  if (sensorVal == HIGH) {
    digitalWrite(13, LOW);
  } else {
    digitalWrite(13, HIGH);
  }
}

```
### 猜数字装置搭建

参考电路：https://www.bilibili.com/video/BV164411J7GE?p=20&spm_id_from=pageDriver
注意事项：分清数码管型号（共阴，共阳）
#### 相关语法
##### if…else
``` C
if( 表达式1 )
 {
    语句块1
 } 
 else 
 {
    语句块2
 }
 
```
##### switch case
``` c
switch (var) {
    case 1:
        //当var等于1时执行这里的程序
        break;
    case 2:
        //当var等于2时执行这里的程序
        break;
    default:
        // 如果var的值与以上case中的值都不匹配
        // 则执行这里的程序
        break;
}
```

需要注意的几点内容：
1. 在以上结构示例代码中，当变量var和某个case后面的数值匹配成功后，如果没有break, Arduino会执行该分支以及后面所有分支的语句。
2. case 后面必须是一个整数，或者是结果为整数的表达式，但不能包含任何变量。
3. case 后面不能使用字符串，但可以使用字符，使用字符时需要用单引号把字符括起来，如: case: 'b'。
4. default 不是必须的。当没有 default 时，如果所有 case 都匹配失败，那么就什么都不执行。

#### 相关指令（函数）
##### random()
>**random()说明**
random函数可用来产生随机数。
**语法**
random(max)
random(min, max)
**参数**
min: 产生随机数的下限(包含此数值)
max: 产生随机数的上限（不包含此数值）
**返回值**
在最小值(min)和最大值减一(max-1)之间的随机数值
**注意**
单独使用random（）函数。每次程序运行所产生的随机数字都是同一系列数字。并非真实的随机数，而是所谓的伪随机数。如果希望每次程序运行时产生不同的随机数值。应配合使用randomseed()函数。具体操作请参见本站randomseed()函数的具体说明。
##### randomSeed()
>**randomSeed()说明**
randomSeed()函数可用来产生随机种子。单独使用random()函数所产生的随机数，在每一次程序重新启动后，总是重复同一组随机数字。如果希望程序重新启动后产生的随机数值与上一次程序运行时的随机数不同，则需要使用randomSeed()函数。
在实际应用时，可以通过调用analogRead()函数读取一个空引脚，作为随机种子数值。具体操作，本页面后续示例程序将进行说明。
**语法**
randomSeed(seedVal)
**参数**
seedVal: 随机种子数值
**示例**
``` Arduino
long randNumber;

void setup(){
  Serial.begin(9600);
  randomSeed(analogRead(A0)); 
  //将引脚A0放空，每次程序启动时所读取的数值都是不同的。
  //这么做可以产生真正的随机种子值，从而产生随机数值。
}

void loop(){
  randNumber = random(300);  // 产生随机数
  Serial.println(randNumber);

  delay(50);
}
```

#### 示例程序
``` arduino 
/*
 * MC猜数字 （Ver. 1.0）
 *  - LED数码管的原理和使用
 *  - if...else if的概念和应用
 *  - while循环的概念和应用
 *  - switch case控制语句
 *  - random函数的使用
 *  - 建立和使用自定义函数（三种形式：无参数无返回值，有参数无返回值，有参数有返回值）
 *  - 通过串口监视器观察调试程序运行状况
 *  
 *  电路连接：
 *  有关本制作的详细电路连接资料，请参阅太极创客网站的《零基础入门学用Arduino教程》相关网页。
 *  太极创客网站地址：
 *  www.taichi-maker.com
 *  
*/
int thisResult;  //存储按键按下以后显示在数码管的数字。
int nextResult;  //存储作弊数字，也就是下一次按键按下后即将显示的数字。

void setup() {
  pinMode(2, INPUT_PULLUP); //2号引脚上连接有按键开关，将2号引脚设置为输入上拉模式
  int pinNumber = 3;        //设置3-9号引脚为输出模式
  while(pinNumber <= 9){
    pinMode(pinNumber, OUTPUT);
    pinNumber = pinNumber + 1;
  }
  randomSeed(analogRead(A0)); //为了每一次复位或断电后产生不同顺序的随机数字
}

// the loop function runs over and over again forever
void loop() {
  if (!digitalRead(2)){      //读取2号引脚电平状态
    getRandomNumber(0,10);   //用户按下按键后，开始新一次猜数字游戏
  }
  displayNumber(thisResult); //将猜数字游戏"结果"显示在数码管中
}

/*
用户在每一次按下按键后，随机产生的数字将存储于nextResult变量中。
而实际显示在数码管上的数字是thisResult变量。
当thisResult即将显示在数码管前，程序会将下一次显示的数字通过
图形暗示的形式显示在数码管上。具体程序如何显示暗示图形，
请参阅displayCheat()函数说明。
*/
void getRandomNumber(int minNumber, int maxNumber){
  thisResult = nextResult; 
  int i; 
  while(i < 15){
    i = i + 1;
    nextResult = random(0, 10);
    displayRandom();         //显示随机图案，混淆注意力
    delay(50 + i * 10);      //让随机图案显示时间由快到慢，增加混淆
    displayClear();
  }
  displayCheat(nextResult);  //显示作弊图案，用户可通过此函数所显示的图案
                             //获知下次按键后将要出现在LED数码管上的数字。  
                             //此图案是在用户每次按下按键后显示新的数字
                             //前的最后一次图案显示
  delay(500);
  displayClear();
}

//根据参数数值在LED数码管上显示数字
void displayNumber(int ledNumber){     
  switch(ledNumber){
    case 1:  //显示1
      digitalWrite(4, LOW);
      digitalWrite(7, LOW); 
      break;   
    case 2:  //显示2
      digitalWrite(3, LOW);
      digitalWrite(4, LOW); 
      digitalWrite(5, LOW); 
      digitalWrite(8, LOW); 
      digitalWrite(9, LOW); 
      break;   
    case 3:   //显示3
      digitalWrite(3, LOW);
      digitalWrite(4, LOW); 
      digitalWrite(5, LOW); 
      digitalWrite(7, LOW); 
      digitalWrite(8, LOW); 
      break;   
    case 4:  //显示4
      digitalWrite(4, LOW); 
      digitalWrite(5, LOW); 
      digitalWrite(6, LOW); 
      digitalWrite(7, LOW); 
      break;  
    case 5:  //显示5
      digitalWrite(3, LOW);
      digitalWrite(5, LOW); 
      digitalWrite(6, LOW); 
      digitalWrite(7, LOW); 
      digitalWrite(8, LOW); 
      break;
    case 6:  //显示6
      digitalWrite(3, LOW);
      digitalWrite(5, LOW); 
      digitalWrite(6, LOW); 
      digitalWrite(7, LOW); 
      digitalWrite(8, LOW); 
      digitalWrite(9, LOW); 
      break;    
    case 7:  //显示7
      digitalWrite(3, LOW);
      digitalWrite(4, LOW); 
      digitalWrite(7, LOW);  
      break;
    case 8:  //显示8
      digitalWrite(3, LOW);
      digitalWrite(4, LOW);
      digitalWrite(5, LOW); 
      digitalWrite(6, LOW); 
      digitalWrite(7, LOW); 
      digitalWrite(8, LOW); 
      digitalWrite(9, LOW); 
      break;
    case 9:  //显示9
      digitalWrite(3, LOW);
      digitalWrite(4, LOW);
      digitalWrite(5, LOW); 
      digitalWrite(6, LOW); 
      digitalWrite(7, LOW); 
      digitalWrite(8, LOW); 
      break;
    case 0:  //显示默认
      digitalWrite(3, LOW);
      digitalWrite(4, LOW);
      digitalWrite(6, LOW); 
      digitalWrite(7, LOW); 
      digitalWrite(8, LOW); 
      digitalWrite(9, LOW); 
      break;
    default:
        digitalWrite(4, LOW); 
        digitalWrite(5, LOW); 
        digitalWrite(7, LOW); 
        digitalWrite(8, LOW);  
        digitalWrite(9, LOW);   
    }
}

//清理显示内容
void displayClear(){
  digitalWrite(3, HIGH);
  digitalWrite(4, HIGH);
  digitalWrite(5, HIGH); 
  digitalWrite(6, HIGH); 
  digitalWrite(7, HIGH); 
  digitalWrite(8, HIGH); 
  digitalWrite(9, HIGH); 
}

//显示随机图案以混淆注意力
//使作弊图案显示时不易察觉。
void displayRandom(){
  int randomPin = random(3,9);
  digitalWrite(randomPin, LOW);  
}

//显示作弊图案。
void displayCheat(int number){
  switch(number){
    case 1:  // 显示数字1作弊图案
      digitalWrite(3, LOW);
      break;   
    case 2:  // 显示数字2作弊图案
      digitalWrite(6, LOW); 
      break;   
    case 3:  // 显示数字3作弊图案
      digitalWrite(4, LOW); ;
      break;   
    case 4:  // 显示数字4作弊图案
      digitalWrite(5, LOW); 
      break;  
    case 5:  // 显示数字5作弊图案
      digitalWrite(9, LOW); 
      break;
    case 6:  // 显示数字6作弊图案
      digitalWrite(7, LOW);   
      break;    
    case 7: // 显示数字7作弊图案
      digitalWrite(8, LOW);
      break;
    case 8: // 显示数字8作弊图案
      digitalWrite(6, LOW);
      digitalWrite(4, LOW);
      break;
    case 9: // 显示数字9作弊图案
      digitalWrite(9, LOW);
      digitalWrite(7, LOW);
      break;
    case 0: // 显示数字0作弊图案
      digitalWrite(3, LOW);
      digitalWrite(8, LOW); 
      break;
    }
}
```
### 控制LED亮度（模拟输入/输出）
实现按钮控制LED亮度
参考电路：https://www.bilibili.com/video/BV164411J7GE?p=26
#### 相关指令

##### analogRead()

>**analogRead()说明**
本指令用于从Arduino的模拟输入引脚读取数值。Arduino控制器有多个10位数模转换通道。这意味着Arduino可以将0－5伏特的电压输入信号映射到数值0－1023。
换句话说，我们可以将5伏特等分成1024份。0伏特的输入信号对应着数值0，而5伏特的输入信号对应着1023。
例：
当模拟输入引脚的输入电压为2.5伏特的时候，该引脚的数值为512。
(2.5伏特 / 5伏特 = 0.5， 1024 X 0.5 ?＝512)
引脚的输入范围以及解析度可以使用analogReference()指令进行调整。
Arduino控制器读取一次模拟输入需要消耗100微秒的时间（0.0001秒）。控制器读取模拟输入的最大频率是每秒10，000次。
注意：在模拟输入引脚没有任何连接的情况下，用analogRead()指令读取该引脚，这时获得的返回值为不固定的数值。这个数值可能受到多种因素影响，如将手靠近引脚也可能使得该返回值产生变化。
**语法**
analogRead(pin)
**参数**
pin：被读取的模拟引脚号码
**返回值**
0到1023之间的值

##### analogWrite（PWM）
>**analogWrite说明**
将一个模拟数值写进Arduino引脚。这个操作可以用来控制LED的亮度, 或者控制电机的转速. Arduino每一次对引脚执行analogWrite()指令，都会给该引脚一个固定频率的PWM信号。PWM信号的频率大约为490Hz.
在Arduino UNO控制器中，5号引脚和6号引脚的PWM频率为980Hz。在一些基于ATmega168和ATmega328的Arduino控制器中，analogWrite()函数支持以下引脚: 3, 5, 6, 9, 10, 11。
在Arduino Mega控制其中,该函数支持引脚 2 – 13 和 44 – 46。使用ATmega8的Arduino控制器中，该函数只支持引脚 9, 10, 11.
在调用analogWrite()函数前，您无需使用pinMode()函数来设置该引脚。
**语法**
analogWrite(pin, value)
**参数**
pin：被读取的模拟引脚号码
value：0到255之间的PWM频率值, 0对应off, 255对应on
**返回值**
无
#### 示例程序
**按钮控制亮度：**
``` ARDUINO 
/*
25 模拟输出1 - analogWrite
太极创客
www.taichi-maker.com
25 模拟输出1 - analogWrite
*/
boolean pushButton1;   // 创建布尔型变量用来存储按键开关1的电平状态
boolean pushButton2;   // 创建布尔型变量用来存储按键开关2的电平状态
int ledPin = 9;        //LED引脚号
int brightness = 128;  //LED亮度参数

void setup() {
  // put your setup code here, to run once:
  pinMode(2, INPUT_PULLUP); //将引脚2设置为输入上拉模式
  pinMode(8, INPUT_PULLUP); //将引脚8设置为输入上拉模式
  pinMode(ledPin, OUTPUT);  //将LED引脚设置为输出模式
  Serial.begin(9600);      //启动串口通讯
}

void loop() {
  // put your main code here, to run repeatedly:
  pushButton1 = digitalRead(2); //读取引脚2电平状态并将其赋值给布尔变量
  pushButton2 = digitalRead(8); //读取引脚8电平状态并将其赋值给布尔变量
  
  if (!pushButton1 && brightness > 0){     // 当按下按键开关1并且LED亮度参数大于0
    brightness--;                          // 减低LED亮度参数
                                           //（brightness-- 相当于  brightness = brightness - 1;）
  } else if (!pushButton2 && brightness < 255) {  //当按下按键开关2并且LED亮度参数小于255
    brightness++;                                 //增加LED亮度参数
                                                  //（brightness++ 相当于  brightness = brightness + 1;）
  }
  analogWrite(ledPin, brightness);         //模拟输出控制LED亮度
  Serial.println(brightness);              //将LED亮度参数显示在串口监视器上
  delay(10);
}
```

**呼吸灯效果：**
``` arduino
/*
演示如何通过for循环语句实现LED明暗交替（呼吸灯）效果。

2017-04-28
*/
void setup() {
  pinMode(9, OUTPUT);      //设置9号引脚为输出模式
  Serial.begin(9600);     //启动串口通讯
}

void loop() {
  // LED由暗到明
  for (int brightness = 0; brightness <= 255; brightness++){
    analogWrite(9, brightness);   
    Serial.println(brightness);
    delay(10);
  } 
  
  // LED由明到暗  
  for (int brightness = 255; brightness >=0 ; brightness--){
    analogWrite(9, brightness);
    Serial.println(brightness);
    delay(10);
  }
}
```
**电位器控制亮度**
``` arduino 
/*
  电位器模拟输出
 
 读取模拟输入引脚，并将读取到的数值映射到0 - 255之间。然后用该映射结果设置
 引脚9的LED亮度，同时通过串口监视器显示这一映射结果。
 
 电路连接:
     电位器中间引脚连接到模拟输入A0引脚
     电位器两端引脚分别连接在Arduino +5V和接地引脚
   * LED正极通过 限流电阻连接在Arduino的9号引脚
     LED负极接地
 */
 
void setup() {
  
  Serial.begin(9600);  // 串口通讯初始化(9600 bps)
  pinMode(9, OUTPUT);  // 设置9号引脚为输出模式
}
 
void loop() {
  int analogInputVal = analogRead(A0);  // 读取模拟输入值
 
  int brightness = map(analogInputVal, 0, 1023, 0, 255); //将模拟输入数值（0 - 1023）等比映射到模拟输出数值区间（0-255）内
  
  analogWrite(9, brightness);  //根据模拟输入值调节LED亮度
 
  // 将结果通过串口监视器显示:
  Serial.print("analogInputVal = ");
  Serial.println(analogInputVal);
  
  Serial.print("brightness = ");
  Serial.println(brightness);
  
  Serial.println("");
}

```

## 机械臂部分（舵机控制，串口通讯）
### 舵机
#### 基本介绍
舵机，也叫直流伺服电机。
#### 示例程序

``` arduino
/* Sweep
 by BARRAGAN <http://barraganstudio.com>
 This example code is in the public domain.
 
 modified 8 Nov 2013
 by Scott Fitzgerald
 http://www.arduino.cc/en/Tutorial/Sweep
*/
 
#include <Servo.h>
 
Servo myservo;  // 创建Servo对象用以控制伺服电机
                 // 很多开发板允许同时创建12个Servo对象
 
int pos = 0;    // 存储伺服电机角度信息的变量
 
void setup() {
  myservo.attach(9);  // Servo对象连接在9号引脚 
  Serial.begin(9600);
}
 
void loop() {
  for (pos = 0; pos <= 180; pos += 45) { // 0度转到180度
    // 每一步增加1度
    myservo.write(pos);              // 告诉伺服电机达到'pos'变量的角度
    Serial.println(pos);
    delay(1000);                       // 等待15毫秒以确保伺服电机可以达到目标角度
  }
  for (pos = 180; pos >= 0; pos -= 45) { // 180度转到0度
    myservo.write(pos);              // 告诉伺服电机达到'pos'变量的角度
    Serial.println(pos);
    delay(1000);                       // 等待15毫秒以确保伺服电机可以达到目标角度
  }
}
```
### 串口通信
#### serial(串行通信)基本介绍
**说明**
串行端口用于Arduino和个人电脑或其他设备进行通信。所有Arduino控制器都有至少一个串行端口（也称为UART或者USART）。个人电脑可以通过USB端口与Arduino的引脚0(RX)和引脚1(TX) 进行通信。所以当Arduino的引脚0和引脚1用于串行通信功能时，Arduino的引脚0和引脚1是不能做其他用的。你也可以通过Arduino开发环境软件中的串口监视器来与Arduino 控制器进行串口通信，你只需要点击Arduino IDE软件中的“串口监视器”按钮就可以打开串口监视器。

**Serial（串行通信）函数**
[available](#available)
[begin](#begin)
[end](#end)
[find](#find)

findUntil
flush
peek
print
println
parseInt
parseFloat
read
readBytes
readBytesUntil
write
readString
readStringUntil


##### available
**说明**
available() 函数可用于检查设备是否接收到数据。该函数将会返回等待读取的数据字节数。

available()函数属于Stream类。该函数可被Stream类的子类所使用，如（Serial, WiFiClient, File 等）。

**语法**
stream.available()
注：此处stream为概念对象名称。在实际使用过程中，需要根据实际使用的stream子类对象名称进行替换。如：
Serial.available()
wifiClient.available()

**参数**
无

**返回值**
等待读取的数据字节数。
返回值数据类型：int

**示例程序**
``` Arduino
/**********************************************************************
Stream类用于处理字符数据流或二进制数据流。Stream类是不能被直接调用的。
然而当我们使用基于Stream类的库时，都会调用Stream中的内容。

以下Arduino库及相应库中的类都是基于Stream类所实现的。
 库          类
Core        Serial
Wifi        WiFiClient
Ehternet    EthernetClient
ESP8266FS   File
SD          File
Wire        Wire
GSM         GSMClient
SoftwareSerial  SoftwareSerial

此程序使用Serial库来演示Stream类中的available()以及
readString()函数的使用方法。
available() 函数可用于检查设备是否接收到数据。该函数将会返回等待读取的数据字节数。
readString() 函数将读取stream中的字符并存储到字符中。

***********************************************************************/

void setup() {
  // 启动串口通讯
  Serial.begin(9600); 
  Serial.println();
}

void loop() {
  
  if (Serial.available()){                      // 当串口接收到信息后
    Serial.println("Serial Data Available..."); // 通过串口监视器通知用户
    
    String serialData = Serial.readString();    // 将接收到的信息使用readString()存储于serialData变量
    Serial.print("Received Serial Data: ");     // 然后通过串口监视器输出serialData变量内容
    Serial.println(serialData);                 // 以便查看serialData变量的信息
  }
}
```
