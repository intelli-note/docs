---
title: Intelli Note
language_tabs:
  - shell: Shell
  - http: HTTP
  - javascript: JavaScript
  - ruby: Ruby
  - python: Python
  - php: PHP
  - java: Java
  - go: Go
toc_footers: []
includes: []
search: true
code_clipboard: true
highlight_theme: darkula
headingLevel: 2
generator: "@tarslib/widdershins v4.0.22"

---

# Intelli Note

A WeChat Mini Program for taking notes on anything/everything.

# 用户

## GET 用户许可证

GET /users/licence

登录/注册/维持登录态接口，返回具有**有效期**的许可证 `token` ，微信小程序端可保存 `token` 许可证用于后续调用接口时识别用户身份。

使用说明：
1. 通过 [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) 获取登录凭证 `code` 
2. 使用登录凭证 `code` 请求此接口，会有如下几种情况：
    - 未注册用户：服务端会自动完成注册并返回许可证 `token`
    - 未登陆/登录过期用户：服务端校验后返回许可证 `token`
    - 已登录用户：服务端返回最新的许可证 `token`
3. 将 `token` 及其**有效期**保存本地，后续接口调用时将其放在请求头`Authorization`字段中
4. 用户下次进入小程序时校验 `token` 是否过期，如果登录态过期则需要重新调用接口更新许可证

登录流程详见：[微信开发者文档-小程序登录](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/login.html)

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|code|query|string| 是 |通过 `wx.login` 获取|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "user": {
      "id": "530000197401277154",
      "username": "魏丽",
      "avatar": "http://dummyimage.com/234x60",
      "biography": "府规入持条分己设来造热正二矿记门重。",
      "gender": "未知",
      "follows_num": "8968",
      "followers_num": "2348万"
    },
    "licence": {
      "access_token": "UVMGP2HX@F$jkY!Yv9YMf$9[[7Ym%@[&69G8h&RvkzO)Xli!Zc",
      "expires_in": 189704
    }
  }
}
```

```json
{
  "code": 400,
  "msg": "code无效"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»» username|string|true|none|用户名|none|
|»»» avatar|string|true|none|用户头像|头像图片URL|
|»»» biography|string|false|none|个人简介|此项非必须|
|»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»» state|object|false|none||none|
|»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»» licence|object|true|none|许可证信息|none|
|»»» access_token|string|true|none|token|接口调用凭证|
|»»» expires_in|integer|true|none|有效期|接口调用凭证超时时间，单位（秒）|

## POST 用户修改个人资料

POST /users

> Body 请求参数

```yaml
username: "{% mock 'cname' %}"
avatar: cmMtdXBsb2FkLTE3MTE2MDQ3NTYzNTYtMg==/avatar.jpg
biography: "{% mock 'cparagraph', 1, 250 %}"
gender: "{% mock 'natural', '0', 2 %}"

```

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|body|body|object| 否 |none|
|» username|body|string| 否 |姓名|
|» avatar|body|string(binary)| 否 |头像|
|» biography|body|string| 否 |个人简介|
|» gender|body|integer| 否 |性别|

#### 枚举值

|属性|值|
|---|---|
|» gender|0|
|» gender|1|
|» gender|2|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "id": "990000198812204494",
    "username": "萧艳",
    "avatar": "http://dummyimage.com/180x150",
    "biography": "条路议所斯并细也但儿北规家道和亲平农。",
    "gender": "未知",
    "follows_num": "8423",
    "followers_num": "3578"
  }
}
```

```json
{
  "code": 400,
  "msg": "用户名长度不符合要求"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[user](#schemauser)|false|none|交易对象|账单创建时另外一方用户信息|
|»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»» username|string|true|none|用户名|none|
|»» avatar|string|true|none|用户头像|头像图片URL|
|»» biography|string|false|none|个人简介|此项非必须|
|»» gender|string|true|none|性别|取值范围：男、女和未知|
|»» follows_num|string|true|none|关注数|用户关注的数量|
|»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»» state|object|false|none||none|
|»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 搜索用户

GET /users

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|key|query|string| 是 |搜索关键字|
|page_num|query|integer| 是 |页码|
|page_size|query|integer| 是 |每页数量|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 1,
    "list": [
      {
        "id": "220000198610093580",
        "username": "郑强",
        "avatar": "http://dummyimage.com/468x60",
        "biography": "无亲该广物增来日风非石江整流。",
        "gender": "未知",
        "follows_num": "7384",
        "followers_num": "3744亿",
        "state": {
          "follow_status": true,
          "self_status": true
        }
      }
    ],
    "size": 1,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» pages|integer|true|none|总页数|none|
|»» page_num|integer|true|none|当前页码|none|
|»» page_size|integer|true|none|每页的数量|none|
|»» total|integer|true|none|总数|none|
|»» list|[[user](#schemauser)]|true|none|当前页的数据列表|none|
|»»» 交易对象|[user](#schemauser)|false|none|交易对象|账单创建时另外一方用户信息|
|»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»» username|string|true|none|用户名|none|
|»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»» biography|string|false|none|个人简介|此项非必须|
|»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»» state|object|false|none||none|
|»»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 分页获取用户关注列表

GET /follows

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|page_num|query|integer| 是 |页码，从1开始|
|page_size|query|integer| 是 |每页数量|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 1,
    "list": [
      {
        "id": "62000020010522322X",
        "username": "叶娜",
        "avatar": "http://dummyimage.com/728x90",
        "biography": "说须阶适品切采细速重子内生酸。",
        "gender": "未知",
        "follows_num": "421",
        "followers_num": "6836万",
        "state": {
          "follow_status": true,
          "self_status": false
        }
      }
    ],
    "size": 1,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» pages|integer|true|none|总页数|none|
|»» page_num|integer|true|none|当前页码|none|
|»» page_size|integer|true|none|每页的数量|none|
|»» total|integer|true|none|总数|none|
|»» list|[[user](#schemauser)]|true|none|当前页的数据列表|none|
|»»» 交易对象|[user](#schemauser)|false|none|交易对象|账单创建时另外一方用户信息|
|»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»» username|string|true|none|用户名|none|
|»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»» biography|string|false|none|个人简介|此项非必须|
|»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»» state|object|false|none||none|
|»»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## POST 关注用户

POST /follows

> Body 请求参数

```json
{
  "userId": "string"
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» userId|body|string| 是 | 用户ID|被关注者的用户ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 分页获取用户粉丝列表

GET /followers

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|page_num|query|integer| 是 ||页码，从1开始|
|page_size|query|integer| 是 ||每页数量|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 1,
    "list": [
      {
        "id": "440000199008067860",
        "username": "苏秀兰",
        "avatar": "http://dummyimage.com/728x90",
        "biography": "代能门求件况划到准示起三白持得根真。",
        "gender": "女",
        "follows_num": "1342",
        "followers_num": "2287亿",
        "state": {
          "follow_status": false,
          "self_status": false
        }
      }
    ],
    "size": 1,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» pages|integer|true|none|总页数|none|
|»» page_num|integer|true|none|当前页码|none|
|»» page_size|integer|true|none|每页的数量|none|
|»» total|integer|true|none|总数|none|
|»» list|[[user](#schemauser)]|true|none|当前页的数据列表|none|
|»»» 交易对象|[user](#schemauser)|false|none|交易对象|账单创建时另外一方用户信息|
|»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»» username|string|true|none|用户名|none|
|»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»» biography|string|false|none|个人简介|此项非必须|
|»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»» state|object|false|none||none|
|»»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 分页获取用户账单列表

GET /bills

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|page_num|query|integer| 是 ||页码，从1开始|
|page_size|query|integer| 是 ||每页数量|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 2,
    "list": [
      {
        "id": "310000200305061461",
        "type": 1,
        "amount": "127.58",
        "note": {
          "id": "140000201208153623",
          "author": {
            "id": "230000201502117588",
            "username": "陈霞",
            "avatar": "http://dummyimage.com/234x60",
            "biography": "长史经好细争命全增争变西面江做北越。",
            "gender": "未知",
            "follows_num": "8447",
            "followers_num": "8977万",
            "state": {
              "follow_status": false,
              "self_status": false
            }
          },
          "title": "身本后公以快率",
          "cover": "http://dummyimage.com/728x90",
          "view_num": "2761w",
          "star_num": "4690w",
          "comment_num": "6709",
          "create_time": "1970-08-13 00:44:11",
          "update_time": "2024-03-28 14:55:17"
        },
        "counterpart": {
          "id": "530000199701232897",
          "username": "赖刚",
          "avatar": "http://dummyimage.com/160x600",
          "biography": "科品生转王理完毛专点干除准走。",
          "gender": "女",
          "follows_num": "5622",
          "followers_num": "8744亿",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "create_time": "2024-03-28 14:55:17"
      },
      {
        "id": "120000200908299334",
        "type": 2,
        "amount": "148.25",
        "note": {
          "id": "460000199011195381",
          "author": {
            "id": "150000201308044258",
            "username": "马超",
            "avatar": "http://dummyimage.com/120x600",
            "biography": "条军红育验必眼打直党证使边存红传保。",
            "gender": "男",
            "follows_num": "7282",
            "followers_num": "9330亿",
            "state": {
              "follow_status": false,
              "self_status": false
            }
          },
          "title": "离长家面运",
          "cover": "http://dummyimage.com/728x90",
          "view_num": "9482w",
          "star_num": "8433w",
          "comment_num": "5063w",
          "create_time": "1994-08-03 22:36:20",
          "update_time": "2024-03-28 14:55:17"
        },
        "counterpart": {
          "id": "630000198506207644",
          "username": "白勇",
          "avatar": "http://dummyimage.com/728x90",
          "biography": "安油我什叫标件断方非月整实合儿。",
          "gender": "女",
          "follows_num": "3590",
          "followers_num": "3780亿",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "create_time": "2024-03-28 14:55:17"
      }
    ],
    "size": 2,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» pages|integer|true|none|总页数|none|
|»» page_num|integer|true|none|当前页码|none|
|»» page_size|integer|true|none|每页的数量|none|
|»» total|integer|true|none|总数|none|
|»» list|[[bill](#schemabill)]|true|none|当前页的数据列表|none|
|»»» 交易详情|[bill](#schemabill)|false|none|交易详情|交易账单|
|»»»» id|string|true|none|账单ID|数字编号形式的账单ID|
|»»»» type|integer|true|none|类型|账单类型|
|»»»» amount|string|true|none|账单金额|三位逗分|
|»»»» note|[note-overview](#schemanote-overview)|true|none|笔记|账单所关联的笔记|
|»»»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»»» username|string|true|none|用户名|none|
|»»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»»» state|object|false|none||none|
|»»»»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»»»»» title|string|true|none|标题|笔记标题|
|»»»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»»»» view_num|string|true|none|浏览量|none|
|»»»»» star_num|string|true|none|收藏量|none|
|»»»»» comment_num|string|true|none|评论量|none|
|»»»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»»»» counterpart|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»» username|string|true|none|用户名|none|
|»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»» state|object|false|none||none|
|»»»» create_time|string|true|none|创建时间|订单创建时间|
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

#### 枚举值

|属性|值|
|---|---|
|type|0|
|type|1|
|type|2|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 获取用户钱包信息

GET /wallets

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "balance": "345.44",
    "revenue": "858.87"
  }
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[wallet](#schemawallet)|false|none|数据|none|
|»» balance|string|true|none|余额|账户余额，三位逗分|
|»» revenue|string|true|none|累计收入|累计余额，三位逗分|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 根据ID查询用户

GET /users/{user_id}

注：用户未登录时， `state` 字段不返回。

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|user_id|path|string| 是 ||用户ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "id": "610000201511048845",
    "username": "林涛",
    "avatar": "http://dummyimage.com/234x60",
    "biography": "步斗角题你置系民山易别验十重手。",
    "gender": "女",
    "follows_num": "8002",
    "followers_num": "6846万",
    "state": {
      "follow_status": false,
      "self_status": false
    }
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[user](#schemauser)|false|none|交易对象|账单创建时另外一方用户信息|
|»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»» username|string|true|none|用户名|none|
|»» avatar|string|true|none|用户头像|头像图片URL|
|»» biography|string|false|none|个人简介|此项非必须|
|»» gender|string|true|none|性别|取值范围：男、女和未知|
|»» follows_num|string|true|none|关注数|用户关注的数量|
|»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»» state|object|false|none||none|
|»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|

## DELETE 取关用户

DELETE /follows/{user_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|user_id|path|string| 是 ||用户ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## DELETE 删除账单

DELETE /bills/{bill_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|bill_id|path|string| 是 ||账单ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

# 笔记

## GET 分页获取笔记列表

GET /notes

**默认查询当前用户的笔记列表**。此外，可通过对请求参数 `query_type` 的设置，共有五种查询方式：
- `purchase`：查询当前用户购买的笔记
- `history`：查看当前用户笔记浏览记录
- `recommend`：获取推荐的笔记列表
- `favorite`：查询某一收藏夹内的笔记
需要设置请求参数 `favorite_id` （收藏夹ID）
- `collection`：查询某一合集内的笔记
需要设置请求参数 `collection_id` （合集ID）
- `search`：搜索笔记
需要设置请求参数 `search_key` （搜索关键字）和 `search_scope` （搜索范围）。其中 `search_scope` 是枚举量（默认值为 `0` ）：当值为 `0` 时，搜索笔记和合集；当值为 `1` 时，仅搜索笔记；当值为 `2` 时，仅搜索合集

注：当前用户仅可查询公开的或者本人的收藏夹和合集，并且可查询搜索到的笔记的权限为公开、免费或者本人是作者。

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|query_type|query|string| 否 ||查询类别|
|favorite_id|query|string| 否 ||收藏夹ID|
|collection_id|query|string| 否 ||合集ID|
|search_key|query|string| 否 ||搜索关键字|
|search_scope|query|integer| 否 ||搜索范围|
|page_num|query|integer| 是 ||页码，从1开始|
|page_size|query|integer| 是 ||每页数量|

#### 枚举值

|属性|值|
|---|---|
|query_type|purchase|
|query_type|history|
|query_type|favorite|
|query_type|collection|
|query_type|recommend|
|query_type|search|
|search_scope|0|
|search_scope|1|
|search_scope|2|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 3,
    "list": [
      {
        "id": "220000199609016371",
        "author": {
          "id": "520000197103255043",
          "username": "钱洋",
          "avatar": "http://dummyimage.com/160x600",
          "biography": "商题土县认油精还拉次十律大议史光京。",
          "gender": "女",
          "follows_num": "6291",
          "followers_num": "9707万",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "title": "并向外",
        "cover": "http://dummyimage.com/180x150",
        "view_num": "5276w",
        "star_num": "5672",
        "comment_num": "8011",
        "create_time": "1987-05-06 02:17:11",
        "update_time": "2024-03-28 15:41:33"
      },
      {
        "id": "650000201011147350",
        "author": {
          "id": "12000019821028224X",
          "username": "万静",
          "avatar": "http://dummyimage.com/125x125",
          "biography": "便来公海克队改身象率成断京学对。",
          "gender": "女",
          "follows_num": "6621",
          "followers_num": "7870万",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "title": "花了数且意说",
        "cover": "http://dummyimage.com/88x31",
        "view_num": "2371",
        "star_num": "760",
        "comment_num": "8751w",
        "create_time": "1971-04-04 15:11:54",
        "update_time": "2024-03-28 15:41:33"
      },
      {
        "id": "230000200508175733",
        "author": {
          "id": "460000200507031167",
          "username": "任静",
          "avatar": "http://dummyimage.com/88x31",
          "biography": "增石和行听难织整种说做平受。",
          "gender": "未知",
          "follows_num": "2458",
          "followers_num": "9835",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "title": "马下响叫压五",
        "cover": "http://dummyimage.com/234x60",
        "view_num": "384w",
        "star_num": "7297",
        "comment_num": "6359",
        "create_time": "2001-06-04 15:37:48",
        "update_time": "2024-03-28 15:41:33"
      }
    ],
    "size": 3,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

> 权限不足

```json
{
  "msg": "用户权限不足"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|权限不足|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» pages|integer|true|none|总页数|none|
|»» page_num|integer|true|none|当前页码|none|
|»» page_size|integer|true|none|每页的数量|none|
|»» total|integer|true|none|总数|none|
|»» list|[[note-overview](#schemanote-overview)]|true|none|当前页的数据列表|none|
|»»» 笔记|[note-overview](#schemanote-overview)|false|none|笔记|账单所关联的笔记|
|»»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»» username|string|true|none|用户名|none|
|»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»» state|object|false|none||none|
|»»»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»»»» title|string|true|none|标题|笔记标题|
|»»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»»» view_num|string|true|none|浏览量|none|
|»»»» star_num|string|true|none|收藏量|none|
|»»»» comment_num|string|true|none|评论量|none|
|»»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

状态码 **403**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

## POST 新增笔记

POST /notes

> Body 请求参数

```yaml
title: string
cover: cmMtdXBsb2FkLTE3MTE3Nzc5ODQ1NDctNQ==/comment.jpg
content: string
publicOption: true
freeOption: 0

```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» title|body|string| 是 ||笔记标题|
|» cover|body|string(binary)| 否 ||笔记封面，非必填项|
|» content|body|string| 是 ||笔记内容|
|» publicOption|body|boolean| 否 ||配置选项：笔记是否公开|
|» freeOption|body|integer| 否 ||配置选项：笔记是否免费。若值为0，则免费，其余则收费，单位：分|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "id": "220000197709031318",
    "author": {
      "id": "820000199602298187",
      "username": "谭霞",
      "avatar": "http://dummyimage.com/234x60",
      "biography": "完之质社已位区周出住集需候部成个。",
      "gender": "男",
      "follows_num": "655",
      "followers_num": "252万",
      "state": {
        "follow_status": false,
        "self_status": true
      }
    },
    "title": "马程然装制便",
    "cover": "http://dummyimage.com/120x240",
    "view_num": "0",
    "star_num": "0",
    "comment_num": "0",
    "create_time": "2024-03-30 16:51:15",
    "update_time": "2024-03-30 16:51:15"
  }
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[note-overview](#schemanote-overview)|false|none|笔记|账单所关联的笔记|
|»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»» username|string|true|none|用户名|none|
|»»» avatar|string|true|none|用户头像|头像图片URL|
|»»» biography|string|false|none|个人简介|此项非必须|
|»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»» state|object|false|none||none|
|»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»» title|string|true|none|标题|笔记标题|
|»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»» view_num|string|true|none|浏览量|none|
|»» star_num|string|true|none|收藏量|none|
|»» comment_num|string|true|none|评论量|none|
|»» create_time|string|true|none|创建时间|笔记创建的时间|
|»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 根据ID查询某一笔记

GET /notes/{note_id}

查看笔记的具体内容。
- 当用户未登录时， `state` 字段不返回
- 当笔记不属于当前用户时， `configuration` 字段不返回

注：用户仅可查看公开、免费权限或者本人的笔记

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|note_id|path|string| 是 ||笔记ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "overview": {
      "id": "420000197003059826",
      "author": {
        "id": "710000197612258141",
        "username": "黄涛",
        "avatar": "http://dummyimage.com/125x125",
        "biography": "为海家层提条为强热派原期美级连象。",
        "gender": "未知",
        "follows_num": "2186",
        "followers_num": "1483亿",
        "state": {
          "follow_status": false,
          "self_status": true
        }
      },
      "title": "业近都东",
      "cover": "http://dummyimage.com/240x400",
      "view_num": "734w",
      "star_num": "8691",
      "comment_num": "848w",
      "create_time": "2017-06-06 04:07:30",
      "update_time": "2024-03-28 15:51:23"
    },
    "content": "头门直件光广系军支资层程相事有。他么快交们象间得体列会常收六。增按证意该听气处例资住照格里马。入多那业一计论技会节通先划里如细。型条海方节看越流权国养厂样近要。治安书这写火调运省着专所究制层问许。话不积格多改拉内该情已法高政织应。示实持真这土院他风市广据产样解。位结反段路中律非土非再义。深度划花非支我气难率万到度只非统求。料行社信有下克他色织开信社外干如说。育切过段列口拉商际习领加天身并厂。养做样北何山常族段省被细记战。情书声选二等从明阶具说再那眼己斯目。队包细因争与研起利算利务变生飞。步别队积越便着种如它么太量群美八住。因无目变二整分果位电于整场位级使。感米整别能亲却几标打阶七口称织想精。场消保目委边色别条眼南总员想建必。手引育值者于东外或越处基达。代式改向上市消按它效白场光商清。育完部却九重族程确则命集。元联干已社运把列素党状能调动过。平油作学那速光打复存其产导林我话极。细料确形往部主式自放标声第林。干本重参群且今心派社影革资前活严业精。节较直工无价周外共划然状性海表斯。性单极养决任型则界县加列量下音设见。增而色道很并几技近约前引点。极委有之较和收权子复专给四月该革领。回设原基难说该线则众次铁同战在区选。被构油情还反安治体料书业音程员压。半中点角解如空种成史公外造式说速。务应给然组机单气时员强统其际总高真。真步行名生该厂科据适收王进写列。天头看热该织中条规青七作你观。以如增节风不九即必府热对工交与石技因。更难者知委经如物儿期效代办分两利。心三带写主热效车力等维红科。么各米观圆对面精半火工干压矿派条。起引任于指接图条选张住音海。两专治运联流育七利造与十海验须指。声面日或则生类传列方属对只。即清去没矿律般着每行已制北线比。许六争连每一使计成到出情家。变组活切全眼接然响各它定整。圆我规整进意又活好率前拉众接者资。过斯特称须积运去红科部他世进切。正品内件白所则义利以所问。习段叫权土度例看斗记由些此确它。",
    "state": {
      "star_status": true,
      "owner_status": true,
      "favorite_list": [
        {
          "id": "500000197504196722",
          "name": "队干七写具则",
          "description": "极放过放情经图温角外对史年入史比。",
          "num": 9,
          "create_time": "2001-10-18 12:21:47",
          "state": {
            "owner_status": true
          },
          "configuration": {
            "public_config": true
          }
        }
      ]
    },
    "configuration": {
      "public_config": true,
      "free_config": 512306
    }
  }
}
```

```json
{
  "code": 200,
  "data": {
    "overview": {
      "id": "330000197802019458",
      "author": {
        "id": "310000201710264347",
        "username": "顾秀英",
        "avatar": "http://dummyimage.com/180x150",
        "biography": "资却就张见清厂马成市色周听机原地。",
        "gender": "男",
        "follows_num": "9507",
        "followers_num": "5397万",
        "state": {
          "follow_status": false,
          "self_status": false
        }
      },
      "title": "月行定类写",
      "cover": "http://dummyimage.com/240x400",
      "view_num": "8434",
      "star_num": "3812w",
      "comment_num": "2887w",
      "create_time": "2019-08-15 10:35:42",
      "update_time": "2024-03-28 15:52:45"
    },
    "collection_list": [
      {
        "id": "620000198911218273",
        "name": "段八特",
        "cover": "http://dummyimage.com/336x280",
        "description": "话从红层段斗空接报要消感积强。",
        "num": 95,
        "star_num": "3174w",
        "create_time": "2016-02-25 02:18:08",
        "state": {
          "star_status": false,
          "favorite_list": [
            {
              "id": "440000200208114735",
              "name": "到名好分八",
              "description": "关导知界情记分能受子至书化议。",
              "num": 96,
              "create_time": "1996-08-27 06:50:15",
              "state": {
                "owner_status": true
              },
              "configuration": {
                "public_config": true
              }
            }
          ],
          "owner_status": true
        },
        "configuration": {
          "public_config": true
        }
      },
      {
        "id": "440000197711049059",
        "name": "容题作快时",
        "cover": "http://dummyimage.com/336x280",
        "description": "思向太音人及将究结率新里大证真。",
        "num": 65,
        "star_num": "5368w",
        "create_time": "1985-01-05 13:09:16",
        "state": {
          "star_status": false,
          "favorite_list": [
            {
              "id": "810000200612141726",
              "name": "开没积天数",
              "description": "则照需众报率反解许儿种段阶。",
              "num": 37,
              "create_time": "2023-09-11 22:09:42",
              "state": {
                "owner_status": true
              },
              "configuration": {
                "public_config": false
              }
            },
            {
              "id": "130000202402158623",
              "name": "义单点重相构",
              "description": "受号红四如划处全别由道加设飞。",
              "num": 85,
              "create_time": "2002-12-01 10:45:06",
              "state": {
                "owner_status": false
              },
              "configuration": {
                "public_config": false
              }
            }
          ],
          "owner_status": false
        },
        "configuration": {
          "public_config": true
        }
      }
    ],
    "content": "战标非何现海级去切较值多。内两产思经说两只华治每见离节向原。历斗间被外白特小北保安通张先按。形区此并从候置取织不流意或数。干须对证五业走界值位名适写山路使土。据算满应清般由特据那计改油间政金理圆。金图关派如组以行天强生制改值除受。明月心成之上了六党的意题于性们。结要集科眼文上级名技身关增千。如且情叫省就或三十织特般领十难根。联海结集或今理管周界打求利段响组长听。导龙它利石十应拉往者题它参。再识影带命问重断统斯电口例速花月消。省例识东加空再速政号天用价收组。着名白华人再除便能目出立增发价度。半十民重直把改受得开都段度民收音。术达话多观全组备劳委十质传界强通地。或些和利总第南长空角什员面该。路指热且任离只活革家立增车方达约法。南步合装何论并决广火因他基。际相形多内边主会自般还合组。先须响者火本党史人各领都后亲。历法究关般战过是路共使今人。又只下已度制农光你叫消例。图件水存切以示土群规外应。之住类支更史平集定想其消参料知又这选。意制存干产求记路儿便细次精一。步则证条且实划每济书器感加适消表。维眼内们面马美此区办已议目除这联重。压构代石高积定经着三至严划。于热间却来再度候其制京定志当张。院叫事领之约叫即多东保调据列。劳线却战该指格建子或们个式点委。重段群白今头备又问科四织对成。龙质细实品后或热得该做部新美之公构造。工层会称及先决器的次做切空日。已后约联标全原白式热六片素历得。才步示结影斯入先并斗养式住。证过象只世正党天相养划本在农但。众花少维重效越和圆或除论片象意。列称义我放干身法九还族种极电张上动千。便会容满质及际音算据电国通每管阶标。政里计回运难查京气各九教院法交计只。据交具家毛文民员为业非亲组。矿求治人节治市条当市很线到于。压革场理统都精应史那京五明老。六听易资明此场族家论马温路斯理众。具打什深实积是有林当次无越。亲四立领战二却应维状圆长效格部极我。据将装车规市根正构技号对劳口影石。",
    "state": {
      "star_status": false,
      "owner_status": false,
      "favorite_list": []
    }
  }
}
```

> 权限不足

```json
{
  "msg": "用户权限不足"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|权限不足|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[note](#schemanote)|false|none|数据|none|
|»» overview|[note-overview](#schemanote-overview)|true|none|笔记|账单所关联的笔记|
|»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»» username|string|true|none|用户名|none|
|»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»» biography|string|false|none|个人简介|此项非必须|
|»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»» state|object|false|none||none|
|»»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»»» title|string|true|none|标题|笔记标题|
|»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»» view_num|string|true|none|浏览量|none|
|»»» star_num|string|true|none|收藏量|none|
|»»» comment_num|string|true|none|评论量|none|
|»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»» content|string|true|none|内容|Markdown内容|
|»» state|[note-state](#schemanote-state)|false|none|当前用户状态量|此项非必须|
|»»» star_status|boolean|true|none|是否收藏|当前用户是否收藏|
|»»» owner_status|boolean|true|none|是否拥有|是否为当前拥有|
|»» configuration|[note-configuration](#schemanote-configuration)|false|none|笔记配置项|仅作者为当前用户时返回，此项非必须|
|»»» public_config|boolean|true|none|是否公开|是否公开|
|»»» free_config|integer|true|none|是否免费|若值为0，则免费，其余则收费，单位：分|

状态码 **403**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

## POST 修改笔记

POST /notes/{note_id}

> Body 请求参数

```yaml
title: string
cover: cmMtdXBsb2FkLTE3MTE3Nzc5ODQ1NDctNQ==/comment.jpg
content: string
publicOption: true
freeOption: 0

```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|note_id|path|string| 是 ||none|
|body|body|object| 否 ||none|
|» title|body|string| 否 ||笔记标题|
|» cover|body|string(binary)| 否 ||笔记封面，非必填项|
|» content|body|string| 否 ||笔记内容|
|» publicOption|body|boolean| 否 ||配置选项：笔记是否公开|
|» freeOption|body|integer| 否 ||配置选项：笔记是否免费。若值为0，则免费，其余则收费，单位：分|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "id": "220000197709031318",
    "author": {
      "id": "820000199602298187",
      "username": "谭霞",
      "avatar": "http://dummyimage.com/234x60",
      "biography": "完之质社已位区周出住集需候部成个。",
      "gender": "男",
      "follows_num": "655",
      "followers_num": "252万",
      "state": {
        "follow_status": false,
        "self_status": true
      }
    },
    "title": "马程然装制便",
    "cover": "http://dummyimage.com/120x240",
    "view_num": "0",
    "star_num": "0",
    "comment_num": "0",
    "create_time": "2024-03-30 16:51:15",
    "update_time": "2024-03-30 16:51:15"
  }
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[note-overview](#schemanote-overview)|false|none|笔记|账单所关联的笔记|
|»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»» username|string|true|none|用户名|none|
|»»» avatar|string|true|none|用户头像|头像图片URL|
|»»» biography|string|false|none|个人简介|此项非必须|
|»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»» state|object|false|none||none|
|»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»» title|string|true|none|标题|笔记标题|
|»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»» view_num|string|true|none|浏览量|none|
|»» star_num|string|true|none|收藏量|none|
|»» comment_num|string|true|none|评论量|none|
|»» create_time|string|true|none|创建时间|笔记创建的时间|
|»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## DELETE 删除笔记

DELETE /notes/{note_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|note_id|path|string| 是 ||笔记ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 分页获取笔记合集列表

GET /collections

注：
- 用户未登录或者登录过期，不返回 `state` 字段
- 当收藏夹不属于当前用户时，不返回 `configuration` 字段

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|note_id|query|string| 是 ||笔记ID|
|page_num|query|integer| 是 ||页码，从1开始|
|page_size|query|integer| 是 ||每页数量|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 1,
    "list": [
      {
        "id": "320000199702156807",
        "name": "可示利开张易西",
        "cover": "http://dummyimage.com/728x90",
        "description": "非头至阶活传老却统从等年内备布两。",
        "num": 96,
        "star_num": "7112w",
        "create_time": "1974-01-06 15:19:16",
        "state": {
          "star_status": false,
          "favorite_list": [
            {
              "id": "650000197704228947",
              "name": "到能龙回须",
              "description": "点且间上取团美向特因论话回清人适党导。",
              "num": 40,
              "create_time": "2013-09-16 05:44:24",
              "state": {
                "owner_status": true
              },
              "configuration": {
                "public_config": true,
                "default_config": "false"
              }
            }
          ],
          "owner_status": true
        },
        "configuration": {
          "public_config": false
        }
      }
    ],
    "size": 1,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

> 权限不足

```json
{
  "msg": "用户权限不足"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|权限不足|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» pages|integer|true|none|总页数|none|
|»» page_num|integer|true|none|当前页码|none|
|»» page_size|integer|true|none|每页的数量|none|
|»» total|integer|true|none|总数|none|
|»» list|[[collection](#schemacollection)]|true|none|当前页的数据列表|none|
|»»» 合集|[collection](#schemacollection)|false|none|合集|数据项|
|»»»» id|string|true|none|合集ID|数字编号形式的合集ID|
|»»»» name|string|true|none|名称|合集名称|
|»»»» cover|string|true|none|封面|图片URL|
|»»»» description|string|false|none|简介|合集简介，此项非必须|
|»»»» num|integer|true|none|包含笔记数量|合集内拥有笔记的数量|
|»»»» star_num|string|true|none|收藏量|合集被收藏的数量|
|»»»» create_time|string|true|none|创建时间|合集创建时间|
|»»»» state|[collection-state](#schemacollection-state)|false|none|当前用户状态量|此项为非必须|
|»»»»» star_status|boolean|true|none|是否收藏|当前用户是否收藏该合集|
|»»»»» favorite_list|[[favorite](#schemafavorite)]|true|none|合集所属当前用户收藏夹列表|合集所在的收藏夹列表|
|»»»»»» 收藏夹|[favorite](#schemafavorite)|false|none|收藏夹|合集所在的收藏夹信息|
|»»»»»»» id|string|true|none|收藏夹ID|数字编号形式的收藏夹ID|
|»»»»»»» name|string|true|none|名称|收藏夹名称|
|»»»»»»» description|string|false|none|简介|收藏夹简介，此项非必须|
|»»»»»»» num|integer|true|none|笔记数量|收藏夹所包含笔记数量|
|»»»»»»» create_time|string|true|none|创建时间|收藏夹创建时间|
|»»»»»»» state|[favorite-state](#schemafavorite-state)|false|none|当前用户状态量|此项为非必须|
|»»»»»»»» owner_status|boolean|true|none|是否拥有|收藏夹是否属于自己|
|»»»»»»» configuration|[favorite-configuration](#schemafavorite-configuration)|false|none|收藏夹配置|仅作者为当前用户时返回，此项非必须|
|»»»»»»»» public_config|boolean|true|none|是否公开|是否公开|
|»»»»»»»» default_config|boolean|true|none|是否是默认收藏夹|是否是默认收藏夹|
|»»»»» owner_status|boolean|true|none|是否拥有|合集是否属于自己|
|»»»» configuration|[collection-configuration](#schemacollection-configuration)|false|none|合集配置项|仅作者为当前用户时返回，此项非必须|
|»»»»» public_config|boolean|true|none|是否公开|是否公开|
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

状态码 **403**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

## GET 获取AI笔记总结

GET /notes/ai/{note_id}

暂定将通过 `SSE` 协议返回

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|note_id|path|string| 是 ||笔记ID|

> 返回示例

> 200 Response

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

# 评论

## GET 分页获取笔记评论

GET /comments

### 此接口返回**一级评论**

评论分为两种：一级评论和二级评论（即：回复）
- 一级评论是对笔记内容的直接评价
- 二级评论可以是**对一级评论的回复**，同样也可以是**对二级评论的回复**

![comment.jpg](https://api.apifox.com/api/v1/projects/4232631/resources/431242/image-preview)

注：用户未登录时， `state` 字段不返回。

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|note_id|query|string| 是 ||笔记ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 4,
    "list": [
      {
        "id": "510000198707064080",
        "user": {
          "id": "410000201307257220",
          "username": "程勇",
          "avatar": "http://dummyimage.com/250x250",
          "biography": "单土过至儿儿毛些七光元制格此示。",
          "gender": "女",
          "follows_num": "5056",
          "followers_num": "6722",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "提育被以要实六要周九交数音厂使场。",
        "image_list": [
          "http://dummyimage.com/88x31",
          "http://dummyimage.com/468x60",
          "http://dummyimage.com/468x60"
        ],
        "agree_num": 9178,
        "reply_num": 5,
        "create_time": "2024-03-28 20:31:31",
        "state": {
          "agree_status": false
        }
      },
      {
        "id": "320000200004280339",
        "user": {
          "id": "420000197212291533",
          "username": "卢娟",
          "avatar": "http://dummyimage.com/240x400",
          "biography": "置则原算响积复水完被已同已石回格效金。",
          "gender": "未知",
          "follows_num": "7702",
          "followers_num": "7728亿",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "生局七工做史干路好他面成除其体动。",
        "audio": "https://cdn.demiphea.com/demiphea.mp3",
        "agree_num": 3860,
        "reply_num": 0,
        "create_time": "2024-03-28 20:31:31",
        "state": {
          "agree_status": false
        }
      },
      {
        "id": "450000197805131186",
        "user": {
          "id": "230000198504111011",
          "username": "方刚",
          "avatar": "http://dummyimage.com/180x150",
          "biography": "报业金军适支支根温拉天回织七真学。",
          "gender": "女",
          "follows_num": "6365",
          "followers_num": "251万",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "易白九有导集队化至结立化定特。",
        "note": {
          "id": "150000202104238565",
          "author": {
            "id": "230000201507282355",
            "username": "姜明",
            "avatar": "http://dummyimage.com/125x125",
            "biography": "进学会查证里切至风将便土需。",
            "gender": "未知",
            "follows_num": "3132",
            "followers_num": "1079万",
            "state": {
              "follow_status": false,
              "self_status": false
            }
          },
          "title": "切想手叫思基",
          "cover": "http://dummyimage.com/250x250",
          "view_num": "7547",
          "star_num": "8845w",
          "comment_num": "9303w",
          "create_time": "2012-09-19 10:29:55",
          "update_time": "2024-03-28 20:31:31"
        },
        "agree_num": 6536,
        "reply_num": 3,
        "create_time": "2024-03-28 20:31:31",
        "state": {
          "agree_status": true
        }
      },
      {
        "id": "450000197805131187",
        "user": {
          "id": "230000198504111011",
          "username": "方刚",
          "avatar": "http://dummyimage.com/180x150",
          "biography": "报业金军适支支根温拉天回织七真学。",
          "gender": "女",
          "follows_num": "6365",
          "followers_num": "251万",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "对你撒娇看你文件的撒。",
        "video": "https://cdn.demiphea.com/demiphea.mp4",
        "agree_num": 6536,
        "reply_num": 3,
        "create_time": "2024-03-28 20:31:31",
        "state": {
          "agree_status": false
        }
      }
    ],
    "size": 4,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

> 权限不足

```json
{
  "msg": "用户权限不足"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|权限不足|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» pages|integer|true|none|总页数|none|
|»» page_num|integer|true|none|当前页码|none|
|»» page_size|integer|true|none|每页的数量|none|
|»» total|integer|true|none|总数|none|
|»» list|[[comment](#schemacomment)]|true|none|当前页的数据列表|none|
|»»» 评论|[comment](#schemacomment)|false|none|评论|数据项|
|»»»» id|string|true|none|评论ID|数字编号形式的评论ID|
|»»»» user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»» username|string|true|none|用户名|none|
|»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»» state|object|false|none||none|
|»»»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»»»» text|string|false|none|评论文本|非必须项|
|»»»» image_list|[string]|false|none|评论图片|评论图片列表，非必须项|
|»»»» audio|string|false|none|评论语音|语音URL|
|»»»» video|string|false|none|评论视频|视频URL|
|»»»» note|[note-overview](#schemanote-overview)|false|none|笔记|账单所关联的笔记|
|»»»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»»» username|string|true|none|用户名|none|
|»»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»»» state|object|false|none||none|
|»»»»» title|string|true|none|标题|笔记标题|
|»»»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»»»» view_num|string|true|none|浏览量|none|
|»»»»» star_num|string|true|none|收藏量|none|
|»»»»» comment_num|string|true|none|评论量|none|
|»»»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»»»» agree_num|string|true|none|赞同数|评论的点赞数量|
|»»»» reply_num|string|true|none|回复数|评论回复的数量|
|»»»» create_time|string|true|none|评论时间|评论发布时间|
|»»»» state|[comment-state](#schemacomment-state)|true|none|当前用户状态量|none|
|»»»»» agree_status|boolean|true|none|是否点赞|当前用户是否点赞该评论|
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

状态码 **403**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

## POST 发布评论

POST /comments

笔记的（单条）评论信息，**图片、语音、视频和关联笔记只能选其一**。

> Body 请求参数

```json
{
  "noteId": "string",
  "parentId": "string",
  "text": "string",
  "imageList": [
    "string"
  ],
  "audio": "string",
  "video": "string",
  "linkNoteId": "string"
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» noteId|body|string| 是 | 笔记ID|评论所在笔记ID|
|» parentId|body|string| 否 | 父评论ID|若为一级评论则此项不填|
|» text|body|string| 否 | 评论文本|none|
|» imageList|body|[string]| 否 | 评论图片|评论的图片列表|
|» audio|body|string| 否 | 评论音频|音频URL|
|» video|body|string| 否 | 评论视频|视频URL|
|» linkNoteId|body|string| 否 | 评论关联笔记|关联笔记ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "id": "310000200602017846",
    "user": {
      "id": "120000200611105316",
      "username": "徐涛",
      "avatar": "http://dummyimage.com/250x250",
      "biography": "科火现照天己效出广发美县县明果。",
      "gender": "女",
      "follows_num": "2224",
      "followers_num": "1983亿",
      "state": {
        "follow_status": false,
        "self_status": true
      }
    },
    "text": "眼志自件你往律张转点样重而低并高。",
    "image_list": [
      "http://dummyimage.com/300x600"
    ],
    "agree_num": "0",
    "reply_num": "0",
    "create_time": "2024-03-30 17:10:31",
    "state": {
      "agree_status": false
    }
  }
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[comment](#schemacomment)|false|none|评论|数据项|
|»» id|string|true|none|评论ID|数字编号形式的评论ID|
|»» user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»» username|string|true|none|用户名|none|
|»»» avatar|string|true|none|用户头像|头像图片URL|
|»»» biography|string|false|none|个人简介|此项非必须|
|»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»» state|object|false|none||none|
|»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»» text|string|false|none|评论文本|非必须项|
|»» image_list|[string]|false|none|评论图片|评论图片列表，非必须项|
|»» audio|string|false|none|评论语音|语音URL|
|»» video|string|false|none|评论视频|视频URL|
|»» note|[note-overview](#schemanote-overview)|false|none|笔记|账单所关联的笔记|
|»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»» username|string|true|none|用户名|none|
|»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»» biography|string|false|none|个人简介|此项非必须|
|»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»» state|object|false|none||none|
|»»» title|string|true|none|标题|笔记标题|
|»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»» view_num|string|true|none|浏览量|none|
|»»» star_num|string|true|none|收藏量|none|
|»»» comment_num|string|true|none|评论量|none|
|»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»» agree_num|string|true|none|赞同数|评论的点赞数量|
|»» reply_num|string|true|none|回复数|评论回复的数量|
|»» create_time|string|true|none|评论时间|评论发布时间|
|»» state|[comment-state](#schemacomment-state)|true|none|当前用户状态量|none|
|»»» agree_status|boolean|true|none|是否点赞|当前用户是否点赞该评论|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 分页获取评论的回复

GET /sub_comments

### 此接口返回**二级评论**

评论分为两种：一级评论和二级评论（即：回复）
- 一级评论是对笔记内容的直接评价
- 二级评论可以是**对一级评论的回复**，同样也可以是**对二级评论的回复**

![comment.jpg](https://api.apifox.com/api/v1/projects/4232631/resources/431242/image-preview)

针对于二级评论，通过 `parent_comment` 字段来指定回复的对象：若未返回 `parent_comment` 字段，则此二级评论是**对一级评论的回复**；若返回了 `parent_comment` 字段，则此二级评论是**对二级评论的回复**。

注：用户未登录时， `state` 字段不返回。

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|comment_id|query|string| 是 ||评论ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 6,
    "list": [
      {
        "id": "410000202101079582",
        "user": {
          "id": "140000199502287438",
          "username": "傅平",
          "avatar": "http://dummyimage.com/120x240",
          "biography": "那证下花越但确行往交生物据江包。",
          "gender": "女",
          "follows_num": "9597",
          "followers_num": "4104",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "江角由导长高科此整办克说感手料。",
        "image_list": [
          "http://dummyimage.com/240x400",
          "http://dummyimage.com/300x250"
        ],
        "agree_num": "8782",
        "reply_num": "0",
        "create_time": "2024-03-28 21:24:00",
        "state": {
          "agree_status": false
        }
      },
      {
        "id": "990000201812041837",
        "user": {
          "id": "810000198108075950",
          "username": "谭超",
          "avatar": "http://dummyimage.com/468x60",
          "biography": "受同才世线见得着层院消战问。",
          "gender": "女",
          "follows_num": "6718",
          "followers_num": "9895亿",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "文长国离人据系系效该白南几省照离素。",
        "audio": "https://cdn.demiphea.com/demiphea.mp3",
        "agree_num": "2492",
        "reply_num": "0",
        "create_time": "2024-03-28 21:24:00",
        "state": {
          "agree_status": true
        }
      },
      {
        "id": "360000199809286086",
        "user": {
          "id": "620000200905138840",
          "username": "郑涛",
          "avatar": "http://dummyimage.com/125x125",
          "biography": "亲色眼确条今专存必色毛必装至。",
          "gender": "女",
          "follows_num": "8488",
          "followers_num": "4088万",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "定机较派设安眼规拉确着开领公三观议。",
        "video": "https://cdn.demiphea.com/demiphea.mp4",
        "agree_num": "7090",
        "reply_num": "0",
        "create_time": "2024-03-28 21:24:00",
        "state": {
          "agree_status": false
        }
      },
      {
        "id": "620000201709224711",
        "user": {
          "id": "420000197802016574",
          "username": "蔡磊",
          "avatar": "http://dummyimage.com/125x125",
          "biography": "面转而数作最养级基声矿话根子。",
          "gender": "未知",
          "follows_num": "9545",
          "followers_num": "6725亿",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "格教展里月查技农约导式二知党还运情。",
        "note": {
          "id": "120000199404296820",
          "author": {
            "id": "350000201311145284",
            "username": "李超",
            "avatar": "http://dummyimage.com/468x60",
            "biography": "育议查自别重通有题报标改其体名许。",
            "gender": "男",
            "follows_num": "7895",
            "followers_num": "5115",
            "state": {
              "follow_status": false,
              "self_status": false
            }
          },
          "title": "原把去省",
          "cover": "http://dummyimage.com/160x600",
          "view_num": "8203w",
          "star_num": "687",
          "comment_num": "5222w",
          "create_time": "2012-11-07 20:43:48",
          "update_time": "2024-03-28 21:24:00"
        },
        "agree_num": "9660w",
        "reply_num": "0",
        "create_time": "2024-03-28 21:24:00",
        "state": {
          "agree_status": false
        }
      },
      {
        "id": "620000197311077361",
        "user": {
          "id": "210000199505027533",
          "username": "陈平",
          "avatar": "http://dummyimage.com/240x400",
          "biography": "率场多眼热真段提其拉青团证步。",
          "gender": "未知",
          "follows_num": "9861",
          "followers_num": "6797",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "查西层确适指住维需接热调被什写。",
        "note": {
          "id": "820000200808288457",
          "author": {
            "id": "130000201808165570",
            "username": "沈桂英",
            "avatar": "http://dummyimage.com/120x600",
            "biography": "工他间年见元展然个受示此解区。",
            "gender": "女",
            "follows_num": "1054",
            "followers_num": "7976亿",
            "state": {
              "follow_status": false,
              "self_status": false
            }
          },
          "title": "位战知不法",
          "cover": "http://dummyimage.com/468x60",
          "view_num": "1647w",
          "star_num": "4791",
          "comment_num": "2811w",
          "create_time": "1983-05-15 19:48:10",
          "update_time": "2024-03-28 21:24:00"
        },
        "agree_num": "9077",
        "reply_num": "1",
        "create_time": "2024-03-28 21:24:00",
        "state": {
          "agree_status": false
        }
      },
      {
        "id": "710000197004212246",
        "user": {
          "id": "450000197109092259",
          "username": "阎霞",
          "avatar": "http://dummyimage.com/336x280",
          "biography": "意点大该格法世应克新而律场史。",
          "gender": "女",
          "follows_num": "4076",
          "followers_num": "610万",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "气温被包变研土方亲容复工长再。",
        "image_list": [
          "http://dummyimage.com/728x90"
        ],
        "agree_num": "1504",
        "reply_num": "0",
        "create_time": "2024-03-28 21:24:00",
        "state": {
          "agree_status": true
        },
        "parent_comment": {
          "id": "620000197311077361",
          "user": {
            "id": "210000199505027533",
            "username": "陈平",
            "avatar": "http://dummyimage.com/240x400",
            "biography": "率场多眼热真段提其拉青团证步。",
            "gender": "未知",
            "follows_num": "9861",
            "followers_num": "6797",
            "state": {
              "follow_status": false,
              "self_status": false
            }
          },
          "text": "查西层确适指住维需接热调被什写。",
          "note": {
            "id": "820000200808288457",
            "author": {
              "id": "130000201808165570",
              "username": "沈桂英",
              "avatar": "http://dummyimage.com/120x600",
              "biography": "工他间年见元展然个受示此解区。",
              "gender": "女",
              "follows_num": "1054",
              "followers_num": "7976亿",
              "state": {
                "follow_status": false,
                "self_status": false
              }
            },
            "title": "位战知不法",
            "cover": "http://dummyimage.com/468x60",
            "view_num": "1647w",
            "star_num": "4791",
            "comment_num": "2811w",
            "create_time": "1983-05-15 19:48:10",
            "update_time": "2024-03-28 21:24:00"
          },
          "agree_num": "9077",
          "reply_num": "1",
          "create_time": "2024-03-28 21:24:00",
          "state": {
            "agree_status": false
          }
        }
      }
    ],
    "size": 6,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

> 权限不足

```json
{
  "msg": "用户权限不足"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|权限不足|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» pages|integer|true|none|总页数|none|
|»» page_num|integer|true|none|当前页码|none|
|»» page_size|integer|true|none|每页的数量|none|
|»» total|integer|true|none|总数|none|
|»» list|[object]|true|none|当前页的数据列表|none|
|»»» id|string|true|none|评论ID|数字编号形式的评论ID|
|»»» user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»» username|string|true|none|用户名|none|
|»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»» biography|string|false|none|个人简介|此项非必须|
|»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»» state|object|false|none||none|
|»»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»»» text|string|false|none|评论文本|非必须项|
|»»» image_list|[string]|false|none|评论图片|评论图片列表，非必须项|
|»»» audio|string|false|none|评论语音|语音URL|
|»»» video|string|false|none|评论视频|视频URL|
|»»» note|[note-overview](#schemanote-overview)|false|none|笔记|账单所关联的笔记|
|»»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»» username|string|true|none|用户名|none|
|»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»» state|object|false|none||none|
|»»»» title|string|true|none|标题|笔记标题|
|»»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»»» view_num|string|true|none|浏览量|none|
|»»»» star_num|string|true|none|收藏量|none|
|»»»» comment_num|string|true|none|评论量|none|
|»»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»»» agree_num|string|true|none|赞同数|评论的点赞数量|
|»»» reply_num|string|true|none|回复数|评论回复的数量|
|»»» create_time|string|true|none|评论时间|评论发布时间|
|»»» state|[comment-state](#schemacomment-state)|true|none|当前用户状态量|none|
|»»»» agree_status|boolean|true|none|是否点赞|当前用户是否点赞该评论|
|»»» parent_comment|[comment](#schemacomment)|false|none|评论|数据项|
|»»»» id|string|true|none|评论ID|数字编号形式的评论ID|
|»»»» user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»» username|string|true|none|用户名|none|
|»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»» state|object|false|none||none|
|»»»» text|string|false|none|评论文本|非必须项|
|»»»» image_list|[string]|false|none|评论图片|评论图片列表，非必须项|
|»»»» audio|string|false|none|评论语音|语音URL|
|»»»» video|string|false|none|评论视频|视频URL|
|»»»» note|[note-overview](#schemanote-overview)|false|none|笔记|账单所关联的笔记|
|»»»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»» title|string|true|none|标题|笔记标题|
|»»»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»»»» view_num|string|true|none|浏览量|none|
|»»»»» star_num|string|true|none|收藏量|none|
|»»»»» comment_num|string|true|none|评论量|none|
|»»»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»»»» agree_num|string|true|none|赞同数|评论的点赞数量|
|»»»» reply_num|string|true|none|回复数|评论回复的数量|
|»»»» create_time|string|true|none|评论时间|评论发布时间|
|»»»» state|[comment-state](#schemacomment-state)|true|none|当前用户状态量|none|
|»»»»» agree_status|boolean|true|none|是否点赞|当前用户是否点赞该评论|
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

状态码 **403**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

## GET 评论追溯

GET /comments/trace

### 使用
此接口适用于如下场景：当用户收到评论通知时，点击评论通知按钮跳转至相应的笔记，并弹窗展示接口返回的评论上下文（参考哔哩哔哩App），也可以再评论区置顶展示接口返回的评论上下文（参考抖音App）。

### 返回结构说明
此接口返回追溯出的评论上下文列表。追溯原理是评论树的递归遍历，因此列表第一项为评论数的根节点，即**一级评论**，其余项为**二级评论**。

![comment_tree.png](https://api.apifox.com/api/v1/projects/4232631/resources/431243/image-preview)

---
评论分为两种：一级评论和二级评论（即：回复）
- 一级评论是对笔记内容的直接评价
- 二级评论可以是**对一级评论的回复**，同样也可以是**对二级评论的回复**

![comment.jpg](https://api.apifox.com/api/v1/projects/4232631/resources/431242/image-preview)

针对于二级评论，通过 `parent_comment` 字段来指定回复的对象：若未返回 `parent_comment` 字段，则此二级评论是**对一级评论的回复**；若返回了 `parent_comment` 字段，则此二级评论是**对二级评论的回复**。

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|comment_id|query|string| 是 ||评论ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "note": {
      "id": "420000200504251618",
      "author": {
        "id": "610000198110223521",
        "username": "方超",
        "avatar": "http://dummyimage.com/234x60",
        "biography": "科展际非组建联质节确将气正。",
        "gender": "未知",
        "follows_num": "3489",
        "followers_num": "9219万",
        "state": {
          "follow_status": false,
          "self_status": false
        }
      },
      "title": "年提四",
      "cover": "http://dummyimage.com/88x31",
      "view_num": "3751w",
      "star_num": "8588w",
      "comment_num": "4273w",
      "create_time": "2023-05-13 13:48:46",
      "update_time": "2024-03-29 00:02:10"
    },
    "comment_trace_list": [
      {
        "id": "150000202312192127",
        "user": {
          "id": "440000197404016265",
          "username": "孙平",
          "avatar": "http://dummyimage.com/336x280",
          "biography": "志做离东可运党国边计公面商象。",
          "gender": "女",
          "follows_num": "8753",
          "followers_num": "1358",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "党为较进金这都毛即方矿合去场。",
        "image_list": [
          "http://dummyimage.com/120x240",
          "http://dummyimage.com/125x125",
          "http://dummyimage.com/468x60"
        ],
        "agree_num": "9646",
        "reply_num": "2",
        "create_time": "2024-03-28 23:47:13",
        "state": {
          "agree_status": true
        }
      },
      {
        "id": "460000197505132832",
        "user": {
          "id": "320000198805319969",
          "username": "胡明",
          "avatar": "http://dummyimage.com/120x240",
          "biography": "许级知难路下部命标总还意于资明因社与。",
          "gender": "未知",
          "follows_num": "9458",
          "followers_num": "1710",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "强进理多半领院除育员事华东京。",
        "video": "https://cdn.demiphea.com/demiphea.mp4",
        "agree_num": "3088",
        "reply_num": "1",
        "create_time": "2024-03-28 23:47:13",
        "state": {
          "agree_status": false
        }
      },
      {
        "id": "520000200808157309",
        "user": {
          "id": "460000201102285075",
          "username": "程明",
          "avatar": "http://dummyimage.com/234x60",
          "biography": "教价备斗即工石至给院小三。",
          "gender": "女",
          "follows_num": "8455",
          "followers_num": "4013万",
          "state": {
            "follow_status": false,
            "self_status": false
          }
        },
        "text": "件权术道因效完带重接候等政便白。",
        "note": {
          "id": "150000198610148125",
          "author": {
            "id": "370000198501281331",
            "username": "顾明",
            "avatar": "http://dummyimage.com/180x150",
            "biography": "展类思参千在类和布总电米把革维。",
            "gender": "未知",
            "follows_num": "1327",
            "followers_num": "7941万",
            "state": {
              "follow_status": false,
              "self_status": false
            }
          },
          "title": "装导必条利京",
          "cover": "http://dummyimage.com/120x90",
          "view_num": "1823",
          "star_num": "9677",
          "comment_num": "5258w",
          "create_time": "1983-01-22 04:49:16",
          "update_time": "2024-03-28 23:47:13"
        },
        "agree_num": "2226",
        "reply_num": "0",
        "create_time": "2024-03-28 23:47:13",
        "state": {
          "agree_status": true
        },
        "parent_comment": {
          "id": "460000197505132832",
          "user": {
            "id": "320000198805319969",
            "username": "胡明",
            "avatar": "http://dummyimage.com/120x240",
            "biography": "许级知难路下部命标总还意于资明因社与。",
            "gender": "未知",
            "follows_num": "9458",
            "followers_num": "1710",
            "state": {
              "follow_status": false,
              "self_status": false
            }
          },
          "text": "强进理多半领院除育员事华东京。",
          "video": "https://cdn.demiphea.com/demiphea.mp4",
          "agree_num": "3088",
          "reply_num": "1",
          "create_time": "2024-03-28 23:47:13",
          "state": {
            "agree_status": false
          }
        }
      }
    ]
  }
}
```

> 权限不足

```json
{
  "msg": "用户权限不足"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|权限不足|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» note|[note-overview](#schemanote-overview)|true|none|笔记|账单所关联的笔记|
|»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»» username|string|true|none|用户名|none|
|»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»» biography|string|false|none|个人简介|此项非必须|
|»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»» state|object|false|none||none|
|»»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»»» title|string|true|none|标题|笔记标题|
|»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»» view_num|string|true|none|浏览量|none|
|»»» star_num|string|true|none|收藏量|none|
|»»» comment_num|string|true|none|评论量|none|
|»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»» comment_trace_list|[object]|true|none|追溯列表|none|
|»»» id|string|true|none|评论ID|数字编号形式的评论ID|
|»»» user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»» username|string|true|none|用户名|none|
|»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»» biography|string|false|none|个人简介|此项非必须|
|»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»» state|object|false|none||none|
|»»» text|string|false|none|评论文本|非必须项|
|»»» image_list|[string]|false|none|评论图片|评论图片列表，非必须项|
|»»» audio|string|false|none|评论语音|语音URL|
|»»» video|string|false|none|评论视频|视频URL|
|»»» note|[note-overview](#schemanote-overview)|false|none|笔记|账单所关联的笔记|
|»»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»» title|string|true|none|标题|笔记标题|
|»»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»»» view_num|string|true|none|浏览量|none|
|»»»» star_num|string|true|none|收藏量|none|
|»»»» comment_num|string|true|none|评论量|none|
|»»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»»» agree_num|string|true|none|赞同数|评论的点赞数量|
|»»» reply_num|string|true|none|回复数|评论回复的数量|
|»»» create_time|string|true|none|评论时间|评论发布时间|
|»»» state|[comment-state](#schemacomment-state)|true|none|当前用户状态量|none|
|»»»» agree_status|boolean|true|none|是否点赞|当前用户是否点赞该评论|
|»»» parent_comment|[comment](#schemacomment)|false|none|评论|数据项|
|»»»» id|string|true|none|评论ID|数字编号形式的评论ID|
|»»»» user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»» username|string|true|none|用户名|none|
|»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»» state|object|false|none||none|
|»»»» text|string|false|none|评论文本|非必须项|
|»»»» image_list|[string]|false|none|评论图片|评论图片列表，非必须项|
|»»»» audio|string|false|none|评论语音|语音URL|
|»»»» video|string|false|none|评论视频|视频URL|
|»»»» note|[note-overview](#schemanote-overview)|false|none|笔记|账单所关联的笔记|
|»»»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»» title|string|true|none|标题|笔记标题|
|»»»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»»»» view_num|string|true|none|浏览量|none|
|»»»»» star_num|string|true|none|收藏量|none|
|»»»»» comment_num|string|true|none|评论量|none|
|»»»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»»»» agree_num|string|true|none|赞同数|评论的点赞数量|
|»»»» reply_num|string|true|none|回复数|评论回复的数量|
|»»»» create_time|string|true|none|评论时间|评论发布时间|
|»»»» state|[comment-state](#schemacomment-state)|true|none|当前用户状态量|none|
|»»»»» agree_status|boolean|true|none|是否点赞|当前用户是否点赞该评论|

状态码 **403**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

## DELETE 删除评论

DELETE /comments/{comment_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|comment_id|path|string| 是 ||评论ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## POST 点赞评论

POST /comments/like

> Body 请求参数

```json
{
  "commentId": "string"
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» commentId|body|string| 是 | 评论ID|none|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## DELETE 取消点赞评论

DELETE /comments/like/{comment_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|comment_id|path|string| 是 ||评论ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

# 收藏夹

## GET 分页获取收藏夹列表

GET /favorites

如果未携带 `user_id` 参数，则**默认返回当前用户的收藏夹列表（前提是用户已登录）**。
注：
- 用户未登录或者登录过期，不返回 `state` 字段
- 当收藏夹不属于当前用户时，不返回 `configuration` 字段

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|user_id|query|string| 否 ||用户ID，非必填项|
|page_num|query|integer| 是 ||页码，从1开始|
|page_size|query|integer| 是 ||每页数量|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 1,
    "list": [
      {
        "id": "430000197911304884",
        "name": "价平看备必条",
        "description": "说间不区设取参交求学多收回基本化。",
        "num": 58,
        "create_time": "2013-03-01 19:16:01",
        "state": {
          "owner_status": true
        },
        "configuration": {
          "public_config": true,
          "default_config": "true"
        }
      }
    ],
    "size": 1,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

> 权限不足

```json
{
  "msg": "用户权限不足"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|权限不足|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» pages|integer|true|none|总页数|none|
|»» page_num|integer|true|none|当前页码|none|
|»» page_size|integer|true|none|每页的数量|none|
|»» total|integer|true|none|总数|none|
|»» list|[[favorite](#schemafavorite)]|true|none|当前页的数据列表|none|
|»»» 收藏夹|[favorite](#schemafavorite)|false|none|收藏夹|合集所在的收藏夹信息|
|»»»» id|string|true|none|收藏夹ID|数字编号形式的收藏夹ID|
|»»»» name|string|true|none|名称|收藏夹名称|
|»»»» description|string|false|none|简介|收藏夹简介，此项非必须|
|»»»» num|integer|true|none|笔记数量|收藏夹所包含笔记数量|
|»»»» create_time|string|true|none|创建时间|收藏夹创建时间|
|»»»» state|[favorite-state](#schemafavorite-state)|false|none|当前用户状态量|此项为非必须|
|»»»»» owner_status|boolean|true|none|是否拥有|收藏夹是否属于自己|
|»»»» configuration|[favorite-configuration](#schemafavorite-configuration)|false|none|收藏夹配置|仅作者为当前用户时返回，此项非必须|
|»»»»» public_config|boolean|true|none|是否公开|是否公开|
|»»»»» default_config|boolean|true|none|是否是默认收藏夹|是否是默认收藏夹|
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

状态码 **403**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

## POST 新建收藏夹（含拷贝）

POST /favorites

若指定 `favoriteId` 参数，则为拷贝收藏夹

> Body 请求参数

```json
{
  "name": "string",
  "description": "string",
  "configuration": {
    "publicConfig": true,
    "defaultConfig": false
  },
  "favoriteId": "string"
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» name|body|string| 是 | 收藏夹名称|none|
|» description|body|string| 否 | 收藏夹简介|none|
|» configuration|body|object| 否 | 收藏夹配置|none|
|»» publicConfig|body|boolean| 否 | 是否公开|是否公开|
|»» defaultConfig|body|boolean| 否 | 是否是默认收藏夹|是否是默认收藏夹|
|» favoriteId|body|string| 否 | 源收藏夹ID|要拷贝的收藏夹ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "id": "430000200808266423",
    "name": "展周列展团件",
    "description": "年半列维所步研料基更眼成强组装加问不。",
    "num": 0,
    "create_time": "2024-03-30 16:31:32",
    "state": {
      "owner_status": true
    },
    "configuration": {
      "public_config": true,
      "default_config": false
    }
  }
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[favorite](#schemafavorite)|false|none|收藏夹|合集所在的收藏夹信息|
|»» id|string|true|none|收藏夹ID|数字编号形式的收藏夹ID|
|»» name|string|true|none|名称|收藏夹名称|
|»» description|string|false|none|简介|收藏夹简介，此项非必须|
|»» num|integer|true|none|笔记数量|收藏夹所包含笔记数量|
|»» create_time|string|true|none|创建时间|收藏夹创建时间|
|»» state|[favorite-state](#schemafavorite-state)|false|none|当前用户状态量|此项为非必须|
|»»» owner_status|boolean|true|none|是否拥有|收藏夹是否属于自己|
|»» configuration|[favorite-configuration](#schemafavorite-configuration)|false|none|收藏夹配置|仅作者为当前用户时返回，此项非必须|
|»»» public_config|boolean|true|none|是否公开|是否公开|
|»»» default_config|boolean|true|none|是否是默认收藏夹|是否是默认收藏夹|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## PUT 修改收藏夹

PUT /favorites/{favorite_id}

> Body 请求参数

```json
{
  "name": "string",
  "description": "string",
  "configuration": {
    "publicConfig": true,
    "defaultConfig": false
  }
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|favorite_id|path|string| 是 ||none|
|body|body|object| 否 ||none|
|» name|body|string| 否 | 收藏夹名称|none|
|» description|body|string| 否 | 收藏夹简介|none|
|» configuration|body|object| 否 | 收藏夹配置|none|
|»» publicConfig|body|boolean| 否 | 是否公开|是否公开|
|»» defaultConfig|body|boolean| 否 | 是否是默认收藏夹|是否是默认收藏夹|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "id": "430000200808266423",
    "name": "展周列展团件",
    "description": "年半列维所步研料基更眼成强组装加问不。",
    "num": 0,
    "create_time": "2024-03-30 16:31:32",
    "state": {
      "owner_status": true
    },
    "configuration": {
      "public_config": true,
      "default_config": false
    }
  }
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[favorite](#schemafavorite)|false|none|收藏夹|合集所在的收藏夹信息|
|»» id|string|true|none|收藏夹ID|数字编号形式的收藏夹ID|
|»» name|string|true|none|名称|收藏夹名称|
|»» description|string|false|none|简介|收藏夹简介，此项非必须|
|»» num|integer|true|none|笔记数量|收藏夹所包含笔记数量|
|»» create_time|string|true|none|创建时间|收藏夹创建时间|
|»» state|[favorite-state](#schemafavorite-state)|false|none|当前用户状态量|此项为非必须|
|»»» owner_status|boolean|true|none|是否拥有|收藏夹是否属于自己|
|»» configuration|[favorite-configuration](#schemafavorite-configuration)|false|none|收藏夹配置|仅作者为当前用户时返回，此项非必须|
|»»» public_config|boolean|true|none|是否公开|是否公开|
|»»» default_config|boolean|true|none|是否是默认收藏夹|是否是默认收藏夹|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## DELETE 删除收藏夹

DELETE /favorites/{favorite_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|favorite_id|path|string| 是 ||收藏夹ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## POST 向收藏夹内添加笔记

POST /favorites/notes

> Body 请求参数

```json
{
  "noteIds": [
    "string"
  ],
  "favoriteIds": [
    "string"
  ]
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» noteIds|body|[string]| 是 | 笔记ID列表|要添加的笔记ID列表源|
|» favoriteIds|body|[string]| 是 | 收藏夹ID列表|添加到的目的收藏夹ID列表|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success",
  "data": 6
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|integer|false|none|数据|成功数量|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## POST 向收藏夹内添加合集

POST /favorites/collections

> Body 请求参数

```json
{
  "collectionId": [
    "string"
  ],
  "favoriteIds": [
    "string"
  ]
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» collectionId|body|[string]| 是 | 合集ID列表|要添加的合集ID列表源|
|» favoriteIds|body|[string]| 是 | 收藏夹ID列表|添加到的目的收藏夹ID列表|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success",
  "data": 6
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|integer|false|none|数据|成功数量|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## DELETE 收藏夹内删除笔记

DELETE /favorites/notes/{favorite_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|favorite_id|path|string| 是 ||收藏夹ID|
|note_id|query|array[string]| 是 ||笔记ID数组|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success",
  "data": 6
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|integer|false|none|数据|成功数量|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## DELETE 收藏夹内删除合集

DELETE /favorites/collections/{favorite_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|favorite_id|path|string| 是 ||收藏夹ID|
|collection_id|query|array[string]| 是 ||合集ID数组|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success",
  "data": 6
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|integer|false|none|数据|成功数量|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

# 合集

## POST 新建合集

POST /collections

> Body 请求参数

```yaml
name: string
cover: cmMtdXBsb2FkLTE3MTE3ODk5MjQ5ODUtNg==/Frontend_icon.png
description: string
publicOption: true

```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» name|body|string| 是 ||名称|
|» cover|body|string(binary)| 否 ||封面|
|» description|body|string| 否 ||简介|
|» publicOption|body|boolean| 否 ||配置：是否公开|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "id": "430000199712280823",
    "name": "思里较素",
    "cover": "http://dummyimage.com/180x150",
    "description": "率业识月结过断步这细度场。",
    "num": 84,
    "star_num": "8556",
    "create_time": "1997-08-09 14:59:02",
    "state": {
      "star_status": false,
      "favorite_list": [],
      "owner_status": true
    },
    "configuration": {
      "public_config": true
    }
  }
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[collection](#schemacollection)|false|none|合集|数据项|
|»» id|string|true|none|合集ID|数字编号形式的合集ID|
|»» name|string|true|none|名称|合集名称|
|»» cover|string|true|none|封面|图片URL|
|»» description|string|false|none|简介|合集简介，此项非必须|
|»» num|integer|true|none|包含笔记数量|合集内拥有笔记的数量|
|»» star_num|string|true|none|收藏量|合集被收藏的数量|
|»» create_time|string|true|none|创建时间|合集创建时间|
|»» state|[collection-state](#schemacollection-state)|false|none|当前用户状态量|此项为非必须|
|»»» star_status|boolean|true|none|是否收藏|当前用户是否收藏该合集|
|»»» favorite_list|[[favorite](#schemafavorite)]|true|none|合集所属当前用户收藏夹列表|合集所在的收藏夹列表|
|»»»» 收藏夹|[favorite](#schemafavorite)|false|none|收藏夹|合集所在的收藏夹信息|
|»»»»» id|string|true|none|收藏夹ID|数字编号形式的收藏夹ID|
|»»»»» name|string|true|none|名称|收藏夹名称|
|»»»»» description|string|false|none|简介|收藏夹简介，此项非必须|
|»»»»» num|integer|true|none|笔记数量|收藏夹所包含笔记数量|
|»»»»» create_time|string|true|none|创建时间|收藏夹创建时间|
|»»»»» state|[favorite-state](#schemafavorite-state)|false|none|当前用户状态量|此项为非必须|
|»»»»»» owner_status|boolean|true|none|是否拥有|收藏夹是否属于自己|
|»»»»» configuration|[favorite-configuration](#schemafavorite-configuration)|false|none|收藏夹配置|仅作者为当前用户时返回，此项非必须|
|»»»»»» public_config|boolean|true|none|是否公开|是否公开|
|»»»»»» default_config|boolean|true|none|是否是默认收藏夹|是否是默认收藏夹|
|»»» owner_status|boolean|true|none|是否拥有|合集是否属于自己|
|»» configuration|[collection-configuration](#schemacollection-configuration)|false|none|合集配置项|仅作者为当前用户时返回，此项非必须|
|»»» public_config|boolean|true|none|是否公开|是否公开|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## POST 修改合集

POST /collections/{collection_id}

> Body 请求参数

```yaml
name: string
cover: cmMtdXBsb2FkLTE3MTE3ODk5MjQ5ODUtNg==/Frontend_icon.png
description: string
publicOption: true

```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|collection_id|path|string| 是 ||合集ID|
|body|body|object| 否 ||none|
|» name|body|string| 否 ||名称|
|» cover|body|string(binary)| 否 ||封面|
|» description|body|string| 否 ||简介|
|» publicOption|body|boolean| 否 ||配置：是否公开|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "id": "430000199712280823",
    "name": "思里较素",
    "cover": "http://dummyimage.com/180x150",
    "description": "率业识月结过断步这细度场。",
    "num": 84,
    "star_num": "8556",
    "create_time": "1997-08-09 14:59:02",
    "state": {
      "star_status": false,
      "favorite_list": [],
      "owner_status": true
    },
    "configuration": {
      "public_config": true
    }
  }
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[collection](#schemacollection)|false|none|合集|数据项|
|»» id|string|true|none|合集ID|数字编号形式的合集ID|
|»» name|string|true|none|名称|合集名称|
|»» cover|string|true|none|封面|图片URL|
|»» description|string|false|none|简介|合集简介，此项非必须|
|»» num|integer|true|none|包含笔记数量|合集内拥有笔记的数量|
|»» star_num|string|true|none|收藏量|合集被收藏的数量|
|»» create_time|string|true|none|创建时间|合集创建时间|
|»» state|[collection-state](#schemacollection-state)|false|none|当前用户状态量|此项为非必须|
|»»» star_status|boolean|true|none|是否收藏|当前用户是否收藏该合集|
|»»» favorite_list|[[favorite](#schemafavorite)]|true|none|合集所属当前用户收藏夹列表|合集所在的收藏夹列表|
|»»»» 收藏夹|[favorite](#schemafavorite)|false|none|收藏夹|合集所在的收藏夹信息|
|»»»»» id|string|true|none|收藏夹ID|数字编号形式的收藏夹ID|
|»»»»» name|string|true|none|名称|收藏夹名称|
|»»»»» description|string|false|none|简介|收藏夹简介，此项非必须|
|»»»»» num|integer|true|none|笔记数量|收藏夹所包含笔记数量|
|»»»»» create_time|string|true|none|创建时间|收藏夹创建时间|
|»»»»» state|[favorite-state](#schemafavorite-state)|false|none|当前用户状态量|此项为非必须|
|»»»»»» owner_status|boolean|true|none|是否拥有|收藏夹是否属于自己|
|»»»»» configuration|[favorite-configuration](#schemafavorite-configuration)|false|none|收藏夹配置|仅作者为当前用户时返回，此项非必须|
|»»»»»» public_config|boolean|true|none|是否公开|是否公开|
|»»»»»» default_config|boolean|true|none|是否是默认收藏夹|是否是默认收藏夹|
|»»» owner_status|boolean|true|none|是否拥有|合集是否属于自己|
|»» configuration|[collection-configuration](#schemacollection-configuration)|false|none|合集配置项|仅作者为当前用户时返回，此项非必须|
|»»» public_config|boolean|true|none|是否公开|是否公开|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## DELETE 删除合集

DELETE /collections/{collection_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|collection_id|path|string| 是 ||合集ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## POST 往合集内添加笔记

POST /collections/notes

注：仅**当前用户的笔记**才可添加至合集内。

> Body 请求参数

```json
{
  "noteIds": [
    "string"
  ],
  "collectionIds": [
    "string"
  ]
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» noteIds|body|[string]| 是 | 笔记ID列表|要添加的笔记ID列表源|
|» collectionIds|body|[string]| 是 | 合集ID列表|添加到的目的合集ID列表|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success",
  "data": 8
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|integer|false|none|数据|成功数量|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## DELETE 合集内删除笔记

DELETE /collections/notes/{collection_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|collection_id|path|string| 是 ||合集ID|
|note_id|query|array[string]| 是 ||笔记ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success",
  "data": 8
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|integer|false|none|数据|成功数量|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

# 通知

## GET 分页获取通知列表

GET /notices

`type` 值有如下几种情形：
- `follow`：关注通知
- `star`：收藏通知
- `comment`：评论通知
- `trade`：交易通知
- `all`：所有通知

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|page_num|query|integer| 是 ||页码|
|page_size|query|integer| 是 ||每页数量|
|type|query|string| 是 ||通知类型|

#### 枚举值

|属性|值|
|---|---|
|type|follow|
|type|star|
|type|comment|
|type|trade|
|type|all|

> 返回示例

> 成功

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 1,
    "list": [
      {
        "id": "340000198801097418",
        "user": {
          "id": "210000200606198644",
          "username": "李洋",
          "avatar": "http://dummyimage.com/120x90",
          "biography": "会论着质局程使空年务白单次往广平美。",
          "gender": "男",
          "follows_num": "2969",
          "followers_num": "4339亿",
          "state": {
            "follow_status": false,
            "self_status": true
          }
        },
        "follow_time": "2024-03-30 16:10:14",
        "read_status": false
      }
    ],
    "size": 1,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 1,
    "list": [
      {
        "id": "340000198801097419",
        "user": {
          "id": "510000199606267991",
          "username": "汤娜",
          "avatar": "http://dummyimage.com/250x250",
          "biography": "象称最政易式划小产治你认少造权。",
          "gender": "女",
          "follows_num": "9373",
          "followers_num": "5734万",
          "state": {
            "follow_status": false,
            "self_status": true
          }
        },
        "note": {
          "id": "370000200709074286",
          "author": {
            "id": "710000202112144627",
            "username": "崔芳",
            "avatar": "http://dummyimage.com/120x90",
            "biography": "公快矿年影加八完切满生完交。",
            "gender": "女",
            "follows_num": "7206",
            "followers_num": "9128万",
            "state": {
              "follow_status": false,
              "self_status": false
            }
          },
          "title": "积支情广此",
          "cover": "http://dummyimage.com/240x400",
          "view_num": "2443",
          "star_num": "5578w",
          "comment_num": "1808w",
          "create_time": "2020-06-13 05:04:40",
          "update_time": "2024-03-30 16:13:54"
        },
        "star_time": "2024-03-30 16:13:54",
        "read_status": false
      }
    ],
    "size": 1,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 1,
    "list": [
      {
        "id": "340000198801097420",
        "note": {
          "id": "460000201709039524",
          "author": {
            "id": "810000201107133420",
            "username": "高强",
            "avatar": "http://dummyimage.com/468x60",
            "biography": "例在至算做山改科得到经了利真。",
            "gender": "男",
            "follows_num": "2899",
            "followers_num": "5282万",
            "state": {
              "follow_status": true,
              "self_status": false
            }
          },
          "title": "须化油放",
          "cover": "http://dummyimage.com/180x150",
          "view_num": "1904w",
          "star_num": "8554",
          "comment_num": "1303w",
          "create_time": "1988-10-01 06:24:33",
          "update_time": "2024-03-30 16:12:44"
        },
        "comment": {
          "id": "430000198611301758",
          "user": {
            "id": "540000199311101658",
            "username": "胡秀英",
            "avatar": "http://dummyimage.com/240x400",
            "biography": "总性精对法和铁部族加性织率。",
            "gender": "未知",
            "follows_num": "5654",
            "followers_num": "2561万",
            "state": {
              "follow_status": true,
              "self_status": true
            }
          },
          "content": "育把出老民为产重石始派写指光根写而。",
          "create_time": "2024-03-30 16:12:44"
        },
        "read_status": false
      }
    ],
    "size": 1,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

```json
{
  "code": 200,
  "data": {
    "pages": 1,
    "page_num": 1,
    "page_size": 10,
    "total": 1,
    "list": [
      {
        "id": "340000198801097421",
        "payment": {
          "id": "320000197506032963",
          "type": 1,
          "amount": "474.06",
          "note": {
            "id": "150000201406062812",
            "author": {
              "id": "640000200901319170",
              "username": "袁艳",
              "avatar": "http://dummyimage.com/720x300",
              "biography": "热由调族开深形用受向权门体状更最。",
              "gender": "未知",
              "follows_num": "6917",
              "followers_num": "6125万",
              "state": {
                "follow_status": false,
                "self_status": true
              }
            },
            "title": "应这问记成斗或",
            "cover": "http://dummyimage.com/120x600",
            "view_num": "1134",
            "star_num": "9753w",
            "comment_num": "540w",
            "create_time": "1974-03-15 11:27:42",
            "update_time": "2024-03-30 16:11:22"
          },
          "counterpart": {
            "id": "530000202007288289",
            "username": "韩娟",
            "avatar": "http://dummyimage.com/120x600",
            "biography": "许经但步程清走线员进工实行间在。",
            "gender": "男",
            "follows_num": "4401",
            "followers_num": "1314",
            "state": {
              "follow_status": false,
              "self_status": true
            }
          },
          "create_time": "2024-03-30 16:11:22"
        },
        "read_status": false
      }
    ],
    "size": 1,
    "has_previous_page": false,
    "has_next_page": false,
    "is_first_page": true,
    "is_last_page": true,
    "first_page": 1,
    "last_page": 1
  }
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|
|»» pages|integer|true|none|总页数|none|
|»» page_num|integer|true|none|当前页码|none|
|»» page_size|integer|true|none|每页的数量|none|
|»» total|integer|true|none|总数|none|
|»» list|[anyOf]|true|none|当前页的数据列表|none|

*anyOf*

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|»»» *anonymous*|[follow-notice](#schemafollow-notice)|false|none|关注通知|none|
|»»»» id|string|true|none|通知ID|数字编号形式的通知ID|
|»»»» user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»» username|string|true|none|用户名|none|
|»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»» state|object|false|none||none|
|»»»»»» follow_status|boolean|false|none|是否关注|是否关注此用户|
|»»»»»» self_status|boolean|false|none|是否是当前用户|是否是当前用户|
|»»»» follow_time|string|true|none|关注时间|none|
|»»»» read_status|boolean|true|none|是否已读|none|

*or*

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|»»» *anonymous*|[star_notice](#schemastar_notice)|false|none|收藏通知|none|
|»»»» id|string|true|none|通知ID|数字编号形式的用户ID|
|»»»» user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»» username|string|true|none|用户名|none|
|»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»» state|object|false|none||none|
|»»»» note|[note-overview](#schemanote-overview)|true|none|笔记|账单所关联的笔记|
|»»»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»»» username|string|true|none|用户名|none|
|»»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»»» state|object|false|none||none|
|»»»»» title|string|true|none|标题|笔记标题|
|»»»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»»»» view_num|string|true|none|浏览量|none|
|»»»»» star_num|string|true|none|收藏量|none|
|»»»»» comment_num|string|true|none|评论量|none|
|»»»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»»»» star_time|string|true|none|收藏时间|none|
|»»»» read_status|boolean|true|none|是否已读|none|

*or*

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|»»» *anonymous*|[comment-notice](#schemacomment-notice)|false|none|评论通知|none|
|»»»» id|string|true|none|通知ID|数字编号形式的用户ID|
|»»»» note|object|true|none|笔记信息|评论所在的笔记|
|»»»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»»» username|string|true|none|用户名|none|
|»»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»»» state|object|false|none||none|
|»»»»» title|string|true|none|标题|笔记标题|
|»»»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»»»» view_num|string|true|none|浏览量|none|
|»»»»» star_num|string|true|none|收藏量|none|
|»»»»» comment_num|string|true|none|评论量|none|
|»»»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»»»» comment|object|true|none|评论信息|none|
|»»»»» id|string|true|none|评论ID|数字编号形式的评论ID|
|»»»»» user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»»» username|string|true|none|用户名|none|
|»»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»»» state|object|false|none||none|
|»»»»» content|string|true|none|评论文本|none|
|»»»»» create_time|string|true|none|评论时间|评论发布时间|
|»»»» read_status|boolean|true|none|是否已读|none|

*or*

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|»»» *anonymous*|[trade_notice](#schematrade_notice)|false|none|交易通知|none|
|»»»» id|string|true|none|通知ID|数字编号形式的用户ID|
|»»»» payment|[bill](#schemabill)|true|none|交易详情|交易账单|
|»»»»» id|string|true|none|账单ID|数字编号形式的账单ID|
|»»»»» type|integer|true|none|类型|账单类型|
|»»»»» amount|string|true|none|账单金额|三位逗分|
|»»»»» note|[note-overview](#schemanote-overview)|true|none|笔记|账单所关联的笔记|
|»»»»»» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|»»»»»» author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»»» title|string|true|none|标题|笔记标题|
|»»»»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»»»»» view_num|string|true|none|浏览量|none|
|»»»»»» star_num|string|true|none|收藏量|none|
|»»»»»» comment_num|string|true|none|评论量|none|
|»»»»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»»»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»»»»» counterpart|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|»»»»»» id|string|true|none|用户ID|数字编号形式的用户ID|
|»»»»»» username|string|true|none|用户名|none|
|»»»»»» avatar|string|true|none|用户头像|头像图片URL|
|»»»»»» biography|string|false|none|个人简介|此项非必须|
|»»»»»» gender|string|true|none|性别|取值范围：男、女和未知|
|»»»»»» follows_num|string|true|none|关注数|用户关注的数量|
|»»»»»» followers_num|string|true|none|粉丝数|用户粉丝的数量|
|»»»»»» state|object|false|none||none|
|»»»»» create_time|string|true|none|创建时间|订单创建时间|
|»»»» read_status|boolean|true|none|是否已读|none|

*continued*

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

#### 枚举值

|属性|值|
|---|---|
|type|0|
|type|1|
|type|2|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## PUT 将通知设为已读状态

PUT /notices/{notice_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|notice_id|path|string| 是 ||通知ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## DELETE 删除通知

DELETE /notices/{notice_id}

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|notice_id|path|string| 是 ||通知ID|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

# 编辑

## POST 获取文件链接

POST /edit/link

注意：用户在编辑以及评论插入**图片、音频、视频**时，通过此接口上传文件获取链接。

> Body 请求参数

```yaml
file: cmMtdXBsb2FkLTE3MTE4MDAxMDMxODMtMg==/README.pdf

```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» file|body|string(binary)| 是 ||none|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success",
  "data": "https://cdn.demiphea.com/demiphea.mp4"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|string|false|none|数据|文件链接|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## POST OCR识别导入

POST /edit/ocr

> Body 请求参数

```yaml
file: cmMtdXBsb2FkLTE3MTE4MDAxMDMxODMtMg==/README.pdf

```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» file|body|string(binary)| 是 ||none|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success",
  "data": "这是一个？"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|string|false|none|数据|返回数据|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## POST 文件导入

POST /edit/import

> Body 请求参数

```yaml
file: cmMtdXBsb2FkLTE3MTE4MDAxMDMxODMtMg==/README.pdf

```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|body|body|object| 否 ||none|
|» file|body|string(binary)| 否 ||none|

> 返回示例

> 成功

```json
{
  "code": 200,
  "msg": "Success",
  "data": "这是一个？"
}
```

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|string|false|none|数据|返回数据|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 笔记导出

GET /edit/export

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|type|query|string| 是 ||导出类型|
|note_id|query|string| 是 ||笔记ID|

#### 枚举值

|属性|值|
|---|---|
|type|markdown|
|type|word|
|type|pdf|

> 返回示例

> 200 Response

> 400 Response

```json
{
  "msg": "string"
}
```

> 登录态失效

```json
{
  "msg": "用户未登录或登录过期"
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|请求出错|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|object|false|none|数据|none|

状态码 **400**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|错误信息|none|

状态码 **401**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

# 数据模型

<h2 id="tocS_用户状态量">用户状态量</h2>

<a id="schema用户状态量"></a>
<a id="schema_用户状态量"></a>
<a id="tocS用户状态量"></a>
<a id="tocs用户状态量"></a>

```json
{
  "follow_status": true,
  "self_status": true
}

```

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|follow_status|boolean|false|none|是否关注|是否关注此用户|
|self_status|boolean|false|none|是否是当前用户|是否是当前用户|

<h2 id="tocS_comment-state">comment-state</h2>

<a id="schemacomment-state"></a>
<a id="schema_comment-state"></a>
<a id="tocScomment-state"></a>
<a id="tocscomment-state"></a>

```json
{
  "agree_status": true
}

```

评论用户状态量

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|agree_status|boolean|true|none|是否点赞|当前用户是否点赞该评论|

<h2 id="tocS_pagination">pagination</h2>

<a id="schemapagination"></a>
<a id="schema_pagination"></a>
<a id="tocSpagination"></a>
<a id="tocspagination"></a>

```json
{
  "pages": 0,
  "page_num": 0,
  "page_size": 0,
  "total": 0,
  "list": [
    {}
  ],
  "size": 0,
  "has_previous_page": true,
  "has_next_page": true,
  "is_first_page": true,
  "is_last_page": true,
  "first_page": 0,
  "last_page": 0
}

```

分页

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|pages|integer|true|none|总页数|none|
|page_num|integer|true|none|当前页码|none|
|page_size|integer|true|none|每页的数量|none|
|total|integer|true|none|总数|none|
|list|[object]|true|none|当前页的数据列表|none|
|size|integer|true|none|当前页的数据列表长度|none|
|has_previous_page|boolean|true|none|是否有前一页|none|
|has_next_page|boolean|true|none|是否有下一页|none|
|is_first_page|boolean|true|none|是否为首页|none|
|is_last_page|boolean|true|none|是否为尾页|none|
|first_page|integer|true|none|首页页码|none|
|last_page|integer|true|none|尾页页码|none|

<h2 id="tocS_bill">bill</h2>

<a id="schemabill"></a>
<a id="schema_bill"></a>
<a id="tocSbill"></a>
<a id="tocsbill"></a>

```json
{
  "id": "string",
  "type": 0,
  "amount": "string",
  "note": {
    "id": "string",
    "author": {
      "id": "string",
      "username": "string",
      "avatar": "string",
      "biography": "string",
      "gender": "string",
      "follows_num": "string",
      "followers_num": "string",
      "state": {
        "follow_status": true,
        "self_status": true
      }
    },
    "title": "string",
    "cover": "string",
    "view_num": "string",
    "star_num": "string",
    "comment_num": "string",
    "create_time": "string",
    "update_time": "string"
  },
  "counterpart": {
    "id": "string",
    "username": "string",
    "avatar": "string",
    "biography": "string",
    "gender": "string",
    "follows_num": "string",
    "followers_num": "string",
    "state": {
      "follow_status": true,
      "self_status": true
    }
  },
  "create_time": "string"
}

```

账单

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none|账单ID|数字编号形式的账单ID|
|type|integer|true|none|类型|账单类型|
|amount|string|true|none|账单金额|三位逗分|
|note|[note-overview](#schemanote-overview)|true|none|笔记|账单所关联的笔记|
|counterpart|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|create_time|string|true|none|创建时间|订单创建时间|

#### 枚举值

|属性|值|
|---|---|
|type|0|
|type|1|
|type|2|

<h2 id="tocS_wallet">wallet</h2>

<a id="schemawallet"></a>
<a id="schema_wallet"></a>
<a id="tocSwallet"></a>
<a id="tocswallet"></a>

```json
{
  "balance": "string",
  "revenue": "string"
}

```

钱包

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|balance|string|true|none|余额|账户余额，三位逗分|
|revenue|string|true|none|累计收入|累计余额，三位逗分|

<h2 id="tocS_trade_notice">trade_notice</h2>

<a id="schematrade_notice"></a>
<a id="schema_trade_notice"></a>
<a id="tocStrade_notice"></a>
<a id="tocstrade_notice"></a>

```json
{
  "id": "string",
  "payment": {
    "id": "string",
    "type": 0,
    "amount": "string",
    "note": {
      "id": "string",
      "author": {
        "id": "string",
        "username": "string",
        "avatar": "string",
        "biography": "string",
        "gender": "string",
        "follows_num": "string",
        "followers_num": "string",
        "state": {
          "follow_status": null,
          "self_status": null
        }
      },
      "title": "string",
      "cover": "string",
      "view_num": "string",
      "star_num": "string",
      "comment_num": "string",
      "create_time": "string",
      "update_time": "string"
    },
    "counterpart": {
      "id": "string",
      "username": "string",
      "avatar": "string",
      "biography": "string",
      "gender": "string",
      "follows_num": "string",
      "followers_num": "string",
      "state": {
        "follow_status": true,
        "self_status": true
      }
    },
    "create_time": "string"
  },
  "read_status": true
}

```

交易通知

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none|通知ID|数字编号形式的用户ID|
|payment|[bill](#schemabill)|true|none|交易详情|交易账单|
|read_status|boolean|true|none|是否已读|none|

<h2 id="tocS_comment-overview">comment-overview</h2>

<a id="schemacomment-overview"></a>
<a id="schema_comment-overview"></a>
<a id="tocScomment-overview"></a>
<a id="tocscomment-overview"></a>

```json
{
  "id": "string",
  "user": {
    "id": "string",
    "username": "string",
    "avatar": "string",
    "biography": "string",
    "gender": "string",
    "follows_num": "string",
    "followers_num": "string",
    "state": {
      "follow_status": true,
      "self_status": true
    }
  },
  "content": "string",
  "create_time": "string"
}

```

评论概览

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none|评论ID|数字编号形式的评论ID|
|user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|content|string|true|none|评论文本|none|
|create_time|string|true|none|评论时间|评论发布时间|

<h2 id="tocS_comment-notice">comment-notice</h2>

<a id="schemacomment-notice"></a>
<a id="schema_comment-notice"></a>
<a id="tocScomment-notice"></a>
<a id="tocscomment-notice"></a>

```json
{
  "id": "string",
  "note": {
    "id": "string",
    "author": {
      "id": "string",
      "username": "string",
      "avatar": "string",
      "biography": "string",
      "gender": "string",
      "follows_num": "string",
      "followers_num": "string",
      "state": {
        "follow_status": true,
        "self_status": true
      }
    },
    "title": "string",
    "cover": "string",
    "view_num": "string",
    "star_num": "string",
    "comment_num": "string",
    "create_time": "string",
    "update_time": "string"
  },
  "comment": {
    "id": "string",
    "user": {
      "id": "string",
      "username": "string",
      "avatar": "string",
      "biography": "string",
      "gender": "string",
      "follows_num": "string",
      "followers_num": "string",
      "state": {
        "follow_status": true,
        "self_status": true
      }
    },
    "content": "string",
    "create_time": "string"
  },
  "read_status": true
}

```

评论通知

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none|通知ID|数字编号形式的用户ID|
|note|object|true|none|笔记信息|评论所在的笔记|
|» id|string|true|none|笔记ID|数字编号形式的笔记ID|
|» author|[user](#schemauser)|true|none||账单创建时另外一方用户信息|
|» title|string|true|none|标题|笔记标题|
|» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|» view_num|string|true|none|浏览量|none|
|» star_num|string|true|none|收藏量|none|
|» comment_num|string|true|none|评论量|none|
|» create_time|string|true|none|创建时间|笔记创建的时间|
|» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|comment|object|true|none|评论信息|none|
|» id|string|true|none|评论ID|数字编号形式的评论ID|
|» user|[user](#schemauser)|true|none||账单创建时另外一方用户信息|
|» content|string|true|none|评论文本|none|
|» create_time|string|true|none|评论时间|评论发布时间|
|read_status|boolean|true|none|是否已读|none|

<h2 id="tocS_star_notice">star_notice</h2>

<a id="schemastar_notice"></a>
<a id="schema_star_notice"></a>
<a id="tocSstar_notice"></a>
<a id="tocsstar_notice"></a>

```json
{
  "id": "string",
  "user": {
    "id": "string",
    "username": "string",
    "avatar": "string",
    "biography": "string",
    "gender": "string",
    "follows_num": "string",
    "followers_num": "string",
    "state": {
      "follow_status": true,
      "self_status": true
    }
  },
  "note": {
    "id": "string",
    "author": {
      "id": "string",
      "username": "string",
      "avatar": "string",
      "biography": "string",
      "gender": "string",
      "follows_num": "string",
      "followers_num": "string",
      "state": {
        "follow_status": true,
        "self_status": true
      }
    },
    "title": "string",
    "cover": "string",
    "view_num": "string",
    "star_num": "string",
    "comment_num": "string",
    "create_time": "string",
    "update_time": "string"
  },
  "star_time": "string",
  "read_status": true
}

```

收藏通知

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none|通知ID|数字编号形式的用户ID|
|user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|note|[note-overview](#schemanote-overview)|true|none|笔记|账单所关联的笔记|
|star_time|string|true|none|收藏时间|none|
|read_status|boolean|true|none|是否已读|none|

<h2 id="tocS_follow-notice">follow-notice</h2>

<a id="schemafollow-notice"></a>
<a id="schema_follow-notice"></a>
<a id="tocSfollow-notice"></a>
<a id="tocsfollow-notice"></a>

```json
{
  "id": "string",
  "user": {
    "id": "string",
    "username": "string",
    "avatar": "string",
    "biography": "string",
    "gender": "string",
    "follows_num": "string",
    "followers_num": "string",
    "state": {
      "follow_status": true,
      "self_status": true
    }
  },
  "follow_time": "string",
  "read_status": true
}

```

关注通知

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none|通知ID|数字编号形式的通知ID|
|user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|follow_time|string|true|none|关注时间|none|
|read_status|boolean|true|none|是否已读|none|

<h2 id="tocS_comment">comment</h2>

<a id="schemacomment"></a>
<a id="schema_comment"></a>
<a id="tocScomment"></a>
<a id="tocscomment"></a>

```json
{
  "id": "string",
  "user": {
    "id": "string",
    "username": "string",
    "avatar": "string",
    "biography": "string",
    "gender": "string",
    "follows_num": "string",
    "followers_num": "string",
    "state": {
      "follow_status": true,
      "self_status": true
    }
  },
  "text": "string",
  "image_list": [
    "string"
  ],
  "audio": "string",
  "video": "string",
  "note": {
    "id": "string",
    "author": {
      "id": "string",
      "username": "string",
      "avatar": "string",
      "biography": "string",
      "gender": "string",
      "follows_num": "string",
      "followers_num": "string",
      "state": {
        "follow_status": true,
        "self_status": true
      }
    },
    "title": "string",
    "cover": "string",
    "view_num": "string",
    "star_num": "string",
    "comment_num": "string",
    "create_time": "string",
    "update_time": "string"
  },
  "agree_num": "string",
  "reply_num": "string",
  "create_time": "string",
  "state": {
    "agree_status": true
  }
}

```

评论

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none|评论ID|数字编号形式的评论ID|
|user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|text|string|false|none|评论文本|非必须项|
|image_list|[string]|false|none|评论图片|评论图片列表，非必须项|
|audio|string|false|none|评论语音|语音URL|
|video|string|false|none|评论视频|视频URL|
|note|[note-overview](#schemanote-overview)|false|none|笔记|账单所关联的笔记|
|agree_num|string|true|none|赞同数|评论的点赞数量|
|reply_num|string|true|none|回复数|评论回复的数量|
|create_time|string|true|none|评论时间|评论发布时间|
|state|[comment-state](#schemacomment-state)|true|none|当前用户状态量|none|

<h2 id="tocS_favorite-state">favorite-state</h2>

<a id="schemafavorite-state"></a>
<a id="schema_favorite-state"></a>
<a id="tocSfavorite-state"></a>
<a id="tocsfavorite-state"></a>

```json
{
  "owner_status": true
}

```

收藏夹用户状态量

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|owner_status|boolean|true|none|是否拥有|收藏夹是否属于自己|

<h2 id="tocS_collection-state">collection-state</h2>

<a id="schemacollection-state"></a>
<a id="schema_collection-state"></a>
<a id="tocScollection-state"></a>
<a id="tocscollection-state"></a>

```json
{
  "star_status": true,
  "favorite_list": [
    {
      "id": "string",
      "name": "string",
      "description": "string",
      "num": 0,
      "create_time": "string",
      "state": {
        "owner_status": true
      },
      "configuration": {
        "public_config": true,
        "default_config": true
      }
    }
  ],
  "owner_status": true
}

```

合集用户状态量

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|star_status|boolean|true|none|是否收藏|当前用户是否收藏该合集|
|favorite_list|[[favorite](#schemafavorite)]|true|none|合集所属当前用户收藏夹列表|合集所在的收藏夹列表|
|owner_status|boolean|true|none|是否拥有|合集是否属于自己|

<h2 id="tocS_favorite-configuration">favorite-configuration</h2>

<a id="schemafavorite-configuration"></a>
<a id="schema_favorite-configuration"></a>
<a id="tocSfavorite-configuration"></a>
<a id="tocsfavorite-configuration"></a>

```json
{
  "public_config": true,
  "default_config": true
}

```

收藏夹配置项

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|public_config|boolean|true|none|是否公开|是否公开|
|default_config|boolean|true|none|是否是默认收藏夹|是否是默认收藏夹|

<h2 id="tocS_collection-configuration">collection-configuration</h2>

<a id="schemacollection-configuration"></a>
<a id="schema_collection-configuration"></a>
<a id="tocScollection-configuration"></a>
<a id="tocscollection-configuration"></a>

```json
{
  "public_config": true
}

```

合集配置项

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|public_config|boolean|true|none|是否公开|是否公开|

<h2 id="tocS_note-configuration">note-configuration</h2>

<a id="schemanote-configuration"></a>
<a id="schema_note-configuration"></a>
<a id="tocSnote-configuration"></a>
<a id="tocsnote-configuration"></a>

```json
{
  "public_config": true,
  "free_config": 0
}

```

笔记配置项

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|public_config|boolean|true|none|是否公开|是否公开|
|free_config|integer|true|none|是否免费|若值为0，则免费，其余则收费，单位：分|

<h2 id="tocS_note-state">note-state</h2>

<a id="schemanote-state"></a>
<a id="schema_note-state"></a>
<a id="tocSnote-state"></a>
<a id="tocsnote-state"></a>

```json
{
  "star_status": true,
  "favorite_list": [
    {
      "id": "string",
      "name": "string",
      "description": "string",
      "num": 0,
      "create_time": "string",
      "state": {
        "owner_status": true
      },
      "configuration": {
        "public_config": true,
        "default_config": true
      }
    }
  ],
  "owner_status": true
}

```

笔记用户状态量

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|star_status|boolean|true|none|是否收藏|当前用户是否收藏该笔记|
|favorite_list|[[favorite](#schemafavorite)]|true|none|笔记所属当前用户收藏夹列表|笔记所在收藏夹列表|
|owner_status|boolean|true|none|是否拥有|是否为笔记作者|

<h2 id="tocS_favorite">favorite</h2>

<a id="schemafavorite"></a>
<a id="schema_favorite"></a>
<a id="tocSfavorite"></a>
<a id="tocsfavorite"></a>

```json
{
  "id": "string",
  "name": "string",
  "description": "string",
  "num": 0,
  "create_time": "string",
  "state": {
    "owner_status": true
  },
  "configuration": {
    "public_config": true,
    "default_config": true
  }
}

```

收藏夹

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none|收藏夹ID|数字编号形式的收藏夹ID|
|name|string|true|none|名称|收藏夹名称|
|description|string|false|none|简介|收藏夹简介，此项非必须|
|num|integer|true|none|笔记数量|收藏夹所包含笔记数量|
|create_time|string|true|none|创建时间|收藏夹创建时间|
|state|[favorite-state](#schemafavorite-state)|false|none|当前用户状态量|此项为非必须|
|configuration|[favorite-configuration](#schemafavorite-configuration)|false|none|收藏夹配置|仅作者为当前用户时返回，此项非必须|

<h2 id="tocS_collection">collection</h2>

<a id="schemacollection"></a>
<a id="schema_collection"></a>
<a id="tocScollection"></a>
<a id="tocscollection"></a>

```json
{
  "id": "string",
  "name": "string",
  "cover": "string",
  "description": "string",
  "num": 0,
  "star_num": "string",
  "create_time": "string",
  "state": {
    "star_status": true,
    "favorite_list": [
      {
        "id": "string",
        "name": "string",
        "description": "string",
        "num": 0,
        "create_time": "string",
        "state": {
          "owner_status": true
        },
        "configuration": {
          "public_config": true,
          "default_config": true
        }
      }
    ],
    "owner_status": true
  },
  "configuration": {
    "public_config": true
  }
}

```

合集

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none|合集ID|数字编号形式的合集ID|
|name|string|true|none|名称|合集名称|
|cover|string|true|none|封面|图片URL|
|description|string|false|none|简介|合集简介，此项非必须|
|num|integer|true|none|包含笔记数量|合集内拥有笔记的数量|
|star_num|string|true|none|收藏量|合集被收藏的数量|
|create_time|string|true|none|创建时间|合集创建时间|
|state|[collection-state](#schemacollection-state)|false|none|当前用户状态量|此项为非必须|
|configuration|[collection-configuration](#schemacollection-configuration)|false|none|合集配置项|仅作者为当前用户时返回，此项非必须|

<h2 id="tocS_note">note</h2>

<a id="schemanote"></a>
<a id="schema_note"></a>
<a id="tocSnote"></a>
<a id="tocsnote"></a>

```json
{
  "overview": {
    "id": "string",
    "author": {
      "id": "string",
      "username": "string",
      "avatar": "string",
      "biography": "string",
      "gender": "string",
      "follows_num": "string",
      "followers_num": "string",
      "state": {
        "follow_status": true,
        "self_status": true
      }
    },
    "title": "string",
    "cover": "string",
    "view_num": "string",
    "star_num": "string",
    "comment_num": "string",
    "create_time": "string",
    "update_time": "string"
  },
  "content": "string",
  "state": {
    "star_status": true,
    "owner_status": true
  },
  "configuration": {
    "public_config": true,
    "free_config": 0
  }
}

```

笔记

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|overview|[note-overview](#schemanote-overview)|true|none|笔记|账单所关联的笔记|
|content|string|true|none|内容|Markdown内容|
|state|[note-state](#schemanote-state)|false|none|当前用户状态量|此项非必须|
|configuration|[note-configuration](#schemanote-configuration)|false|none|笔记配置项|仅作者为当前用户时返回，此项非必须|

<h2 id="tocS_user">user</h2>

<a id="schemauser"></a>
<a id="schema_user"></a>
<a id="tocSuser"></a>
<a id="tocsuser"></a>

```json
{
  "id": "string",
  "username": "string",
  "avatar": "string",
  "biography": "string",
  "gender": "string",
  "follows_num": "string",
  "followers_num": "string",
  "state": {
    "follow_status": true,
    "self_status": true
  }
}

```

用户

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none|用户ID|数字编号形式的用户ID|
|username|string|true|none|用户名|none|
|avatar|string|true|none|用户头像|头像图片URL|
|biography|string|false|none|个人简介|此项非必须|
|gender|string|true|none|性别|取值范围：男、女和未知|
|follows_num|string|true|none|关注数|用户关注的数量|
|followers_num|string|true|none|粉丝数|用户粉丝的数量|
|state|object|false|none||none|
|» follow_status|boolean|false|none|是否关注|是否关注此用户|
|» self_status|boolean|false|none|是否是当前用户|是否是当前用户|

<h2 id="tocS_note-overview">note-overview</h2>

<a id="schemanote-overview"></a>
<a id="schema_note-overview"></a>
<a id="tocSnote-overview"></a>
<a id="tocsnote-overview"></a>

```json
{
  "id": "string",
  "author": {
    "id": "string",
    "username": "string",
    "avatar": "string",
    "biography": "string",
    "gender": "string",
    "follows_num": "string",
    "followers_num": "string",
    "state": {
      "follow_status": true,
      "self_status": true
    }
  },
  "title": "string",
  "cover": "string",
  "view_num": "string",
  "star_num": "string",
  "comment_num": "string",
  "create_time": "string",
  "update_time": "string"
}

```

笔记概览

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none|笔记ID|数字编号形式的笔记ID|
|author|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|title|string|true|none|标题|笔记标题|
|cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|view_num|string|true|none|浏览量|none|
|star_num|string|true|none|收藏量|none|
|comment_num|string|true|none|评论量|none|
|create_time|string|true|none|创建时间|笔记创建的时间|
|update_time|string|true|none|修改时间|笔记最后一次修改的时间|

