# RESTfulAPI 设计规范


## URL规则
URL版本号：将版本号放在URL里

URL结尾：所有接口使用以"/"结尾，url不以"/"结尾，则重定向以"/"结尾（根据RFC3986定义）

URL不要包含文件扩展名：不能出现.php，.json结尾，请在HTTP  Header中使用Content-Type指出

URL过滤：过滤筛选参数要求都在“ ？”后给予，必须参数则在“ ？”前面以“/”给予
例：GET /admin/1/user/10    //必须参数为1和10
	/admin/1/user/10?sortby=name&order=asc  //sortby和order为过滤参数


## 命名规则
脊椎命名法：整体要求全部小写字母。使用连字符 "-" 分割单词。例子：spinal-case （根据RFC3986定义）


## HTTP规则
### HTTP方法：

 ```
		get -只用做资源的读取。
		post-通过用作创建一个新的资源。
		delete-通过用作资源的删除。
		put -通过用作更新资源或者创建资源
		head-只获取某个资源的头部信息。
 ```

### 查询参数：
	
  ```
		page：指定第几页（默认从1开始）
		limit：指定返回的数量
		offset：指定记录开始位置（默认为0）
  ```


### 资源分页集合数据

 ```
	{
	  "page": 1,            # 当前是第几页
	  "pages": 3,           # 总共多少页
	  "per_page": 10,       # 每页多少数据
	  "has_next": true,     # 是否有下一页数据
	  "has_prev": false,    # 是否有前一页数据
	  "total": 27           # 总共多少数据
	}
 ```

## 状态码规则

   ### 常用code
		113	required_parameter_is_missing 缺少参数
		200 OK - [GET]：请求成功
		201 CREATED - [POST/PUT/PATCH]：创建成功
		202 Accepted - [*]：更新成功
		204 NO CONTENT - [DELETE]：删除数据成功。
		205 Reset Content - 重复内容
		400 INVALID REQUEST - [POST/PUT/PATCH]：请求不存在
		401 Unauthorized - [*]：未授权
		403 Forbidden - [*] 禁止访问
		404 NOT FOUND - [*]：资源不存在
		500 INTERNAL SERVER ERROR - [*]：服务器内部错误

   ### 其它code
		100	invalid_request_scheme  - 错误的请求协议
		101	invalid_request_method  - 错误的请求方法
		102	access_token_is_missing  - 未找到 access_token
		103	invalid_access_token  - access_token 不存在或已被删除
		104	invalid_apikey  - apikey 不存在或已删除
		105	apikey_is_blocked  - apikey 已被禁用
		106	access_token_has_expired  - access_token 已过期
		107	invalid_request_uri  - 请求地址未注册
		116	client_secret_mismatch  - client_secret不匹配
		117	redirect_uri_mismatch  - redirect_uri不匹配
		121	invalid_user  - 用户不存在或已删除
		126	invalid_request_source  - 访问来源不合法
		128	user_locked  - 用户被锁定
		999	unknown  - 未知错误


## 其它注意事项：

	get请求必须是安全的，即有关操作数据的请求尽量使用POST，防止搜索引擎收录，访问接口造成重复请求。(根据RFC3986定义)


## 返回数据示例格式为：

	返回例子：

  ```
		{	"ec": 100,
			"em": "消息",
			"data": {
				"gold": 10000,
				"roomcard": 10086
				}
		}
		
  ```
  
	返回失败例子：
	
  ```
		{	"ec": 400,
			"em": "消息",
			"data": {}
		}
  ```