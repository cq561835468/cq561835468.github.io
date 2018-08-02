# 检查点

### 简易检查点
简易检查点通常用于很少的返回值中，其中也不会有判断等语句，用法:

```
tests["当前值为: "+pm.response.code+"] = pm.response.code == data.Re_State
```
---

这一段是指输出打印信息，无论pass还是fail都会报
```
tests["当前值为: "+pm.response.code+"] 
```

这一段则是检查方式，当这一段的检查内容为True时，这个检查点pass，否则这个检查点fail

```
pm.response.code == data.Re_State
```
![avatar](/img/5.png)

---

### 函数检查点
如果使用for循环时，你会发现使用简易检查点只能检查一次，当出现多个参数时，无法检查到下面的参数了，此时就需要使用到函数检查点了：
函数检查点格式为：

```
    pm.test("当前数值为: "+JsPer[key][keyy], function () {
        pm.expect(JsPer[key][keyy]).to.eql(data.Value);
```
其中前面的依然是打印信息:

```
 pm.test("当前数值为: "+JsPer[key][keyy], function () {
```
具体判断语句为:

```
pm.expect(JsPer[key][keyy]).to.eql(data.Value)
```
- 当改语句为true时，该检查点pass、否则fail。
- 需要注意的一点是在函数检查点中，必须使用pm.expect来返回返回值，如不使用pm.excpect语句，则改检查点一直都是pass的。

将函数检查点运用到for循环中例子：

```
var jsonData = pm.response.json(); //获取返回体"
// 状态码检查点",
tests["当前状态码为: "+pm.response.code+"|状态码应为:"+data.Re_State] = responseCode.code == data.Re_State;
var JsPer = jsonData.FaceListObject.FaceObject

//循环判断key
for (var key in JsPer){
    for(var keyy in JsPer[key]){
        if (keyy == data.Search_Field){
            pm.test("当前数值为: "+JsPer[key][keyy] +"返回验证值应为: "+data.Value, function () {
                pm.expect(JsPer[key][keyy]).to.eql(data.Value);
            });
        }
    }
}
```
### 函数检查点判断方式
#### 单个值判断(是)(非)

```
pm.test("我是打印", function () {
    pm.expect(data1).to.eql(data2); //当data1等于data2时才通过
    
pm.test("我是打印", function () {
    var x = (data1 == data2)
    pm.expect(x).to.eql(false);//当data1不等于data2时才通过
```
#### 多个值判断(与)

```
pm.test("我是打印", function () {
    pm.expect(data1).to.eql(data2);
    pm.expect(data3).to.eql(data4);//当data1=data2 data3=data4时才通过
    
pm.test("我是打印", function () {
    var x = (data1 == data2 && data3 == data4)
    pm.expect(x).to.eql(true);//当data1=data2并且data3=data4时才通过
```
#### 多个值判断(或)

```
pm.test("我是打印", function () {
    var x = (data1 == data2 || data3 == data4)
    pm.expect(x).to.eql(true);//当data1=data2或者data3=data4时才通过
```