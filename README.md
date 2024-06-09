# U-Knob
U-Knob是一款小巧便携的力反馈智能旋钮，你不仅可以自主选择旋钮模式，还可以用它实现PC控制、智能家居控制等。

### BOM

[BOM]: u-knob_png\U-Knob_Bom.xlsx

主要硬件列表：

- 3 块 PCB：主控板 + 驱动板 + 屏幕板
- MCU: ESP32-S3 WROOM-1U-N16R8
- 屏幕: 240x240 圆形 LCD GC9A01 (1.28 寸) + 40.0mm 表蒙子 
- 磁编码器：MT6701CT
- 3205 无刷直流电机（无限位）
- 602535 600mAh 锂电池 

# Get Started

1. 基本环境

   -  VScode + PlatformIO
   - [arduino-esp32](https://github.com/espressif/arduino-esp32) v2.0.14

2. 基本环境

   ```
   # 使用 PlatformIO 打开 U-Knob-master 工程
   # 在src/secrets.h下修改相关配置：WiFi 密码，MQTT Server 等
   
   # （Option）修改 config.h 文件的 MQTT_HOST 为你的名字
   # 该宏用来附带在 MQTT Topic 中
   #define MQTT_HOST               "baitiuy"      
   ```

3. 基本配置

   修改 platform.ini 文件来禁用 MQTT 功能

   ```
   -DXK_MQTT=0 
   ```

   这种方式将保留 Smart Home(S-Home)的 UI 供玩耍，但不会连接 WiFi 和调用 MQTT 发送消息。

4. 编译 && flash




# 功能实现

## LVGL

### UI 设计工具：

- [NXP GUI Guider](https://www.nxp.com/design/software/development-software/gui-guider:GUI-GUIDER)

## SimpleFOC 

- [SimpleFOCStudio](https://github.com/JorgeMaker/SimpleFOCStudio)
- 7 种旋钮模式：边界限制、棘轮、回弹等模式的组合

## MTQQ

### 消息通信机制

- `Subscribe/Unsubscribe`：订阅者向发布者发起/取消订阅
- `Publish`: 发布者向订阅者发布消息
  - 当发布者调用此函数时，消息通知框架将会依次调用各个订阅者通过`SetEventCallback()` 函数注册的回调函数。
- `Notify`: 订阅者向发布者主动发送消息
  - 当订阅者调用此函数时，消息通知框架将调用发布者通过`SetEventCallback()`函数注册的回调函数。
- `Pull`: 订阅者主动向发布者拉消息
  - 消息框架将调用发布者注册的回调函数。这种情况下，发布者注册的 callback 是判断`EVENT_SUB_PULL`事件，将信息填充到订阅者指定的 buf 中

## 智能控制

- Surface Dial 
- U-Knob 通过 MQTT 接入 Home Assistant，可实现控制接入 HASS 的设备
- 按键按压振动反馈、屏幕亮度调节、WiFi 和 MQTT 的 Web 配置
- 电源管理（电池管理、系统深度休眠、自动熄屏）
- 支持 OTA 升级

# 参考项目
- [Hardware: Super Dial 电机旋钮屏](https://oshwhub.com/45coll/a2fff3c71f5d4de2b899c64b152d3da5)
- [Firmware: Super Dial 电机旋钮屏-gitee](https://gitee.com/coll45/super-dial-motor-knob-screen)
- [smart_knob](https://github.com/scottbez1/smartknob)
- [Super knob](https://gitee.com/wenzhengclub/super_knob)
- [X-TRACK](https://github.com/FASTSHIFT/X-TRACK)
- [X-Knob]([https://https://github.com/SmallPond/X-Knob])
- [Peak](https://github.com/peng-zhihui/Peak)
- [xiaomi_miot_raw](https://github.com/ha0y/xiaomi_miot_raw)
