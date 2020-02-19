# 进阶功能
本文介绍如何创建数字孪生体及部署。

## 数字孪生
在创建数字孪生体之前具备以下：

- 了解真实工作环境及作业流程。
- 了解工业互联网系统中模块及数据对象关系。
- 了解场景应用需求。
- 了解、使用三维模型。

###准备业务
![示例][1]

从上图中，需要以下：

 1. 获得实时油泵温度、噪音等级、转速等。
 2. 获得设备警报信息。
 3. 用三维模型动画展示油泵的运行状况。

###制作模型包

- 使用现有或制作三维模型，导出FBX文件。
- FBX文件三角面<6万，顶点数小于<10万。
- 使用unity开发工具生成AB包，并统一命名为digimodel。
- 导入到模型库，文件大小<5M，设计模型封面。

[查看如何制作AB包][2]



###连接数据库
该节将介绍如何使用数据连接器，为数字孪生体创建api并赋予动态数据。

1、连接数据库

- 让effiar与第三方工业互联网的数据库建立连接。
- 为effiar系统创建数据访问账号、权限。
- 设置数据库类型、账号信息、连接信息等。
- 测试连接状态。

![数据库连接][3]

###设计API

![新建api][4]

- 新建API，设置必要名称及描述。
- 设置安全访问类型。
- 设计输入参数。
- 设计响应数据。
- 测试

下图是设计输入参数和响应数据，所有字段、常量均从数据选择进行定义。

![字段][5]

当设计完成后，可进行测试，查看返回值是否为目标数据。

![测试][6]



###挂载API

![数字孪生][7]

当完成所有API设计后，可打开任务在上下文环境设置数字孪生体。

- 添加数字孪生控件。
- 设置三维模型。
- 添加API。
- 根据上下文设置输入参数值。
- 设置展示效果（详情、趋势、列表）。


###测试效果

打开支持4.0版本的智能眼镜，打开effiar企业应用扫描任务二维码进行体验。

## 部署选项
该节介绍如何安装effiar，进行私有云部署：

###准备环境

- 基于CentOS 7.6或以上版本。
- 远程下载系统镜像。
- 开放端口19999和18888。
- 准备https证书。
- 授权文件。

###安装系统

```
1、解压安装包effiar.tar.gz，并进入安装包目录，执行sudo ./install.sh
2、出现以下6项测试必须全部成功才表示成功：
   test Docker √ 
   test docker-compose √ 
   test API success √ 
   test etcd success √ 
   test Signalling Server success √ 
   test MediaServer success √ 
3、配置服务器ip，打开docker-compose.yml，修改第83行 
   SERVER_IP=127.0.0.1 ，将其中的127.0.0.1改为实际IP地址
4、将域名证书拷贝到/home/nginx目录下，并统一命名为effiar
5、将.lic授权文件拷贝到/home/effiar
6、定位到effiar目录，重启服务sudo docker-compose down && sudo docker-compose up -d
7、测试完成。
```



  [1]: http://39.98.243.76:18888/resources/customer/1/resources/image/8422311024844cff9073299a84dad22f.jpg
  [2]: http://39.98.243.76:18888/resources/customer/1/resources/image/34e0eefa1399481bbf3be44e2bede384.jpg
  [3]: http://39.98.243.76:18888/resources/customer/1/resources/image/34e0eefa1399481bbf3be44e2bede384.jpg
  [4]: http://39.98.243.76:18888/resources/customer/1/resources/image/ef6428c9ca02495a9b251467bc459f24.jpg
  [5]: http://39.98.243.76:18888/resources/customer/1/resources/image/23a623e2db8140ab8ec9d0608320360b.jpg
  [6]: http://39.98.243.76:18888/resources/customer/1/resources/image/ce3570a137c6430e97ab3ce4ec830c61.jpg
  [7]: http://39.98.243.76:18888/resources/customer/1/resources/image/e65ec84b2372462881b2f625a255b7c0.jpg
  
  
  
