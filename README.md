# restapi-down
定义了一个简单编写`restful json api`文档的规范。类似`markdown`格式，在文本编辑中根据简单格式及语法进行API文档的编写，然后通过生成器转化至规范的html格式的API文档。

## 简单示例

>***语法代码***
	
	@api: 获取特定商品分类下的商品列表信息。
	@get: /products/:type/:page?:keywords&:size
	@params:
		type:商品分类
		page:页码
		keywords:检索关键字
		size:分页大小
	@body:
	@return:
		status:结果状态
		success:是否成功
		error:错误信息
		data:结果内容
			id:商品主键
			name:商品名称
			keywords:商品关键字
				...
			shop:商家信息
				name:商家名称
				city:所在城市
			photos:商品照片
				url:照片地址
				name:照片名称
				...
			createtime:date:创建时间

>***语法代码(含类型定义)***
	
	@api: 获取特定商品分类下的商品列表信息。
	@get: /products/:type/:page?:keywords&:size
	@params:
		type:int:商品分类
		page:int:页码
		keywords:string:检索关键字
		size:int:分页大小
	@body:
	@return:
		status:int:结果状态
		success:bool:是否成功
		error:string:错误信息
		data:object:结果内容
			id:long:商品主键
			name:string:商品名称
			keywords:list:商品关键字
				:string
			shop:object:商家信息
				name:string:商家名称
				city:string:所在城市
			photos:list:商品照片
				url:string:照片地址
				name:string:照片名称
			createtime:date:创建时间
			
## 格式说明

文档解析器将根据强制缩进进行解析，类似于Python的结构，整个文档结构分为两大部分：

 - 接口标记
 - 参数标记

### 接口标记

#### @api

标记接口的注释说明，同时为接口文档段落的开始；

#### @get @post @put @delete 

标记接口的HTTP方法类型，并说明该接口的请求地址，地址中存在参数的使用`:参数名称`进行参数标记；

#### @params

说明地址中存在的参数列表说明，使用`参数名称:类型:注释说明`格式进行变量的说明，也可以忽略类型进行简写`参数名称:注释说明`；

#### @body

HTTP BODY内容说明，使用参数标记结构对内容主体的说明*（详细参考参数标记章节)*；

#### @return

HTTP响应的内容说明，使用参数标记结构对响应内容的说明*（详细参考参数标记章节)*；

### 参数标记

用于标记文档中需要针对参数及对象等方面的说明，参数标记的基本结构为：`参数名称:类型:注释说明`或忽略类型进行简写`参数名称:注释说明`。多个参数或对象属性使用回车换行区分，多层级参数或对象属性，使用缩进的方式进行区分。

#### 多参数及多字段

	status:int:结果状态
	success:bool:是否成功
	error:string:错误信息

类型说明支持:bool，int，long，float，string，date，object，list；除这些类型之外可以定义自己的类型，但可能不被解析器所支持，无法生成格式文档。

#### 简单数组

	keywords:list:商品关键字
		:string

当存在类型说明时，下级缩进将说明数组内元素的类型，如果忽略类型进行简写，必须使用`...`已标记该字段为数组，无类型说明的示例：

	keywords:商品关键字
		...

#### 对象数组
	
	photos:list:商品照片
		url:string:照片地址
		name:string:照片名称
		
对象数组与简单数组的区别在于，简单数组使用了空名称占位的方式进行说明，如：`:string`，而包含完整参数标记说明格式的就是对象数组，如果忽略类型进行简写，必须使用`...`已标记该字段为数组，无类型说明的示例：

	photos:商品照片
		url:照片地址
		name:照片名称
		...
		
#### 对象嵌套

	data:object:结果内容
		id:long:商品主键
		name:string:商品名称
		
使用缩进说明对象嵌套的层级关系，，无类型说明的示例：

	shop:商家信息
		name:商家名称
		city:所在城市
			
## 生成预览

接口说明：获取特定商品分类下的商品列表信息。

请求地址：GET:/products/:type/:page?:keywords&:size

请求参数：

| 参数名称 | 参数类型 | 说明                                    |
|---------|---------|----------------------------------------|
| type    | int     | 商品分类                                |
| page    | int     | 页码                                   |
| page    | int     | 页码                                   |
| keywords| string  | 检索关键字                              |
| size    | int     | 分页大小                                |

内容主体：无

返回类型：

- 主体内容

| 参数名称 | 参数类型 | 说明                                    |
|---------|---------|----------------------------------------|
| status  | int     | 结果状态                                |
| success | bool    | 是否成功                                |
| error   | string  | 页码                                   |
| data    | object  | 结果内容                                |


- 字段说明（data）：

| 参数名称    | 参数类型      | 说明                            |
|------------|-------------|---------------------------------|
| id         | long        | 商品主键                         |
| name       | string      | 商品名称                         |
| error      | string      | 页码                            |
| keywords   | list:string | 商品关键字                       |
| shop       | object      | 商家信息                         |
| photos     | list:object | 商品照片                         |
| createtime | date        | 创建时间                         |

- 字段说明（data.shop）：

| 参数名称 | 参数类型 | 说明                                    |
|---------|---------|----------------------------------------|
| name    | string  | 商家名称                                |
| city    | string  | 所在城市                                |

- 字段说明（data.photos）：

| 参数名称 | 参数类型 | 说明                                    |
|---------|---------|----------------------------------------|
| url     | string  | 照片地址                                |
| name    | string  | 照片名称                                |


