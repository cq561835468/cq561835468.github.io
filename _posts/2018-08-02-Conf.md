# conf文件编写指南

### conf文件作用
conf文件主要用于针对参数化的参数化，例如当接口A使用到ID、usename、passwd这三个变量时，我们在_data.json文件里会这样写。

```
[
{
	"ID ":"12312313",
	"Username":"test",
	"passwd":"123"
}
]

```
但现在我们考虑下，如果有100个接口，都需要使用到这三个参数的话，我们按照数据和测试用例分离考虑，是不是在100个接口的数据中都加上这三个变量？再考虑到后期如果需要变更里面的密码字段的话，这个工作量是不是很绝望？

由此我们推出了一个解决方案，将参数也进行参数化。

### 如何编写conf文件

现在我们增加2个文件夹，分为input和output，在input文件夹中放置我们的post出去的参数，output中放置我们需要检查的参数。

文件名：Sub_Reg_In.json 放置在input下,放置我们的post出去的参数，内容如下：

```
{
    "Bulk_Sub_Post_In":[
        {
            "ApplicantName":"admin",
            "ApplicantOrg":"3cc204834c5943ecacaacc9f8e8f41c7",
            "BeginTime":"20181101000000",
            "EndTime":"20181231000000",
            "Interface_Url":"10.85.7.62:8084",
            "Interface_st":"/VIID/Subscribes",
            "OperateType":"0",
            "Reason":"subcribe",
            "ReceiveAddr":"http://10.10.40.186/VIID/Test",
            "ResourceURI":"00000000009110000001",
            "SubscribeDetail":"11,12,13,tp,sp",
            "SubscribeID":"20180403999999999885503889408",
            "Title":"订阅"
        }
    ]
}

```
在output目录下，新建一个叫Sub_Reg_Out.json的文件，该文件内容为请求完成后的参数：

```
{
	"Bulk_Sub_Post_Out":[
		{
			"Re_State":"200",
			"StatusCode":"0",
			"Request_Url":"/VIID/Subscribe",
			"StatusString":"",
		}
]
}

```
完成后，在用例同级目录下新建conf.json文件内容为：

```
{"input_parameter":["Sub_Reg_In.Bulk_Sub_Post_In"],"output_parameter":["Sub_Reg_Out. Bulk_Sub_Post_Out "]}
```
改语句含义为：
- input_parameter:在input文件夹中
- Sub_Reg_In:文件名为Sub_Reg_In
- Bulk_Sub_Post_In:把变量名为Bulk_Sub_Post_In对应的参数都取出来
- output_parameter:在output文件夹中
- Sub_Reg_Out:文件名为Sub_Reg_Out
- Bulk_Sub_Post_Out:把变量名为Bulk_Sub_Post_Out对应的参数都取出来

#### 测试conf是否写的正确
进入该目录下，运行以下脚本，注意:第一个参数为产品，第二个为模块，all代表所有模块的conf都走一遍:
![avatar](/img/9.png)
进入conf目录下查看，生成了data文件即conf文件编写正确！
![avatar](/img/10.png)

