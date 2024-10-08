# TS_OTC 使用指引

一款 **郊狼** 与 **欧洲卡车模拟2**/**美国卡车模拟** 联动的程序，通过监听行驶数据/触发事件进行设备强度的增减以实现惩罚与奖励机制。

相关信息：

* [TS_OTC 程序下载链接](https://m15x-my.sharepoint.com/:f:/g/personal/a-s_m15x_onmicrosoft_com/EpmEwgqbXixMnMltDPuygIEBFA0LSSY9rsB9DPAC6cqlWA?e=Tm8VhC)

## 环境要求

本程序实现依赖 [TruckSim-Telemetry](https://github.com/kniffen/TruckSim-Telemetry) 提供的遥测数据高级SDK，截至 2024/10/02
要求如下：

- Windows系统环境
- 针对欧卡2：游戏本体版本号 >= v1.45
- 针对美卡：游戏本体版本号 >= v1.45
- 与运行该程序的计算机处于同一网络环境下的安卓设备（以启动OTC控制器服务）

## 环境配置

1. 配置 TruckSim-Telemetry 的依赖项 [scs-sdk-plugin](https://github.com/RenCloud/scs-sdk-plugin) SCS遥测SDK文件：

    1. 下载 [SDK](https://github.com/RenCloud/scs-sdk-plugin/releases/latest)，文件名格式应为 `release_v_版本号.zip`。

    2. 根据本机系统类型（**32**/**64**位）将对应文件夹（**Win32**/**Win64**）下 `scs-telemetry.dll` 复制到游戏路径
       `bin\win_x64\plugins`（64位） 或 `bin\win_x86\plugins`（32位） 下。

    3. 启动游戏，若提醒高阶 SDK 已启用则配置成功。

2. 下载 [OTC控制器](https://github.com/open-toys-controller/open-DGLAB-controller/releases/latest) 并安装。

## 程序启用

启动 TS_OTC，右上方显示其与游戏/ OTC 控制器链接状态：

1. 卡车模拟 SDK 正确加载后点击确认，TS_OTC 即可显示连接成功并识别游戏名称。

2. 启动 OTC 控制器，连接玩具并开启娱乐模式（需与 PC 端处于同一网络），提示控制接口已开放即可。

3. TS_OTC 中 **OTC连接设置** 处输入娱乐模式的 IP 地址，点击连接，显示成功即可。

## 参数详解

- 基础配置\强度上限设置：

    1. 标题行括号内（OTC-数值）为 OTC 控制器所设置的最高强度，程序设置的上限强度仍不会超过该强度。

    2. 动态强度即随机强度

- 基础配置\仪表盘是否显示绿幕：

    1. 开启后会在仪表盘窗口添加绿色背景，使用 OBS 或直播姬等工具捕捉时可选择该颜色置为透明色。

- 游戏属性*强度设置\超速设置

    1. 假设豁免比例为 **0.2**，基础强度为 **5**，限速为 **50 km/h**，当前速度为 **70 km/h**，计算逻辑为

        1. 超速比例：（70-50）/50 = 0.4

        2. 0.4 大于豁免比例 0.2， 则将超豁免比例的部分折算基础强度 (0.4-0.2)*5 = 1 点

        3. 程序监听时间为 1s，故 1s 内未及时刹车则会持续叠加

- 游戏属性*强度设置\车辆损坏设置

    1. 车损根据总体损坏度（+磨损）等综合评估后的损坏度与基础强度相乘得到基础强度

- 游戏属性*强度设置\货物送达设置

    1. 配置每送达 1 次货物，减少的强度（需配为负值）

- 游戏属性*强度设置\任务取消设置

    1. 配置每取消 1 次任务，增加的强度

- 游戏属性*强度设置\罚款设置

    1. 如罚款 5000€，则根据 5000/折算金额*基础强度获得最终罚款增加的强度

- 游戏属性*强度设置\里程设置

    1. （仅在**任务**内生效）接取任务后，每行驶【基础里程】公里，则减少【基础强度（需配为负值）】

## 常见问题

由于环境有限，目前仅在欧卡2（ETS2）+手柄条件下测试，有其他问题可在该文档 Project 下提 issue ，会尽量回复（咕

- 输入 IP 点击连接后无响应：

    1. IP 输入可能错误，程序正等待获取连接信息导致超时无响应，可重启程序后输入正确的 IP 再次连接。

- 游戏部分信息捕获异常（行驶里程等）：

    1. 行驶里程仅在任务中有效，需要完整捕获自接取时刻开始的状态，如已接取任务后再启用程序则无法正常捕获该信息。

- TO BE CONTINUED