# Arduino


## 1. Arduino简介  

Arduino是一款开源电子平台，专为DIY爱好者和学习者设计，使他们能够轻松创建互动电子项目。 从2005年发布以来，Arduino因其易用性和灵活性而受到广泛欢迎。Arduino的硬件包括多种型号的开发板，软件上则配备简单易用的Arduino IDE，使用户能够通过C/C++编程语言直接与硬件交互。Arduino平台支持各种传感器和执行器，适用于多个应用领域，包括自动化、机器人、艺术等。因此，它成为了教育、创客运动和原型开发的首选工具。  

## 2. 接线图  

![](media/0c5736f2d990a7dbc32ea958f3b66e23.png)  

## 3. 测试代码（测试软件版本：Arduino 1.8.12）  

```cpp  
#include <Wire.h>  
#include "paj7620.h"  

/*  
Notice: When you want to recognize the Forward/Backward gestures, your gestures' reaction time must less than GES_ENTRY_TIME(0.8s).  
You also can adjust the reaction time according to the actual circumstance.  
*/  

#define GES_REACTION_TIME 500 // You can adjust the reaction time according to the actual circumstance.  
#define GES_ENTRY_TIME 800 // When you want to recognize the Forward/Backward gestures, your gestures' reaction time must less than GES_ENTRY_TIME(0.8s).  
#define GES_QUIT_TIME 1000  

void setup()  
{  
    uint8_t error = 0;  
    Serial.begin(9600);  
    Serial.println("\nPAJ7620U2 TEST DEMO: Recognize 9 gestures.");  
    error = paj7620Init(); // initialize Paj7620 registers  
    if (error)  
    {  
        Serial.print("INIT ERROR,CODE:");  
        Serial.println(error);  
    }  
    else  
    {  
        Serial.println("INIT OK");  
    }  
    Serial.println("Please input your gestures:\n");  
}  

void loop()  
{  
    uint8_t data = 0, data1 = 0, error;  

    error = paj7620ReadReg(0x43, 1, &data); // Read Bank_0_Reg_0x43/0x44 for gesture result.  
    if (!error)  
    {  
        switch (data)   
        {  
            case GES_RIGHT_FLAG:  
                delay(GES_ENTRY_TIME);  
                paj7620ReadReg(0x43, 1, &data);  
                if(data == GES_FORWARD_FLAG)  
                {  
                    Serial.println("Forward");  
                    delay(GES_QUIT_TIME);  
                }  
                else if(data == GES_BACKWARD_FLAG)  
                {  
                    Serial.println("Backward");  
                    delay(GES_QUIT_TIME);  
                }  
                else  
                {  
                    Serial.println("Right");  
                }  
                break;  

            case GES_LEFT_FLAG:  
                delay(GES_ENTRY_TIME);  
                paj7620ReadReg(0x43, 1, &data);  
                if(data == GES_FORWARD_FLAG)  
                {  
                    Serial.println("Forward");  
                    delay(GES_QUIT_TIME);  
                }  
                else if(data == GES_BACKWARD_FLAG)  
                {  
                    Serial.println("Backward");  
                    delay(GES_QUIT_TIME);  
                }  
                else  
                {  
                    Serial.println("Left");  
                }  
                break;  

            case GES_UP_FLAG:  
                delay(GES_ENTRY_TIME);  
                paj7620ReadReg(0x43, 1, &data);  
                if(data == GES_FORWARD_FLAG)  
                {  
                    Serial.println("Forward");  
                    delay(GES_QUIT_TIME);  
                }  
                else if(data == GES_BACKWARD_FLAG)  
                {  
                    Serial.println("Backward");  
                    delay(GES_QUIT_TIME);  
                }  
                else  
                {  
                    Serial.println("Up");  
                }  
                break;  

            case GES_DOWN_FLAG:  
                delay(GES_ENTRY_TIME);  
                paj7620ReadReg(0x43, 1, &data);  
                if(data == GES_FORWARD_FLAG)  
                {  
                    Serial.println("Forward");  
                    delay(GES_QUIT_TIME);  
                }  
                else if(data == GES_BACKWARD_FLAG)  
                {  
                    Serial.println("Backward");  
                    delay(GES_QUIT_TIME);  
                }  
                else  
                {  
                    Serial.println("Down");  
                }  
                break;  

            case GES_FORWARD_FLAG:  
                Serial.println("Forward");  
                delay(GES_QUIT_TIME);  
                break;  

            case GES_BACKWARD_FLAG:  
                Serial.println("Backward");  
                delay(GES_QUIT_TIME);  
                break;  

            case GES_CLOCKWISE_FLAG:  
                Serial.println("Clockwise");  
                break;  

            case GES_COUNT_CLOCKWISE_FLAG:  
                Serial.println("anti-clockwise");  
                break;  

            default:  
                paj7620ReadReg(0x44, 1, &data1);  
                if (data1 == GES_WAVE_FLAG)  
                {  
                    Serial.println("wave");  
                }  
                break;  
        }  
    }  
    delay(100);  
}  
```  

## 4. 代码说明  

在Arduino编辑器中，通过“项目—加载库—库管理”找到并安装Paj7620库。安装后，在示例中会找到“Gesture PAJ7620”示例。该库提供了两个示例脚本，分别用于检测9种和15种手势。需要注意的是，在手势测试时会有0.8秒的反应延迟。  

## 5. 测试结果  

按照接线图连接模块并烧录程序，上电后，打开串口监视器并设置波特率为9600，当在模块前方展示手势时，串口监视器将实时显示对应的手势识别结果，如“Forward”、“Backward”等。


