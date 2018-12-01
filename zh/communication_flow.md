# 通信流程

描述载具在自动连接期间进行的高级通信流程。

* LinkManager始终保持UDP链接打开，等待载具的心跳包
* LinkManager检测连接到计算机的新已知设备（Pixhawk，SiK Radio，PX4 Flow） 
    * 创建一个新的 SerialLink对象给链接到的设备
* 字节通过链接发送到 MAVLinkProtocol对象
* MAVLinkProtocol将字节转换为MAVLink消息
* 如果消息是HEARTBEAT，则通知MultiVehicleManager(多机管理类)
* MultiVehicleManager (多机管理类)将会接到心跳包的通知 ```HEARTBEAT``` 并根据HEARTBEAT消息中的信息创建一个新的Vehicle对象
* 载具实例化与载机类型匹配的插件
* ParameterLoader对象发送一个与载具相关的PARAM_REQUEST_LIST ```PARAM_REQUEST_LIST``` 使用参数协议向载具加载相对应的参数
* 当参数加载完成后，与载具相关联的MissionManager将使用任务项协议从载机请求任务项
* 当参数加载完成后，VehicleComponents对象将在Setup视图中显示其UI