# 安装
官网下载解压
* Kibana版本应与Elasticsearch版本major相同，minor可以更低，patch任意
# 启动
```sh
bin/kibana [参数]
```
参数|意义
-|-
-q|抑制输出
-Q|抑制全部输出
# 目录
* config、data应置于$KIBANA_HOME外以便升级
## bin 可执行程序
文件|描述
-|-
kibana|启动Kibana
kibana-plugin|安装插件
## config 配置
文件|描述
-|-
kibana.yml|Kibana配置
## data 数据
## optimize 转译的源码
## plugins 插件
# Sample Data
* 主页点击Add sample data下的链接
* 选择一个Sample，点击Add data
* 点击View data进入Dashboard
* 样本中的timestamp取决于安装数据集的时间，重新安装将导致timestamp更新
## Global Flight
### Filter
* 在Controls设置Origin City和Destination City，点击Apply changes
* 点击Clear form、Apply changes以取消filter
* 也可以点击Add filter手动指定
### Query
* 搜索框输入OriginCityName:Rome并回车，查询从罗马出发的航班
* 在此基础上限定航空公司：OriginCityName:Rome AND (Carrier:JetBeats OR "Kibana Airlines")
* 通常filter比query快
* 点击搜索框右侧的按钮以开启/关闭搜索提示
### Discover
* 点击Discover
* Filter下的下拉菜单选择index
* kibana_sample_data_flights含有时间域，直方图显示了document数量随时间的分布
* 默认所有匹配的document的所有field以_source的形式展示，在Available fields下的各field右侧点击Add可指定显示哪些field
### Edit
* 点击Dashboard，菜单栏点击Edit
* 点击Average Ticket Price的齿轮
* 点击Options菜单的Edit visualization
* Bucket下选择Split Group
* Aggregation下拉菜单选择Terms
* Field选择Carrier
* Order By选择Descending，Size填4
* 点击右三角Apply Changes
* 菜单栏点击Save，对话框点击Confirm Save
* 回到Dashboard编辑模式，拖拽Average Ticket Price右下角以改变大小，拖拽顶部改变位置
* 菜单栏点击Save
### Inspect
* 点击Average Ticket Price右上角
* 点击Options的Inspect查看数据表
* 点击View: Data，选择Requests，查看请求情况统计数据、请求和响应json
### Remove
* 主页点击Add sample data下的链接
* 在想要移除的Sample下点击Remove
