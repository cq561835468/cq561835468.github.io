### 参数化
如果不想一味的修改测试脚本中的数据，或者想要测试该接口使用不同的参数，就必须要使用到参数化技术。

---

当我们参数化完成之后，我们获得的文件为2个：

```
VIID[Face_Many][POST].postman_collection.json //测试脚本文件
VIID[Face_Many][POST].postman_collection_data.json //测试数据文件
```

测试脚本文件中包含：

- url参数化
- body参数化
- head参数化
- 检查点参数化

测试数据文件中包含：

- 参数化的数据


#### 参数化第一步
把需要参数化的数据保存成json文件，像这样：
![avatar](/img/6.png)

#### 参数化第二步
外部数据完成后，确保参数没问题后，开始参数化内部数据：
在postman中，参数化方式为：

```
{{参数名}}
```
例如：
![avatar](/img/7.png)

这就是参数化完的数据，目的就是将测试数据抽出开，此时我们运行测试脚本时，需要选择上我们的外部测试数据即可。
![avatar](/img/8.png)

---
### URL参数化

```
{{FL_Interface_Url}}{{FL_Interface_st}}
```

### body参数化


```
    "FaceID": "{{FL_FaceID}}",
    "InfoKind": {{FL_InfoKind}},
    "SourceID": "{{FL_SourceID}}",
    "DeviceID": "{{FL_DeviceID}}",
    "LeftTopX": {{FL_LeftTopX}},
    "LeftTopY": {{FL_LeftTopY}},
    "RightBtmX": {{FL_RightBtmX}},
```

### 检查点参数化
检查点参数化有些不同，需要使用到data.参数名的方式来参数化，例如:

```
var jsonData = pm.response.json(); //获取返回体
var datas = jsonData.ResponseStatusListObject.ResponseStatusObject
tests["当前状态码为: "+pm.response.code+"|状态码应为:"+data.Re_State] = pm.response.code == data.Re_State
```
其中，data.Re_State就是指获取外部数据中的Re_State变量的意思。
![avatar](/img/6.png)

#### 参数化第三步
使用外部数据跑一遍确定没有问题即可！