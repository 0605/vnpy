# DataManager - 历史数据管理模块

历史数据管理模块，是VN Trader内部针对历史数据的全功能管理工具。





## 加载启动

### VN Station加载

启动登录VN Station后，点击VN Trader Pro按钮，在配置对话框中的“上层应用”栏勾选DataManager。



### 脚本加载

在启动脚本中添加如下代码：

```
# 写在顶部
from vnpy.app.data_manager import DataManagerApp

# 写在创建main_engine对象后
main_engine.add_app(DataManagerApp)
```





## 启动模块

启动VN Trader后，在菜单栏中点击【功能】-> 【数据管理】，或者点击左侧按钮栏的图标<img src="https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/00.png" style="zoom:50%;" />即可进入该图形化历史数据管理界面，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/1.png)





## 下载数据

在查看数据之前，需要保证数据库内有足够的历史数据，故vn.py提供了历史数据一键下载的功能。

点击右上角【下载数据】按钮，会弹出下载历史数据窗口，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/2.png)

需要填写代码、交易所、周期、以及开始日期以及四个字段信息。<span id="jump">

- 代码：代码格式为合约品种，如IF88、rb88；
- 交易所：合约交易的交易所（点击窗口右侧箭头按钮可选择vn.py支持的所有交易所）；
- 周期：格式为MINUTE（对应一分钟）、HOUR（对应一小时）、DAILY（对应一天）、WEEKLY（对应一周）和TICK（对应一个tick）五个频率（点击窗口右侧箭头按钮可选择频率）；
- 开始和结束日期：格式为yy/mm/dd，如2018/2/25（点击窗口右侧箭头按钮可改变日期大小）。</span>

填写完字段信息后，点击下方【下载】按钮启动下载程序，下载成功如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/3.png)

 

### RQData（期货、股票、期权）

RQData提供国内期货、股票ETF以及期权的历史数据。在使用前要保证RQData初始化完毕（RQData配置详见基本使用篇的全局配置部分）。



### 数字货币（现货、期货、永续）

数字货币交易所都提供历史数据，但是每个交易所对获取历史数据长度的限制是不同的。下载前需要连接好接口。

在每个接口文件（vnpy.gateway文件夹下）处理收到的合约信息时，如果该接口支持历史数据查询，则history_data会填True，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/cta_backtester/5.png)

如果没有填写该项或填False，则说明该接口不提供历史数据。



### IB（外盘期货、股票、外汇等）

盈透证券提供外盘股票、期货、期权的历史数据， 下载前需要连接好IB接口。

注意IB上的行情数据，除少量免费提供外（外汇、贵金属），其他大部分都需要在IB官网的后台管理系统中付费购买后，才能在VN Trader中订阅使用，进而进行历史数据下载。





## 导入数据

打开DataManager模块的界面后，点击右上角的【导入数据】按钮，即可看到从CSV文件导入数据的对话框，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/16.png)

点击顶部的【选择文件】按钮，会弹出窗口，以供选择要加载的CSV文件路径，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/5.png)

选择完CSV文件路径之后，填写合约信息（填写格式详见本章[下载数据](#jump)部分的介绍）；

然后查看CSV文件的表头信息，并将对应的表头字符串输入在表头信息中，对于【持仓量】字段，如果CSV文件中不存在的话（比如股票数据）请留空即可；

在格式信息中，配置时间戳的日期时间格式，用于后续的时间解析处理。填写完毕，则如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/4.png)

点击【确定】按钮，开始从CSV文件导入数据到数据库中。导入过程中界面会处于半卡住的情况，对于越大的CSV文件（数据量越多），卡住的时间也会越长。成功载入之后，会弹出窗口显示载入成功，入下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/6.png)

最后需要注意的是，这里导入的合约代码（symbol）和交易所（exchange）两个字段组合起来，才能构成在CTA回测等模块中使用的本地代码（vt_symbol）。举例来说，合约代码为IF2003，交易所选择CFFEX（中金所），则在CtaBacktester中回测要用到的本地代码应为IF2003.CFFEX。





## 查看数据

目前VN Trader中获取数据的方式一共有三种：

- 通过RQData下载

- 从CSV文件导入

- 使用DataRecorder模块录制

不管采用何种方法获取数据，点击左上角的【刷新】按钮，即可看到当前数据库中已有数据的统计情况，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/7.png)

点击【查看】按钮，则会弹出选择数据区间对话框，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/10.png)

选择好要显示的数据范围后，点击【确定】按钮即可在右侧表格中看到每个时间点上具体的数据字段：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/11.png)





## 导出数据

选择任意一个合约，点击该合约行数据右侧的【导出】按钮后，会弹出对话框，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/8.png)

选好要导出的数据区间范围点击【确定】后，会再次弹出对话框选择输出文件的位置，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/9.png)

输入确定即可完成CSV文件的导出。





## 删除数据

选择任意一个合约，点击该合约行数据右侧的【删除】按钮后，会弹出对话框，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/12.png)

点击【OK】按钮即可删除该合约数据，并弹出删除成功窗口，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/13.png)

此时再点击【刷新】按钮，图形界面上已经没有该合约的信息了，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/data_manager/14.png)





## 更新数据

