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

# Authentication

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

### Params

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|code|query|string| yes |通过 `wx.login` 获取|

> Response Examples

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

### Responses

|HTTP Status Code |Meaning|Description|Data schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|

### Responses Data Schema

HTTP Status Code **200**

|Name|Type|Required|Restrictions|Title|description|
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
|»» licence|object|true|none|许可证信息|none|
|»»» access_token|string|true|none|token|接口调用凭证|
|»»» expires_in|integer|true|none|有效期|接口调用凭证超时时间，单位（秒）|

## PUT 用户修改个人资料

PUT /users

> Body Parameters

```yaml
username: "{% mock 'cname' %}"
avatar: cmMtdXBsb2FkLTE3MTE2MDQ3NTYzNTYtMg==/avatar.jpg
biography: "{% mock 'cparagraph', 1, 250 %}"
gender: "{% mock 'natural', '0', 2 %}"

```

### Params

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|body|body|object| no |none|
|» username|body|string| no |姓名|
|» avatar|body|string(binary)| no |头像|
|» biography|body|string| no |个人简介|
|» gender|body|integer| no |性别|

#### Enum

|Name|Value|
|---|---|
|» gender|0|
|» gender|1|
|» gender|2|

> Response Examples

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

### Responses

|HTTP Status Code |Meaning|Description|Data schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### Responses Data Schema

HTTP Status Code **200**

|Name|Type|Required|Restrictions|Title|description|
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

HTTP Status Code **401**

|Name|Type|Required|Restrictions|Title|description|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 分页获取用户关注列表

GET /follows

### Params

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|page_num|query|integer| yes |页码，从1开始|
|page_size|query|integer| yes |每页数量|

> Response Examples

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
        "followers_num": "6836万"
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

### Responses

|HTTP Status Code |Meaning|Description|Data schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### Responses Data Schema

HTTP Status Code **200**

|Name|Type|Required|Restrictions|Title|description|
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
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

HTTP Status Code **401**

|Name|Type|Required|Restrictions|Title|description|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 分页获取用户粉丝列表

GET /followers

### Params

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|page_num|query|integer| yes |页码，从1开始|
|page_size|query|integer| yes |每页数量|

> Response Examples

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
        "followers_num": "2287亿"
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

### Responses

|HTTP Status Code |Meaning|Description|Data schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### Responses Data Schema

HTTP Status Code **200**

|Name|Type|Required|Restrictions|Title|description|
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
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

HTTP Status Code **401**

|Name|Type|Required|Restrictions|Title|description|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 分页获取用户账单列表

GET /bills

### Params

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|page_num|query|integer| yes |页码，从1开始|
|page_size|query|integer| yes |每页数量|

> Response Examples

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
            "followers_num": "8977万"
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
          "followers_num": "8744亿"
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
            "followers_num": "9330亿"
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
          "followers_num": "3780亿"
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

### Responses

|HTTP Status Code |Meaning|Description|Data schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### Responses Data Schema

HTTP Status Code **200**

|Name|Type|Required|Restrictions|Title|description|
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
|»»»» create_time|string|true|none|创建时间|订单创建时间|
|»» size|integer|true|none|当前页的数据列表长度|none|
|»» has_previous_page|boolean|true|none|是否有前一页|none|
|»» has_next_page|boolean|true|none|是否有下一页|none|
|»» is_first_page|boolean|true|none|是否为首页|none|
|»» is_last_page|boolean|true|none|是否为尾页|none|
|»» first_page|integer|true|none|首页页码|none|
|»» last_page|integer|true|none|尾页页码|none|

#### Enum

|Name|Value|
|---|---|
|type|0|
|type|1|
|type|2|

HTTP Status Code **401**

|Name|Type|Required|Restrictions|Title|description|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 获取用户钱包信息

GET /wallets

> Response Examples

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

### Responses

|HTTP Status Code |Meaning|Description|Data schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### Responses Data Schema

HTTP Status Code **200**

|Name|Type|Required|Restrictions|Title|description|
|---|---|---|---|---|---|
|» code|integer|true|none|业务码|none|
|» msg|string|false|none|业务处理信息|none|
|» data|[wallet](#schemawallet)|false|none|数据|none|
|»» balance|string|true|none|余额|账户余额，三位逗分|
|»» revenue|string|true|none|累计收入|累计余额，三位逗分|

HTTP Status Code **401**

|Name|Type|Required|Restrictions|Title|description|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

# 笔记

## GET 分页获取笔记列表

GET /notes

通过对请求参数 `query_type` 的设置，共有五种查询方式：
- `purchase`：查询当前用户购买的笔记
- `recommend`：获取推荐的笔记列表
- `favorite`：查询某一收藏夹内的笔记
需要设置请求参数 `favorite_id` （收藏夹ID）
- `collection`：查询某一合集内的笔记
需要设置请求参数 `collection_id` （合集ID）
- `search`：搜索笔记
需要设置请求参数 `search_key` （搜索关键字）和 `search_scope` （搜索范围）。其中 `search_scope` 是枚举量（默认值为 `0` ）：当值为 `0` 时，搜索笔记和合集；当值为 `1` 时，仅搜索笔记；当值为 `2` 时，仅搜索合集。

注：当前用户仅可查询公开的或者本人的收藏夹和合集，并且可查询搜索到的笔记的权限为公开或者本人是作者。

### Params

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|query_type|query|string| yes |查询类别|
|favorite_id|query|string| no |收藏夹ID|
|collection_id|query|string| no |合集ID|
|search_key|query|string| no |搜索关键字|
|search_scope|query|integer| no |搜索范围|
|page_num|query|integer| yes |页码，从1开始|
|page_size|query|integer| yes |每页数量|

#### Enum

|Name|Value|
|---|---|
|query_type|purchase|
|query_type|favorite|
|query_type|collection|
|query_type|recommend|
|query_type|search|
|search_scope|0|
|search_scope|1|
|search_scope|2|

> Response Examples

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
          "followers_num": "9707万"
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
          "followers_num": "7870万"
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
          "followers_num": "9835"
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

### Responses

|HTTP Status Code |Meaning|Description|Data schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|登录态失效|Inline|

### Responses Data Schema

HTTP Status Code **200**

|Name|Type|Required|Restrictions|Title|description|
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

HTTP Status Code **401**

|Name|Type|Required|Restrictions|Title|description|
|---|---|---|---|---|---|
|» msg|string|true|none|响应信息|none|

## GET 根据ID查询某一笔记

GET /notes/{note_id}

查看笔记的具体内容。
- 当用户未登录时， `state` 字段不返回
- 当笔记不属于当前用户时， `configuration` 字段不返回

### Params

|Name|Location|Type|Required|Description|
|---|---|---|---|---|
|note_id|path|string| yes |笔记ID|

> Response Examples

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
        "followers_num": "1483亿"
      },
      "title": "业近都东",
      "cover": "http://dummyimage.com/240x400",
      "view_num": "734w",
      "star_num": "8691",
      "comment_num": "848w",
      "create_time": "2017-06-06 04:07:30",
      "update_time": "2024-03-28 15:51:23"
    },
    "collection_list": [
      {
        "id": "13000020110907053X",
        "name": "按色委下省报",
        "cover": "http://dummyimage.com/250x250",
        "description": "传具其理再置此才它物高传你越。",
        "num": 7,
        "star_num": "7497",
        "create_time": "2005-08-15 08:24:36",
        "state": {
          "star_status": false,
          "favorite_list": [
            {
              "id": "350000198309081440",
              "name": "元包边强团",
              "description": "求象北器目深青消元马马压龙市步。",
              "num": 90,
              "create_time": "1995-06-18 17:00:13",
              "state": {
                "owner_status": true
              },
              "configuration": {
                "public_config": false
              }
            },
            {
              "id": "710000200801013877",
              "name": "易队要量八资",
              "description": "取响京育很容水且声置石商基。",
              "num": 33,
              "create_time": "1982-10-23 08:15:50",
              "state": {
                "owner_status": true
              },
              "configuration": {
                "public_config": false
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
        "id": "310000198710276730",
        "name": "况日消太很速",
        "cover": "http://dummyimage.com/120x240",
        "description": "度生外按单温干入前经习手形定价。",
        "num": 68,
        "star_num": "2445",
        "create_time": "2012-09-22 15:14:55",
        "state": {
          "star_status": true,
          "favorite_list": [
            {
              "id": "530000199508208457",
              "name": "平类精王被查",
              "description": "知争红习备界众育众成品则九重然。",
              "num": 26,
              "create_time": "2008-10-20 20:35:27",
              "state": {
                "owner_status": false
              },
              "configuration": {
                "public_config": false
              }
            },
            {
              "id": "150000199709054027",
              "name": "集飞还",
              "description": "约以议历更建于高体专书单属复什观性。",
              "num": 81,
              "create_time": "2003-03-29 09:19:56",
              "state": {
                "owner_status": true
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
        "followers_num": "5397万"
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

### Responses

|HTTP Status Code |Meaning|Description|Data schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|成功|Inline|

### Responses Data Schema

HTTP Status Code **200**

|Name|Type|Required|Restrictions|Title|description|
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
|»»» title|string|true|none|标题|笔记标题|
|»»» cover|string|false|none|封面图片|图片URL，此项非必须。若笔记无自带封面：内容含有图片时默认返回首图，若包含音频而不包含图片则返回默认音频图片，若包含视频则返回视频切片。|
|»»» view_num|string|true|none|浏览量|none|
|»»» star_num|string|true|none|收藏量|none|
|»»» comment_num|string|true|none|评论量|none|
|»»» create_time|string|true|none|创建时间|笔记创建的时间|
|»»» update_time|string|true|none|修改时间|笔记最后一次修改的时间|
|»» collection_list|[[collection](#schemacollection)]|true|none|笔记所属合集列表|合集列表|
|»»» 合集|[collection](#schemacollection)|false|none|合集|笔记所归属的合集详情|
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
|»»»»» owner_status|boolean|true|none|是否拥有|合集是否属于自己|
|»»»» configuration|[collection-configuration](#schemacollection-configuration)|false|none|合集配置项|仅作者为当前用户时返回，此项非必须|
|»»»»» public_config|boolean|true|none|是否公开|是否公开|
|»» content|string|true|none|内容|Markdown内容|
|»» state|[note-state](#schemanote-state)|false|none|当前用户状态量|此项非必须|
|»»» star_status|boolean|true|none|是否收藏|当前用户是否收藏|
|»»» owner_status|boolean|true|none|是否拥有|是否为当前拥有|
|»» configuration|[note-configuration](#schemanote-configuration)|false|none|笔记配置项|仅作者为当前用户时返回，此项非必须|
|»»» public_config|boolean|true|none|是否公开|是否公开|
|»»» free_config|integer|true|none|是否免费|若值为0，则免费，其余则收费，单位：分|

# Data Schema

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

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
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
      "followers_num": "string"
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
    "followers_num": "string"
  },
  "create_time": "string"
}

```

账单

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
|---|---|---|---|---|---|
|id|string|true|none|账单ID|数字编号形式的账单ID|
|type|integer|true|none|类型|账单类型|
|amount|string|true|none|账单金额|三位逗分|
|note|[note-overview](#schemanote-overview)|true|none|笔记|账单所关联的笔记|
|counterpart|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|create_time|string|true|none|创建时间|订单创建时间|

#### Enum

|Name|Value|
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

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
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
        "followers_num": "string"
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
      "followers_num": "string"
    },
    "create_time": "string"
  },
  "read_status": true
}

```

交易通知

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
|---|---|---|---|---|---|
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
    "followers_num": "string"
  },
  "content": "string",
  "create_time": "string"
}

```

评论概览

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
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
  "note": {
    "id": "string",
    "author": {
      "id": "string",
      "username": "string",
      "avatar": "string",
      "biography": "string",
      "gender": "string",
      "follows_num": "string",
      "followers_num": "string"
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
      "followers_num": "string"
    },
    "content": "string",
    "create_time": "string"
  },
  "read_status": true
}

```

评论通知

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
|---|---|---|---|---|---|
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
  "user": {
    "id": "string",
    "username": "string",
    "avatar": "string",
    "biography": "string",
    "gender": "string",
    "follows_num": "string",
    "followers_num": "string"
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
      "followers_num": "string"
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

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
|---|---|---|---|---|---|
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
  "user": {
    "id": "string",
    "username": "string",
    "avatar": "string",
    "biography": "string",
    "gender": "string",
    "follows_num": "string",
    "followers_num": "string"
  },
  "follow_time": "string",
  "read_status": true
}

```

关注通知

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
|---|---|---|---|---|---|
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
    "followers_num": "string"
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
      "followers_num": "string"
    },
    "title": "string",
    "cover": "string",
    "view_num": "string",
    "star_num": "string",
    "comment_num": "string",
    "create_time": "string",
    "update_time": "string"
  },
  "agree_num": 0,
  "create_time": "string",
  "sub_comment": [
    {
      "id": "string",
      "user": {
        "id": "string",
        "username": "string",
        "avatar": "string",
        "biography": "string",
        "gender": "string",
        "follows_num": "string",
        "followers_num": "string"
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
          "followers_num": "string"
        },
        "title": "string",
        "cover": "string",
        "view_num": "string",
        "star_num": "string",
        "comment_num": "string",
        "create_time": "string",
        "update_time": "string"
      },
      "agree_num": 0,
      "create_time": "string",
      "sub_comment": [
        {
          "id": "string",
          "user": {
            "id": null,
            "username": null,
            "avatar": null,
            "biography": null,
            "gender": null,
            "follows_num": null,
            "followers_num": null
          },
          "text": "string",
          "image_list": [
            "string"
          ],
          "audio": "string",
          "video": "string",
          "note": {
            "id": null,
            "author": null,
            "title": null,
            "cover": null,
            "view_num": null,
            "star_num": null,
            "comment_num": null,
            "create_time": null,
            "update_time": null
          },
          "agree_num": 0,
          "create_time": "string",
          "sub_comment": [
            {}
          ]
        }
      ]
    }
  ]
}

```

评论

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
|---|---|---|---|---|---|
|id|string|true|none|评论ID|数字编号形式的评论ID|
|user|[user](#schemauser)|true|none|交易对象|账单创建时另外一方用户信息|
|text|string|false|none|评论文本|非必须项|
|image_list|[string]|false|none|评论图片|评论图片列表，非必须项|
|audio|string|false|none|评论语音|语音URL|
|video|string|false|none|评论视频|视频URL|
|note|[note-overview](#schemanote-overview)|false|none|笔记|账单所关联的笔记|
|agree_num|integer|true|none|赞同数|评论的点赞数量|
|create_time|string|true|none|评论时间|评论发布时间|
|sub_comment|[[comment](#schemacomment)]|true|none|子评论|none|

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

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
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
        "public_config": true
      }
    }
  ],
  "owner_status": true
}

```

合集用户状态量

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
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
  "public_config": true
}

```

收藏夹配置项

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
|---|---|---|---|---|---|
|public_config|boolean|true|none|是否公开|是否公开|

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

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
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

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
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
        "public_config": true
      }
    }
  ],
  "owner_status": true
}

```

笔记用户状态量

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
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
    "public_config": true
  }
}

```

收藏夹

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
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
          "public_config": true
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

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
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
      "followers_num": "string"
    },
    "title": "string",
    "cover": "string",
    "view_num": "string",
    "star_num": "string",
    "comment_num": "string",
    "create_time": "string",
    "update_time": "string"
  },
  "collection_list": [
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
            "id": null,
            "name": null,
            "description": null,
            "num": null,
            "create_time": null,
            "state": null,
            "configuration": null
          }
        ],
        "owner_status": true
      },
      "configuration": {
        "public_config": true
      }
    }
  ],
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

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
|---|---|---|---|---|---|
|overview|[note-overview](#schemanote-overview)|true|none|笔记|账单所关联的笔记|
|collection_list|[[collection](#schemacollection)]|true|none|笔记所属合集列表|合集列表|
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
  "followers_num": "string"
}

```

用户

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
|---|---|---|---|---|---|
|id|string|true|none|用户ID|数字编号形式的用户ID|
|username|string|true|none|用户名|none|
|avatar|string|true|none|用户头像|头像图片URL|
|biography|string|false|none|个人简介|此项非必须|
|gender|string|true|none|性别|取值范围：男、女和未知|
|follows_num|string|true|none|关注数|用户关注的数量|
|followers_num|string|true|none|粉丝数|用户粉丝的数量|

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
    "followers_num": "string"
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

### Attribute

|Name|Type|Required|Restrictions|Title|Description|
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

