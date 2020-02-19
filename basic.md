
# 基础功能


## 自定义专家端

所有请求均为POST，返回值code=200或1为成功。

###发送验证码
-   路径：/api/customer/login/sendLoginSms ：
-   参数：

    ```
    {
      "phone":"登录专家手机号码"
    }
    ```
    
###用户登录
-   路径：/api/customer/login/login ：
-   参数：
    ```
    {
      "phone":"登录专家手机号码"
      "securityCode":"6位数字短信验证码"
    }
    ```
    
-   响应：
    ```
    {
      "token":"用户token，后面所有接口需要把token添加到请求header"
      "imtoken":"用户IMtoken，后面发送消息接口需要把IMtoken添加到请求header"
    }
    ```
    
###获取个人信息
-   路径：/api/customer/remote/getStaffNameHead：
-   参数(header)：
    ```
    {
      "customerToken":"你的token"
    }
    ```
    
-   响应：
    ```
    {
      "data":{
          "customerStatus":1,
          "phone"："你的手机号码",
          "headImage":"头像地址",
          "name":"姓名"
      }
    }
    ```

###获取智能眼镜列表
-   路径：/api/customer/remote/getGlassesList：
-   header：
    ```
    {
      "customerToken":"你的token"
    }
    ```
    
-   参数：
    ```
    {
      "pageNum":"页码（默认位1）"
    }
    ```
    
-   响应：
    ```
    {
    "code":1,
    "message":"成功",
    "data":{
        "pageNum":1,
        "pageSize":1,
        "size":1,
        "startRow":0,
        "endRow":0,
        "total":1,
        "pages":1,
        "list":[
            [
                {
                    "lineStatus":"是否在线（在线方可进行呼叫）",
                    "glassesName":"设备名称",
                    "headImage":"正在使用该眼镜的工作人员头像",
                    "imageUrl":"智能眼镜图片",
                    "phoneNum":"正在使用该眼镜的工作人员手机号码",
                    "deviceCode":"智能眼镜设备ID",
                    "workerName":"正在使用该眼镜的工作人员姓名"
                }
            ]
        ],
        "prePage":0,
        "nextPage":0,
        "isFirstPage":true,
        "isLastPage":true,
        "hasPreviousPage":false,
        "hasNextPage":false,
        "navigatePages":8,
        "navigatepageNums":[
            1
        ],
        "navigateFirstPage":1,
        "navigateLastPage":1,
        "lastPage":1,
        "firstPage":1
    }
}
    ```

###获取房间列表
-   路径：/api/customer/remote/underwayVideoRoomList：
-   header：
    ```
    {
      "customerToken":"你的token"
    }
    ```
    
-   参数：
    ```
    {
      "pageNum":"页码（默认位1）"
      "roomStatus":"房间状态（1为正在进行的，0为历史）"
    }
    ```
      
-   响应：
    ```
    {
    "code":1,
    "message":"成功",
    "data":{
        "pageNum":1,
        "pageSize":1,
        "size":1,
        "startRow":0,
        "endRow":0,
        "total":1,
        "pages":1,
        "list":[
            {
                "deviceCode":"设备硬件ID",
                "deviceId":1,
                "workHeadImage":"工作人员头像",
                "roomId":"房间号",
                "roomchatID":"聊天室id",
                "cover":"房间视频封面",
                "createTime":"创建时间",
                "endTime":"关闭时间",
                "videoFile":"历史视频地址（正在进行房间，改字段为空）",
                "workerName":"员工测试",
                "roomToken":"房间token",
                "deviceGlassesName":"设备名称"
            }
        ],
        "prePage":0,
        "nextPage":0,
        "isFirstPage":true,
        "isLastPage":true,
        "hasPreviousPage":false,
        "hasNextPage":false,
        "navigatePages":8,
        "navigatepageNums":[
            1
        ],
        "navigateFirstPage":1,
        "navigateLastPage":1,
        "lastPage":1,
        "firstPage":1
    }
}
    ```

###发送消息
-   路径：/api/message/send：
-   Header：
    ```
    {
      "access-token":"你的IMtoken"
    }
    ```
    
-   参数：
    ```
    {
      "content":"内容"
      "action":"0为单聊 1群聊  （在呼叫动作中使用单聊，在音视频房间中使用群聊）"
      "sender":"当前用户手机号码"
      "receiver":"单聊为目标用户手机号码，群聊为聊天室id"
      "format":"默认0"
    }
    ```
   
-   响应：
    ```
    {
      "code":200
    }
    ```


###开始接入依赖包

* 1 、引入effiar_im.js、jquery.js、jquery.json-2.3.min.js、layim.config.js、layui.js、message.js、replyboby.js、sentbody.js videocall.js文件。
* 2、 在store目录新建index.js，内容如下：

```
const state = {
    connectStatus: false,
    isWebCall:false,//是否为web端主动呼叫
    content:'',//单聊消息
    goupContent:'' //群消息
}
const getters = {
    getConnectStatus: ({connectStatus}) => connectStatus,
    getCallStatus:({isWebCall}) => isWebCall,
    getContent:({content}) => content,
    getGoupContent:({goupContent}) => goupContent
}
const actions = {
}
const mutations = {
    updateConnectStatus(state, payload) {
        state.connectStatus = payload
    },
    updateCallStatus(state,payload){
        state.isWebCall = payload
    },
    updateContent(state,payload){
        state.content = payload
    },
    updateGoupContent(state,payload){
        state.goupContent = payload
    }
}
export default {
    state,
    getters,
    actions,
    mutations
}
```

* 3、在主界面进行IM连接CIMWebBridge.connection( )。
* 4、在主界面watch收到的消息（如果在音视频界面，则使用getGoupContent进行群消息监听）。
```
watch{
          getContent:function(newVal){
               const content=newVal.content;
               其他字段查看发送消息的字段；
          }
}
```

* 5、当接受到Start时，弹出呼叫请求对话框，并向呼叫者发送两种信号（接受、拒绝）
当为接受时：调用发送IM消息API，content内容为以下内容，并等待Call#Room房间信息。

* 6、当接受到Refuse时，关闭对话框吗，不做任何处理。

* 7、当接受到Room时，或者三个room值，并跳转到音视频界面。


###接入音视频画面
* 1、在index.html 引入videocall.js文件。
* 2、初始化音视频对象，并输入roomId，roomToken，调用joinroom()。
* 3、等待成员列表回调，创建video标签，并将成员姓名为“Glass”的视频画面push到该标签。
```
<div id="videoList" ref="video"></div>

```
* 4、离开房间调用leaveroom()。
* 5、在音视频页面watch监听getGoupContent（聊天室消息）。
* 6、发送消息，将action设置为1，receiver设置为chatroomId，具体消息格式参考下表。

###消息规则


| Call       | #   |  Start |  #  | phoneNumber | #  | name  | #  |  head  |
| ---------- | ----|---------|----|-------------|----|--------|----|-------|
| ---------- | ----|呼叫开始信号|----|呼叫者手机号码|----|呼叫者姓名|----|呼叫者头像|
| Call       | #   |  Refuse |  #  | phoneNumber | #  | name  | #  |  head  |
| ---------- | ----|拒绝呼叫|----|呼叫者手机号码|----|呼叫者姓名|----|呼叫者头像|
| Call       | #   |  Room |  #  | roomId | #  | roomToken  | #  |  chatroomId  |
| ---------- | ----|音视频房间|----|房间号|----|房间密钥|----|房间聊天室id|



## 自定义系统接口
此部分接口依赖开发者密钥和遵循数据规则。

###同步用户
-   路径：/api/sync/syncUserInfo：
-   参数：
    ```
    {
      "body":"具体人员数据放在请求body里面"
      "key":"开发者密钥"
    }
    ```
    
-   body：
    ```
    "body": {
        "users": [
            {
                "phone": "18211026134 (用户手机号码)",
                "name": "张三 (用户名字)",
                "head": "/head/h123.jpg (用户头像直接路径)",
                "id": "123456 (用户外部系统id)"
            }
        ]
    }
   ```
-   响应：

    ```
    {
      "code":"200"
    }
    ```
 

###同步任务模版
-   路径：/api/sync/saveStandardCalorieInfo ：
-   参数：
    ```
    {
      "cardNo":"任务id",
      "title":"任务名称",
      "txt":"任务文件"
    }
    ```
    
-   txt文件内容规则：
    ```
    "data":[
    {
    "title":"巡视变压器",
    "content":"运行环境符合要求。主要包括变压器接地良好，各连接头紧固;各侧避雷器工作正常;各继电保护装置工作正常等",
    "part":"主变",
    "index":"1/10"
    }
    
    ]
    ```
    
-   响应：
    ```
    {
      "code":200
    }
    ```




###同步多媒体资源
-   路径：/api/sync/syncResource ：
-   参数：
    ```
    {
      "body":"具体资源数据放到body data"
      "key":"开发者密钥"
    }
    ```

-   data内容规则：
    ```
    "data":[
        {
            "url":"12.1.2.13/res/hd.jpg "(资源文件直接地址),
            "title":"资源名称",
            "type":"1",（1 图片 2 视频 3 pdf文档）
        }
    ]
    ```
    
-   响应：
    ```
    {
      "code":200
    }
    ```


###同步任务结果数据
-   路径：/api/由开发在自己系统便携同步接口 ：
-   参数：
    ```
    {
      "file":"接受任务结果文件",
      "title":"任务名称",
      "content":"任务描述",
      "starttime":"任务开始时间 单位毫秒"
      "endtime":"任务结束时间 单位毫秒"
    }
    ```
    
-   file文件内容规则：
    ```
    --  任务编号目录 
         -  image 目录
         -  video 目录
         -  task 文件
    ```
  
 以下为每步执行详情：
    ``` 
    "task":[
        {
        "leftState":"正常",
        "part":"电机运行",
        "lon":"0.0",
        "title":"观察电机状态",
        "content":"观察电机噪声是否过大，查看实时振动数据",
        "input":"-1",
        "rightState":"异常",
        "startTime":"1572058921228",
        "endTime":"1572058940975",
        "captruePhoto":"123324324.jpg",  （对应在相应目录的此文件名称）
        "lat":"0.0", 
        "captrueVideo":"-1",
        "status":"0"
        }
    ]
    ```
   
-   响应：
    ```
    {
      "token":200
    }
    ```



###推送消息
-   路径：/api/sync/pushMessage ：
-   参数：
    ```
    {
      "message":"具体资源数据放到body data"
      "key":"开发者密钥"
    }
    ```
    
-   data内容规则：
    ```
    "data":{
        "userId": "135555628872 (推送消息接收方手机号码)",
        "deviceName": "发动机",
        "deviceID": "123123",
        "state": "0 (0 异常、1正常、-1未指定)",
        "content": "0 (内容)",
        "solve": "将电源拨出，并打开散热板"
    }
    ```
    
-   响应：

    ```
    {
      "code":200
    }
    ```


