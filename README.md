# -ardunino-
以esp8288nodemcu为控制核心，在接收至来自RFID522的信号和云端服务器的信号控制舵机来实现门的打开。






基于esp8266wifi模块的智能门锁


二○二二年五月

基于esp8266wifi模块的智能门锁
摘要：为了使个人居住的家庭或宿舍得到更好的开门体验及解决出门忘带钥匙的烦恼，因此设计一个多功能智能门锁。系统设计包括了硬件和软件两部分。硬件主要包括了esp8266wifi NodeMCU模块、RFID522射频IC ID卡感应模块、MG996R 舵机、电源模块 。软件部分在Arduino开发环境下进行。

关键词：esp8266;RFID522;Arduino;

随着当代信息社会的快速发展，人们对物质和精神方面的需求也随之提高.越来越多的智能电子产品进个人们的生活中，智能门锁的应用也越来越广泛。目前较流行的开门方式有密码开锁、指纹识别、连网远程解锁、人脸识别等等。本文以esp8288nodemcu为控制核心，在接收至来自RFID522的信号和云端服务器的信号控制舵机来实现门的打开。

1.硬件设计：
本文设计的智能门锁以esp8266wifi NodeMCU作为控制核心，工作模块有MFRC522识别卡模块、MG996舵机。
1.1 esp8266mcu
	Esp8266Nodemcu是一款集成了Wifi功能的MCU开发板，可以直接连接wifi,开发环境多元化ESP8266模组共有16个引脚，引脚分布图如图所示，要想使模组正常工作，必须对其外围电路进行设计，。复位电路采用按键控制电路，复位信号RST低电平有效，工作状态下复位信号置高，需要复位时，按下按键将RST管脚置零进行复位；使能管脚EN一直置高；ESP8266工作模式由GPIO0决定，上拉进入工作模式，下拉进入下载模式，所以GPIO0管脚的电路设计成按键控制，未按下时上拉VCC，按下后下拉GND，通过按键控制ESP8266在工作模式和下载模式之间切换；本硬件在Arduino IDE开发下，使用C语言进行编程，集编程和烧录一体，并且还有许多的库函数可以使用。最后实现的总体系统框架图如下图1.11所示。
 
 

1.2 MFRC522
	是应用于 13.56MHz 非接触式通信中高集成度读写卡系列芯片中的一员。是 NXP 公司针对“三表”应用推出的一款低 电压、低成本、体积小的非接触式读写卡芯片，是智能仪表和便携 式手持设备研发的较好选择，芯片引脚图如下所示：
 

1.3 MG99R舵机模块
	舵机的控制一般需要一个20ms左右的时基脉冲，该脉冲的高电平部分一般为0.5ms-2.5ms范围内的角度控制脉冲部分，总间隔为2ms。以180度角度伺服为例，那么对应的控制关系是这样的：
0.5ms--0度；
1.0ms--45度；
1.5ms--90度；
2.0ms-135度；
2.5ms--180度；

2.系统软件设计
软件设计使用 Arduino IDE集成开发环境，基于官方提供的开发包和下载的MFRC522库进行程序设计。
2.1 程序初始化
	程序的初始化在ardunio ide中的setup函数中设定。设置串口的波特率为9600bps,里使用串口主要用于输出MFRC522所感应到的卡片类型和卡号以便于设置用户使用的ID卡；mfrc522.PCD_Init();初始化MFRC522；使用Button.attach();函数设置对应终端上的控制按钮。调用舵机库中的myservo.attach(0,500,2500);函数设置舵机数据引脚D3。调用库函数BlinkerMIOT.attachPowerState(miotPowerState);设置小爱电源控制；

2.2 门锁控制程序
	在loop循环中不断检测是否有来自MFRC522的信号和云端手机app的信号并作出相应的控制舵机转动。当有卡片或手机模拟的门禁卡时MFRC522将卡号信息传送到esp8266nodemcu中进行判断卡号是否正确，正确则将信号发送到舵机上进行开门，否则不作出反应，在这部分程序的设计中可以实现无网络情况下进开门。在接入互联网的情况下可以使用手机app远程开门，使用小米米家接入esp8266该设备可进行远程的语音控制。
 



 
 
							图2.21 系统连接元件

 
图2.2.2 app控制页面

 
图2.2.3 esp8266安装测试模型
 
图2.2.4 MFRC522安装测试模型
3.系统测试
	搭建系统测试环境对其功能测试验证，门锁采用micro数据线对esp8266进行供电如图2.2.3安装所示。采用6V2A舵机供电电源适配器对舵机进行供电。MFRC522连接到esp8266外放到门外如图2.2.4所示。测试过程如下：
（1）	通电后查看舵机初始化时是否归位。
（2）	通过点击手机app开门按钮，查看舵机是否有反应。
（3）	在联网的情况下使用手机唤醒小爱同学并让小爱同学开门，舵机作出相应的响应。
（4）	使用已经录入信息到esp8266的ID卡和没有录入信息的卡片放到MFRC522进行感应测试，观察舵机所作出的反应。


4.总结
	本文使用esp8266 Nodemcu模组设计实现了一款可便捷实时开闭锁、造价低廉、安全性高的智能门锁，有着多种的开门方式，可以大大方便人们的生活。当主人不在家时可以征得主人的同意进行远程解锁，当无网络连接时也可以使用的手机或手表模拟的门禁卡即可实现开门，不用再受传统钥匙的限制，出门仅带一手机即可。当在家里但手头上有事要忙时可以唤醒手机小爱同学进行语音开门，完全不用自己动手。


#include <SPI.h>
#include <MFRC522.h>
//#include <ESP8266WiFiMulti.h>//ESP8266WiFiMulti.可连接最强WiFi
 

#include <Servo.h>//舵机库

#define BLINKER_WIFI//
#define BLINKER_MIOT_LIGHT   //定义为语音控制灯设备
//#define BLINKER_MIOT_OUTLET//开关
#include <Blinker.h>//

//ESP8266WiFiMulti wifiMulti;建立对象
char auth[] = "1cfab3e25453";   //！key！  代码需更改的地方

//通过addAP函数存储 wifi名  密码
//wifiMulti.addAP("esp8266,liu10086");//mate40pro
//wifiMulti.addAP("HUAWEI-4FX2AG_HiLinK,L13104930958");//华为路由器
//wifiMulti.addAP("PY,ilovepython3");//1004wifi

//char ssid[] = "HUAWEI-4FX2AG_HiLink";             //!wifi名称!
//char pswd[] = "L13104930958";      //!wifi密码!

//char ssid[] = "esp8266";             //!wifi名称!
//char pswd[] = "liu10086";      //!wifi密码!

//char auth[] = "54bd482bc146";   //！key！  代码需更改的地方
char ssid[] = "PY";             //!wifi名称!
char pswd[] = "ilovepython3";      //!wifi密码!

BlinkerButton Button1("btn-abc");   //位置1 按钮 数据键名
BlinkerButton Button2("btn-max");   //位置1 按钮 数据键名
BlinkerButton Button3("btn-min");   //位置2 按钮 数据键名
Servo myservo;
#define RST_PIN         5           // 配置针脚
#define SS_PIN          4
MFRC522 mfrc522(SS_PIN, RST_PIN);   // 创建新的RFID实例
MFRC522::MIFARE_Key key;
int servo_max,servo_min;
void button1_callback(const String & state) {    //位置1按钮 开关灯
    BLINKER_LOG("get button state: ", state);
    //myservo.write(servo_max);
    digitalWrite(LED_BUILTIN, LOW);   // Turn the LED on (Note that LOW is the voltage level
  // but actually the LED is on; this is because
  // it is active low on the ESP-01)
  delay(1000); 
    digitalWrite(LED_BUILTIN, HIGH); 
    Blinker.vibrate();//让反馈让手机震动
}
void button2_callback(const String & state) {    //位置1按钮
    BLINKER_LOG("get button state: ", servo_max);
    myservo.write(servo_max);
    
    delay(1000); 
     ESP.deepSleep(2e6);
   // myservo.write(servo_max);
    Blinker.vibrate();
}
void button3_callback(const String & state) {   //位置2按钮
    BLINKER_LOG("get button state: ", servo_min);//print
    myservo.write(servo_min);//写舵机
    delay(1000); 
    myservo.write(180);
    delay(1000); 
    ESP.deepSleep(2e6);
    Blinker.vibrate();//让反馈让手机震动
}

//小爱电源类回调
void miotPowerState(const String & state)
{
    BLINKER_LOG("need set power state: ", state);

    if (state == BLINKER_CMD_ON) {
        digitalWrite(LED_BUILTIN, LOW); 
              //myservo.write(0);
              
                 myservo.write(0);
        BlinkerMIOT.powerState("on");
        BlinkerMIOT.print();
              delay(1000);
              
             myservo.write(180);
             delay(1000);
            
             ESP.deepSleep(2e6);
            
    }
    else if (state == BLINKER_CMD_OFF) {
        digitalWrite(LED_BUILTIN, HIGH);
        //myservo.write(180);
        //myservo.write(180);
        
        ESP.deepSleep(2e6);
        BlinkerMIOT.powerState("off");
        BlinkerMIOT.print();
    }
}


void setup() {
   Serial.begin(9600); // 设置串口波特率为9600
    BLINKER_DEBUG.stream(Serial);//
    Blinker.begin(auth, ssid, pswd);//
    pinMode(LED_BUILTIN, OUTPUT);     // Initialize the LED_BUILTIN pin as an output
    Button1.attach(button1_callback);
 //digitalWrite(LED_BUILTIN, HIGH);
   // servo.attach(16,500,2500);
    
  Button2.attach(button2_callback);
  Button3.attach(button3_callback);
        myservo.attach(0,500,2500);  //servo.attach():设置舵机数据引脚D3
            myservo.write(180);
        //myservo.write(0);  //servo.write():设置转动角度
        servo_max=180;
        servo_min=0;
   
   BlinkerMIOT.attachPowerState(miotPowerState); //小爱电源控制
 
  SPI.begin();        // SPI开始
  mfrc522.PCD_Init(); // Init MFRC522 card
  Serial.println("test-demo-start");
}


void loop() {
   Blinker.run();//
  // 寻找新卡
  

  if ( ! mfrc522.PICC_IsNewCardPresent()) {
    //Serial.println("没有找到卡");
    return;
  }

  // 选择一张卡
  if ( ! mfrc522.PICC_ReadCardSerial()) {
    Serial.println("没有卡可选");
    return;
  }


  // 显示卡片的详细信息
  Serial.print(F("卡片 UID:"));
  dump_byte_array(mfrc522.uid.uidByte, mfrc522.uid.size);
  Serial.println();
  Serial.print(F("卡片类型: "));
  MFRC522::PICC_Type piccType = mfrc522.PICC_GetType(mfrc522.uid.sak);
  Serial.println(mfrc522.PICC_GetTypeName(piccType));
  Serial.println("here is 139");
  // 检查兼容性
  if (    piccType != MFRC522::PICC_TYPE_MIFARE_MINI
          &&  piccType != MFRC522::PICC_TYPE_MIFARE_1K
          &&  piccType != MFRC522::PICC_TYPE_MIFARE_4K) {
    Serial.println(F("仅仅适合Mifare Classic卡的读写"));
    return;

    
  }

  MFRC522::StatusCode status;
  if (status != MFRC522::STATUS_OK) {
    Serial.print(F("身份验证失败？或者是卡链接失败"));
    Serial.println(mfrc522.GetStatusCodeName(status));
    return;
  }

  
 /*if (mfrc522.uid.uidByte[0]==0x8E&&
mfrc522.uid.uidByte[1] ==0xC0&&
mfrc522.uid.uidByte[2] ==0x2C&&
mfrc522.uid.uidByte[3] ==0x9A){
    myservo.write(180);
    delay(1000);
    delay(1000);
    myservo.write(0);
}
else*/ 
if ( mfrc522.uid.uidByte[0]==0x13&&//白卡
mfrc522.uid.uidByte[1] ==0xA2&&
mfrc522.uid.uidByte[2] ==0x73&&
mfrc522.uid.uidByte[3] ==0xA3){ 
  
    myservo.write(0);
    delay(1000);
    myservo.write(180);
    delay(1000);
    ESP.deepSleep(2e6);
}
else if (mfrc522.uid.uidByte[0]==0x06&&  //刘俊卡
mfrc522.uid.uidByte[1] ==0x31&&
mfrc522.uid.uidByte[2] ==0x57&&
mfrc522.uid.uidByte[3] ==0xAE){ 
    myservo.write(0);
    
    delay(1000);
   
    myservo.write(180);
        delay(1000);
    ESP.deepSleep(2e6);
}
else if ( mfrc522.uid.uidByte[0]==0xAB&&  //浩卡
mfrc522.uid.uidByte[1] ==0x4B&&
mfrc522.uid.uidByte[2] ==0x3A&&
mfrc522.uid.uidByte[3] ==0xB2){ 
    myservo.write(0);
    delay(1000);
    
     myservo.write(180);
         delay(1000);
    ESP.deepSleep(2e6);
}
else if( mfrc522.uid.uidByte[0]==0x6B&&  //明卡
mfrc522.uid.uidByte[1] ==0x58&&
mfrc522.uid.uidByte[2] ==0x37&&
mfrc522.uid.uidByte[3] ==0xB2){ 
    myservo.write(0);
    delay(1000);
  

    myservo.write(180);
        delay(1000);
    ESP.deepSleep(2e6);
}
else if ( mfrc522.uid.uidByte[0]==0x8B&&  //恺卡
mfrc522.uid.uidByte[1] ==0xF9&&
mfrc522.uid.uidByte[2] ==0x71&&
mfrc522.uid.uidByte[3] ==0xB2){ 
    myservo.write(0);
    delay(1000);
   

    myservo.write(180);
        delay(1000);
    ESP.deepSleep(2e6);
}
else if ( mfrc522.uid.uidByte[0]==0x1B&&  //恺卡2
mfrc522.uid.uidByte[1] ==0x5A&&
mfrc522.uid.uidByte[2] ==0x41&&
mfrc522.uid.uidByte[3] ==0xB2){ 
     myservo.write(0);
    delay(1000);
   

    myservo.write(180);
        delay(1000);
    ESP.deepSleep(2e6);
}
else if ( mfrc522.uid.uidByte[0]==0xEB&&  //符卡
mfrc522.uid.uidByte[1] ==0xA9&&
mfrc522.uid.uidByte[2] ==0x4C&&
mfrc522.uid.uidByte[3] ==0xB2){ 
    myservo.write(0);
    delay(1000);
   

    myservo.write(180);
        delay(1000);
    ESP.deepSleep(2e6);
}
else if ( mfrc522.uid.uidByte[0]==0x6B&&  //关卡
mfrc522.uid.uidByte[1] ==0x57&&
mfrc522.uid.uidByte[2] ==0x3B&&
mfrc522.uid.uidByte[3] ==0xB2){ 
    myservo.write(0);
    delay(1000);
  

    myservo.write(180);
        delay(1000);
    ESP.deepSleep(2e6);
}
  //停止 PICC
  mfrc522.PICC_HaltA();
  //停止加密PCD
  mfrc522.PCD_StopCrypto1();
  Serial.println("here is 251");
  ESP.deepSleep(2e6); //睡眠2s后restet,先烧入程序后连res引脚
  return;
}

/**
   将字节数组转储为串行的十六进制值
*/
void dump_byte_array(byte *buffer, byte bufferSize) {
  for (byte i = 0; i < bufferSize; i++) {
    Serial.print(buffer[i] < 0x10 ? " 0" : " ");
    Serial.print(buffer[i], HEX);
    /*digitalWrite(LED_BUILTIN, LOW);   // Turn the LED on (Note that LOW is the voltage level
  // but actually the LED is on; this is because
  // it is active low on the ESP-01)
  delay(1000);                      // Wait for a second
  digitalWrite(LED_BUILTIN, HIGH);  // Turn the LED off by making the voltage HIGH
  delay(2000);                      // Wait for two seconds (to demonstrate the active low LED)
  */
  }
}

