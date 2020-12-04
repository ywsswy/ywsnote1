LCD 都需要背光，而 OLED 不需要，因为它是自发光的。
分辨率为 128*64
 SPI/IIC 接口模块
模块接口定义：
1. GND 电源地
2. VCC 电源正（3～5.5V）
3. D0 OLED 的 D0 脚，在 SPI 和 IIC 通信中为时钟管脚
4. D1 OLED 的 D1 脚，在 SPI 和 IIC 通信中为数据管脚
5. RES OLED 的 RES#脚，用来复位（低电平复位）
6. DC OLED 的 D/C#E 脚，数据和命令控制管脚
7. CS OLED 的 CS#脚，也就是片选管脚
IIC 接口模块
1. GND 电源地
2. VCC 电源正（3～5.5V）
3. SCL OLED 的 D0 脚，在 IIC 通信中为时钟管脚
4. SDA OLED 的 D1 脚，在 IIC 通信中为数据管脚
