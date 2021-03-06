# ghost_sa
open_server for sensorsdata

感谢：

感谢神策公司开源了他们的SDK，让用不起神策服务端的中小微企业也可以使用大数据带来的便利。

ghost_sa（鬼策）的结构设计主要考虑方便技术资源不足的中小微企业使用，部署测试快速，并支持复杂数据字段上报（神策原版不支持），所以在长时间段，多字段扫描的场景，性能不如神策原版。需要完整的神策，请购买神策官方授权，他们的程序很给力。https://www.sensorsdata.cn/



介绍：

ghost_sa(鬼策)可以理解为不带前端界面的神策服务端。
主要用途是
1.接收 神策SDK 上报的数据
2.实现神策上的短链创建与解析功能
3.移动端广告监测功能（支持追溯移动端广告来源，支持信息流和积分墙两种模式）。
4.移动端激活回调

使用了flask框架，可以通过uwsgi部署。数据库建议使用TiDB，实测1天200万事件量，单次查询当天事件在10毫秒左右，查询1个月范围的数据，返回在30-60秒左右。
实际使用在TiDB最低的配置3x8c_32g的情况下，每天可以支持500万的事件量。
如果只是体验和测试功能，也可以用MySQL 5.7（含）以上的版本，不过性能很差。

支持使用Kafka。

目前经过测试，支持IOS，Android，JS，小程序，Python的SDK上报。
SDK可以在神策的项目中下载 https://github.com/sensorsdata
SDK的使用方法，可以直接查看神策官方文档 https://www.sensorsdata.cn/manual/


框架说明：

/flask_main.py <--主程序，执行后即可开始接收数据

/kafka_consumer.py <--Kafka订阅程序（如果开启Kafka支持，使用此程序订阅并写入数据库）

/configs <--配置，包括查询密码，数据库密码，第三方依赖的密码都在这里配置

/component <--主要组件，运行程序所需要的主要组件都在这里

/geoip <--IP和ASN识别组件，下载的mmdb需要放在这里

/image <--需要返回的1像素图片所在处。当然，不嫌流量贵，也可以换成其他图片哈

/tools <--迁移工具，包括实时同步神策的数据和迁移历史数据进入鬼策

/logs <--日志，目前只会记录错误日志，按天分

/data_export <--迁移用数据，存放神策历史数据，用于导入鬼策。导入完后，可删除


安装初始化：

安装之前需要先准备好数据库，测试功能可以用mysql5.7。

！！！正式环境建议使用tidb或其他newsql。

1.打开/geoip/geo.py 文件，根据文件里的地址，下载ipcity和ipasn文件，并放到/geoip目录下。

2.配置/configs/db.py 里的数据库连接参数。

3.打开/configs/admin.py 修改查询密码和Kafka支持（默认关闭，直接写入数据）。如果开启Kafka支持，需要配置/configs/kafka.py和运行/kafka_consumer.py来订阅数据。

4.打开/component/setup.py 在最后一行修改自己想要创建的项目名。运行setup.py程序，会完成数据表创建，鬼策服务端初始化完成。使用mysql测试可以用/component/setup_mysql.py创建，会相应缩短字段长度来适应MySQL的限制。

5.运行/flask_main.py 可以开始接收数据了。

更多文档可以看 wiki  https://github.com/white-shiro-bai/ghost_sa/wiki



国内用户可以加我微信 Ben_Xiaobai ，加入鬼策微信群

也可以通过视频，了解鬼策。这里有一个以鬼策为基础的视频分析课（还在佛系更新）

https://space.bilibili.com/920446/channel/detail?cid=124583