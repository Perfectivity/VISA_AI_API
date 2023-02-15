# 🐼 VISA_AI_API (2022年 11月 08日 更新 BY.金柱盛)

Artificial intelligence API used in VISA wechat mini program recommendation functions.
(人工智能地区推荐)

## 参数
---
|**parameter**|**必填**|**definition**|**example**|
|--|--|--|--|
|token❗️|是||tk101_visa_SHA256FlagUnixHash0930#@@endpoint64bit
|openid|是|微信OpenID|odsabK4hNsnWSSFDV7WTksAleo6c89(Real)
|gender|是|男1,女2|1 或 2
|age|是|年龄|18 到 90
|price_range|是|商品价格范围| 1：0-175元 ，2：175-290元 ， 3:290-583元 ，4:583-1167元，5:1167元以上
|visit|是|访问次数|
|sensitivity❗️|否|推荐敏感度|low: 低, medium: 默认 high: 高
|deep_experience❗️|否|经验值|On:活性 Off: 非活性(默认)

---
## 返回的 JSON 数据 
---
|**KEY**|**FEATURES**|**备注**|
|--|--|--|
|openID|要请的OpenID||
|callback|时间|
|token_rc|男1,女2||
|ai_type|JSON数据的identify||
|restaurant|推荐/非推荐的**首尔餐厅**的编号|没有返回的数据时显示为 “ ”|
|tour|推荐/非推荐的**首尔景点**的编号|没有返回的数据时显示为 “ ”|
|night|推荐/非推荐的**首尔夜景**的编号|没有返回的数据时显示为 “ ”|
|shopping|推荐/非推荐的**首尔购物中心**的编号|没有返回的数据时显示为 “ ”|
|food|推荐/非推荐的**店铺**的编号|没有返回的数据时显示为 “ ”|
|souvenir|推荐/非推荐的**纪念品商店**的编号|没有返回的数据时显示为 “ ”|

❗️ 接口返回数据不是产品名称，而是产品的编号，所以返回的地区编号必须与后台页面输入的相同地区编号的地区名称相连接。


---
## Category Flag
---
|**地区分类**|**参数**|
|--|--|
|首尔餐厅|restraurant
|首尔景点|tour
|首尔夜景|night
|首尔购物中心|shopping
|店铺|food
|纪念品商店|souvenir

## ai_type
---
|**ai_type**|**参数**|
|--|--|
|推荐地区（来源于微信小程序）|preference
|推荐地区（正确答案）|standard
|非推荐地区（来源于微信小程序）|unlike
|非推荐地区 (正确答案）|unlike_standard

---
# 接口详情

##  1. 推荐地区（来源于小程序）

🟨 Request URL: https://www.ai-model.kr/ai-deep-mind-visa/predict_proba_visa.ai

🟨 调用数据 ： 6个推荐地区 （每个分类中选1个）

返回数据例子 (**POST**)
```json
{
"openid": "realOpenid", 
"callback": "2022-10-20 13:20:36", 
"token_rcv": "OK",
"ai_type": "preference",
"restaurant": "9351915000116", 
"tour":"9351915000127",
"night": "9351915000135",
"shopping": "9351915000145",
"food": "9351915000154",
"souvenir": "9351915000162"
}
```

##  2. 推荐地区（正确答案）

🟩 Request URL: https://www.ai-model.kr/ai-deep-mind-visa/predict_standard_visa.ai

🟩 调用数据 ： 12个推荐地区 （每个分类中选2个）

返回数据例子 (**POST**)
```json
{"openid": "realOpenid", 
"callback": "2022-10-20 18:56:23", 
"token_rcv": "OK",
"ai_type": "standard",
"restaurant": [ "9351915000116", "9351915000114" ], 
"tour": [ "9351915000127", "9351915000124" ], 
"night": [ "9351915000135" , "9351915000134" ], 
"shopping": [ "9351915000145", "9351915000144" ], 
"food": [ "9351915000154", "9351915000153" ], 
"souvenir": [ "9351915000162", "9351915000161" ]
}
```

**请注意**‼️ ： 审查委员会通过比较跟 1.推荐地区（来源于小程序） 与 2.推荐地区（正确答案）的过程，计算两个数据的类似度。

##  3. 非推荐地区（来源于小程序）

🟦 Request URL: https://www.ai-model.kr/ai-deep-mind-visa/predict_proba_unlike_visa.ai

🟦 调用数据 ： 4个推荐地区

返回数据例子 (**POST**)
```json
{ 
"openid": "realOpenid", 
"callback": "2022-10-20 16:23:56", 
"token_rcv": "OK",
"ai_type": "unlike",
"restaurant": "",
"tour":"9351915000131", 
"night": "9351915000142", 
"shopping": "9351915000150", 
"food": "",
"souvenir": "9351915000168" 
}
```


##  4. 非推荐地区（正确答案）

🟪 Request URL: https://www.ai-model.kr/ai-deep-mind-visa/predict_standard_unlike_visa.ai

🟪 调用数据 ： 6个推荐地区 （每个分类中选1个）

返回数据例子 (**POST**)
```json
{
"openid": "realOpenid", 
"callback": "2022-10-20 17:34:15", 
"token_rcv": "OK",
"ai_type": "unlike_standard", 
"restaurant": "9351915000119", 
"tour":"9351915000131", 
"night": "9351915000142", 
"shopping": "9351915000150", 
"food": "9351915000159", 
"souvenir": "9351915000168"
}
```

---

# 🐼 计算该接口的正确度（人工智能推荐正确度）
---

### 计算 **AUC**(正确度)

AUC的 计算方式如下

![image](https://user-images.githubusercontent.com/78646027/135964126-944919f1-25b9-48f7-89db-7575293ad548.png)

 1) TP  表示推荐地区（来源于小程序）和 推荐地区（正确答案）中相匹配的地区号码的个数。
 2) TN 表示非推荐地区（来源于小程序）和 非推荐地区（正确答案）中相匹配的地区号码的个数。
 3) FP  表示推荐地区（来源于小程序）和 推荐地区（正确答案）中相不匹配的地区号码的个数。
 4) FN 表示非推荐地区（来源于小程序）和 非推荐地区（正确答案）中相不匹配的地区号码的个数。

### 🟨例子

## 1) TP 与 FP



推荐地区（来源于小程序)数据
```json
{
"openid": "aawre2456aa434MMsaaaaaa", 
"callback": "2022-11-00 10:58:16", 
"token_rcv": "OK",
"ai_type": "preference",
"restaurant": "9351915000116", 
"tour":"9351915000127",
"night": "9351915000135",
"shopping": "9351915000145",
"food": "9351915000154",
"souvenir": "9351915000162"
}
```
推荐地区（正确答案）
```json
{"openid": "aaaaaaaaaaaaaa", 
"callback": "2022-11-00 14:31:26", 
"token_rcv": "OK",
"ai_type": "standard",
"restaurant": [ "9351915000116", "9351915000114" ], 
"tour": [ "9351915000127", "9351915000124" ], 
"night": [ "9351915000135" , "9351915000134" ], 
"shopping": [ "9351915000145", "9351915000144" ], 
"food": [ "9351915000158", "9351915000153" ], 
"souvenir": [ "9351915000160", "9351915000161" ]
}
```

这两个接口调用的数据中，
"restaurant": "9351915000116", 
"tour":"9351915000127", 
"night": "9351915000135",
"shopping": "9351915000145" 
这4个是一致（两个数据中都有），

"food": "9351915000154", 
"souvenir": "9351915000162" 
这2个是不一致（只在推荐产品（来源于小程序)数据中存在）。


所以说，**这样的情况下，TP是4，FP是2。**

---

## 2) TN 与 FN



非推荐地区（来源于小程序)数据
```json
{ 
"openid": "aaaaaaaaaaaaaa", 
"callback": "2022-11-00 14:52:20", 
"token_rcv": "OK",
"ai_type": "unlike",
"restaurant": "",
"tour":"9351915000131", 
"night": "9351915000142", 
"shopping": "9351915000150", 
"food": "",
"souvenir": "9351915000168" 
}
```
非推荐地区（正确答案）
```json
{
"openid": "aaaaaaaaaaaaaa", 
"callback": "2022-11-00 14:54:15", 
"token_rcv": "OK",
"ai_type": "unlike_standard", 
"restaurant": "9351915000119", 
"tour":"9351915000131", 
"night": "9351915000142", 
"shopping": "9351915000150", 
"food": "9351915000159", 
"souvenir": "9351915000170"
}
```

这两个接口调用的数据中，
"tour":"9351915000131", 
"night": "9351915000142", 
"shopping": "9351915000150",  
这3个是一致（两个数据中都有），

"souvenir": "9351915000168"
这1个是不一致（只在非推荐地区（来源于小程序)数据中存在）。


所以说，**这样的情况下，TN是3，FN是1。**

---

## 3) 计算 AUC

结果如下，

|**特性**|**数量**|
|--|--|
TP|4
FP|2
TN|3
FN|1

由于AUC的计算方法，

AUC= 4 + 3 / 4 + 2 + 3 + 1 = **0.7**


---

### 成功判断标准

100人的平均AUC0.6以上的话才算合格。

![image](https://user-images.githubusercontent.com/78646027/135964173-135f666c-37d6-4ae4-966a-56a88f8f2738.png)



🔆操作中发生疑问时，微信 : sjthsy
