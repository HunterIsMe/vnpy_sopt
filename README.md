# vn.py框架的CTP期权（ETF）交易接口

<p align="center">
  <img src ="https://vnpy.oss-cn-shanghai.aliyuncs.com/vnpy-logo.png"/>
</p>

<p align="center">
    <img src ="https://img.shields.io/badge/version-3.5.5.0-blueviolet.svg"/>
    <img src ="https://img.shields.io/badge/platform-windows|linux-yellow.svg"/>
    <img src ="https://img.shields.io/badge/python-3.7-blue.svg" />
    <img src ="https://img.shields.io/github/license/vnpy/vnpy.svg?color=orange"/>
</p>

## 说明

基于CTP期权版的3.5.5接口封装开发，接口中自带的是【穿透式实盘环境】的dll文件。

## 安装

安装需要基于2.2.0版本以上的[VN Studio](https://www.vnpy.com)。

直接使用pip命令：

```
pip install vnpy_sopt
```

下载解压后在cmd中运行

```
python setup.py install
```

由于在安装的同时需要编译C++代码，因此在执行上述命令之前需要去微软[官网](https://my.visualstudio.com/Downloads?q=build%20tools)下载Visual Studio Build Tools。其中工作负荷选择Visual C++生成工具，同时推荐下载2017版。

## 使用

以脚本方式启动（script/run.py）：

```
from vnpy.event import EventEngine
from vnpy.trader.engine import MainEngine
from vnpy.trader.ui import MainWindow, create_qapp

from vnpy_sopt import SoptGateway


def main():
    """主入口函数"""
    qapp = create_qapp()

    event_engine = EventEngine()
    main_engine = MainEngine(event_engine)
    main_engine.add_gateway(SoptGateway)
    
    main_window = MainWindow(main_engine, event_engine)
    main_window.showMaximized()

    qapp.exec()


if __name__ == "__main__":
    main()
```
