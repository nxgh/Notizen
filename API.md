## Posts

### `GET ` /posts

- 描述： 获取文章列表
- QueryParams

| 字段 | 类型 | 必选 | 描述   |
| ---- | ---- | ---- | ------ |
| page | int  | N    | 文章页 |

示例

`axios.get('/post?page=1')`

相应内容

| 字段      | 类型   | 必选 | 描述   |
| --------- | ------ | ---- | ------ |
| id        |        | Y    | 文章id |
| title     | string | Y    | 标题   |
| timestamp | string | Y    | 时间戳 |
| category  | string | Y    | 分类   |

示例

```
{
	"posts": [
		{
			"id": 1,
			"title": "",
			"timestamp": "",
			"category": ""
		}
	]
}
```





### `GET` /post/<post_id>

- 描述： 根据文章id获取文章详情

  **相应内容**

  | 字段      | 类型   | 必选 | 描述     |
  | --------- | ------ | ---- | -------- |
  | id        |        | Y    | 文章id   |
  | title     | string | Y    | 标题     |
  | timestamp | string | Y    | 时间戳   |
  | category  | string | Y    | 分类     |
  | body      | string | Y    | 正文     |
  | comments  | Array  | N    | 评论列表 |

   comments 内容

  | 字段      | 类型   | 必选 | 描述     |
  | --------- | ------ | ---- | -------- |
  | author    | string | Y    | 用户名   |
  | body      | string | Y    | 评论正文 |
  | timestamp | string | N    | 时间戳   |
  | replay    | Array  | Y    | 回复评论 |

  replay 内容

  | 字段      | 类型   | 必选 | 描述     |
  | --------- | ------ | ---- | -------- |
  | author    | string | Y    | 用户名   |
  | timestamp | string | N    | 时间戳   |
  | body      | string | Y    | 评论正文 |

  示例

  ```
  {
  	"id": "",
  	"title": "",
  	"timestamp": "",
  	"body": "",
  	"category": "",
  	"comments": [
  		{
  			"author": "",
  			"body": "",
  			"timestamp": "",
  			"replay": [
  				"author": "",
  				"body": "",
  				"timestamp":"",
  			]
  		}
  	]
  }
  ```

  

### `POST` /post

- 描述： 创建文章， 管理员权限

- headers

  | 字段          | 类型   | 必选 | 描述      |
  | ------------- | ------ | ---- | --------- |
  | Authorization | string | Y    | token令牌 |

- BodyParams

  | 字段     | 类型   | 必选 | 描述 |
  | -------- | ------ | ---- | ---- |
  | title    | string | Y    | 标题 |
  | category | string | Y    | 分类 |
  | body     | string | Y    | 正文 |

  示例

  ```
  {
  	"title": "",
  	"category": "",
  	"body": ""
  }
  ```

  相应内容

  - 创建成功，返回状态码201

  备注

  - 服务端生成时间戳

### `DELETE` /post/<post_id>

- 描述： 根据文章id删除文章, 管理员权限

- headers

  | 字段          | 类型   | 必选 | 描述      |
  | ------------- | ------ | ---- | --------- |
  | Authorization | string | Y    | token令牌 |

  相应内容

  - 返回状态码204

### `PUT` /post/<post_id>

- 描述：根据文章id更新文章, 管理员权限

- headers

  | 字段          | 类型   | 必选 | 描述      |
  | ------------- | ------ | ---- | --------- |
  | Authorization | string | Y    | token令牌 |

- BodyParams

  同 `POST` /post

  响应内容

  - 返回状态码 200

## Category

### `GET` /categories

- 描述：获取分类列表

  响应内容

  | 字段     | 类型   | 必选 | 描述   |
  | -------- | ------ | ---- | ------ |
  | id       | string | Y    | 分类id |
  | category | string | Y    | 分类名 |

### `GET`/category/<category_name>

- 根据分类名返回该分类下所有文章

  相应内容

  - 同`GET`/posts

## User

### `GET` /token

- 描述： token合法性校验, 管理员权限

- headers

  | 字段          | 类型   | 必选 | 描述      |
  | ------------- | ------ | ---- | --------- |
  | Authorization | string | Y    | token令牌 |

  **示例**

  ```
  {
  	"Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpYXQiOjE1Njk1NzA5OTAsImV4cCI6MTU2OTY1NzQyMCwiaXNzIjoiYWRtaW4ifQ.Ft0S3hrThSefGQ4tBE3xkRCybl8RZ_USCPcV63r91aY" 
  }
  ```

  **相应内容**

  - 返回状态码， 无信息
  - 状态码为200， token合法
  - 状态码为403， token不合法

### `POST` /token

- 描述： 管理员登录
- 状态码：201

- BodyParams

| 字段  | 类型   | 必选 | 描述     |
| ----- | ------ | ---- | -------- |
| email | string | Y    | 登录邮箱 |
| pwd   | string | Y    | 登录密码 |

​	示例

```
{
	"email":"test@example.com",
	"pwd": "password"
}
```



- 相应内容

| 字段  | 类型   | 必须 | 描述      |
| ----- | ------ | ---- | --------- |
| token | string | Y    | token令牌 |
| type  | string | Y    | 令牌类型  |

​	示例

```
{
	"token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpYXQiOjE1Njk1NzA5OTAsImV4cCI6MTU2OTY1NzQyMCwiaXNzIjoiYWRtaW4ifQ.Ft0S3hrThSefGQ4tBE3xkRCybl8RZ_USCPcV63r91aY",
	"type": "bearer"
}
```

**token使用方法**

```
headers 中添加 Authorization字段 ，值为"Bearer " + token值
axios.get('/test', {
	headers: {
		Authorization: "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpYXQiOjE1Njk1NzA5OTAsImV4cCI6MTU2OTY1NzQyMCwiaXNzIjoiYWRtaW4ifQ.Ft0S3hrThSefGQ4tBE3xkRCybl8RZ_USCPcV63r91aY" 
	}
})
```

## Comment

### `GET` /comments

- 描述： 获取评论列表, 需管理员权限

- headers

  | 字段          | 类型   | 必选 | 描述      |
  | ------------- | ------ | ---- | --------- |
  | Authorization | string | Y    | token令牌 |

- 响应内容

  示例

```
{
	"comments": [
		{
			"post_id": 1,
			"id": 1,
			"author": "",
			"body": "",
			"timestamp":"",
		}
	]
}
```



### `POST` /comment

- 描述：提交评论
- BodyParams

| 字段    | 类型   | 必须 | 描述     |
| ------- | ------ | ---- | -------- |
| post_id | int    | Y    | 文章id   |
| author  | string | Y    | 用户名   |
| email   | string | Y    | 用户邮箱 |
| body    | string | Y    | 评论正文 |

- 备注
  - 服务端生成时间戳, 及评论id

### `DELETE` /comment/<comment_id>

- 描述: 根据评论id删除评论, 需管理员权限

- headers

  | 字段          | 类型   | 必选 | 描述      |
  | ------------- | ------ | ---- | --------- |
  | Authorization | string | Y    | token令牌 |

- 响应内容
  
  - 返回204状态码   

## 登录逻辑

用户访问Index时，自localStorage中查询有无 token 字段

有 显示admin路由

无 显示 login路由







