# WinterDemo
红岩网校后端研发部2024年寒假作业题目

关于本接口文档和作业
1. 必须完成全部的基础功能，截至时间：2025年2月16日23:59:59前
2. 将你的项目文件上传至GitHub，提交时只提交你的项目仓库地址，至be@redrock.team
3. 基础功能的接口已在本文档给出，仅作示例请根据自己的实际进行修改。 
4. 本接口文档对于用户信息的验证全部为token验证。
5. 本文档采用RESTful风格的API虽然写的很拉 ，如果有路径参数会在访问路径上用 {} 标明，并且会在请求参数中解释其含义。
对于返回值，我们规定返回以下格式的 json：
 {
      status: 10000,      // 一个5位数的状态码，正常返回下为10000，其他非正常状态下的状态码需要自行定义
      "info": "success",  // 本次请求的状态文字信息，正常返回下为success，其他非正常状态下的需要自行定义
      "data": {           // 一个json对象，里面包含其他要返回的数据的键值对
          "param1": "...",
          "param2": "..."
      }
  }
我们解释返回参数时，只解释data字段里的参数值
1. Web框架可使用gin、Hertz两个框架，如果使用了其他第三方库，请在你的README.md文件中展示你对该库的理解与该库是如何，以及你为什么要使用这个第三方库，以及这个库在你的作业中的起了什么作用。
2. 对于某个接口的讲解如下：
3. 请注意一个操作所牵扯到的其他数据，并且要注意操作的原子性。
4. 一定要对入参进行合法性校验
5. 商品内容的图片展示和存储等不做要求，只需要像商品信息的字段link给出一个链接即可（可以是自己编造的，也可以像这个链接一样IPone16）
6. 有任何疑问请问学姐
基础功能
类似一个商品网站，可参考淘天集团 淘宝
- 用户的注册与登录
- 修改用户的信息
- 查询商品
- 查看商品详情
- 查看分类下的商品
- 给商品进行评论（评论的增删查改）
- 商品加入购物车
- 获取购物车所有商品
- 下单（只需向前端返回一个订单order，实际业务较为复杂可能会接触一些微服务相关内容）
加分项
- 完善订单服务
- 嵌套评论
- 匿名评论
- 浏览的商品的历史记录
- 商家和管理员的出现（可以自由增删商品）
- 与商家客服的聊天
- 单一的登录较为不安全，试着加入验证码操作
- 使用缓存
- 设计一套热度算法，使得能让该用户常看的商品分类下的商品出现在首页
- 用户可以与客服进行聊天
- 部署到自己的服务器上，并且可以访问
- 考虑安全性（xxs，sql注入，csrf等）
- 你任何想加的功能
基础功能接口
此处的接口文档仅作参考，请根据你的项目实际情况对参数以及返回示例进行增删改查，如有错误请忽略联系学姐
返回示例未作错误示例的处理，请自行结合代码实际情况对返回示例的异常返回进行展示
用户相关
注册
请求路径：POST /user/register
请求头：
请求参数：application/json
暂时无法在飞书文档外展示此内容
返回参数：
返回示例：
成功返回：
{
  "status": 10000,
  "info": "success"
}
登录（获取token）
请求路径： GET /user/token
请求头: 
请求参数：application/json
暂时无法在飞书文档外展示此内容
返回参数：
暂时无法在飞书文档外展示此内容
返回示例：
成功返回
 {
   "status": 10000,
   "info": "success",
   "data": {
     "refresh_token": "{refresh_token}",
     "token": "{token}"
   }
 }

刷新token（维持登录状态）
请求路径： GET /user/token/refresh
请求头：
暂时无法在飞书文档外展示此内容
请求参数: application/json
暂时无法在飞书文档外展示此内容
返回参数：
暂时无法在飞书文档外展示此内容
返回示例：
成功返回
{
  "status": 10000,
  "info": "success",
  "data": {
    "refresh_token": "{refresh_token}",
    "token": "{token}"
  }
}
修改用户密码
请求路径：PUT /user/password
请求头：
暂时无法在飞书文档外展示此内容
请求参数：application/json
暂时无法在飞书文档外展示此内容
返回参数：
返回示例：
成功返回
{
  "info": "success",
  "status": 10000
}
获取用户信息
请求路径：GET /user/info/{user_id}
请求头: 
暂时无法在飞书文档外展示此内容
请求参数：query
暂时无法在飞书文档外展示此内容
返回参数：
暂时无法在飞书文档外展示此内容
返回示例：
成功返回
{
  "status": 10000,
  "info": "success",
  "data": {
      "user": {
      "id": "123456",
      "avatar": "{avatar_url}",
      "nickname": "你好世界",
      "introduction": "永远少年",
      "phone": 12345678900,
      "qq":123456789,
      "gender": "未知",
      "email": "test@gmail.com",
      "birthday": "2022-01-02"
    }
  }
}
修改用户信息
请求路径：PUT /user/info
该接口用于更改用户信息，除token外，全部请求参数都是非必选的，要求该接口有某个参数才修改对应的信息。比如我请求时只选了nickname和introduction字段，那么我们只改用户昵称和简介的信息，其他信息保持原状。
请求头: 
暂时无法在飞书文档外展示此内容
请求参数：application/json
暂时无法在飞书文档外展示此内容
返回参数：
返回示例：
成功返回
{
   "info": "success",
   "status": 10000
 }
商品相关
获取商品列表
请求路径：GET /product/list
请求头：
请求参数：
字段名
类型
说明
products
复杂数据类型数组
商品的信息
返回参数：
返回示例：
成功返回
{
  "status": 10000,
  "info": "success",
  "data": {
    "products":[
      {
        "product_id": "1",
        "name": "傲慢与偏见",
        "description": "一本书",
        "type":"book",
        "comment_num": 35,
        "price": 9.8,
        "is_addedCart":true
        "cover": "http://127.0.0.1/picture_url1",
        "publish_time": "1980-11-07",
        "link": "http://127.0.0.1/test1"
      },
      {
        "product_id": "2",
        "name": "T-shirt",
        "description": "一件短袖",
        "type":"clothes",
        "comment_num": 100,
        "price": 88.88,
        "is_addedCart":false
        "cover": "http://127.0.0.1/picture_url2",
        "publish_time": "1980-11-07",
        "link": "http://127.0.0.1/test2"
      }
    ]
  }
}
搜索商品
请求路径 ：GET /book/search
请求头：
暂时无法在飞书文档外展示此内容
请求参数：query
暂时无法在飞书文档外展示此内容
返回参数：
暂时无法在飞书文档外展示此内容
返回示例：
成功返回
{
  "status": 10000,
  "info": "success",
  "data": {
      "products":[
      {
        "product_id": "1",
        "name": "傲慢与偏见",
        "description": "一本书",
        "type":"book",
        "comment_num": 35,
        "price": 9.8,
        "is_addedCart":true
        "cover": "http://127.0.0.1/picture_url1",
        "publish_time": "1980-11-07",
        "link": "http://127.0.0.1/test1"
      }
}
加入购物车
请求路径：PUT /product/addCart
请求头：
暂时无法在飞书文档外展示此内容
请求参数：x-www-form-urlencoded/form-data
暂时无法在飞书文档外展示此内容
返回参数：
返回示例：
成功返回
{
  "info": "success",
  "status": 10000
}
获取购物车商品列表
请求路径 ： GET /product/cart
请求头：
暂时无法在飞书文档外展示此内容
请求参数：query
暂时无法在飞书文档外展示此内容
返回参数
字段名
类型
说明
products
复杂数据类型数组
商品的信息
返回示例：
{
  "status": 10000,
  "info": "success",
  "data": {
    "products":[
      {
        "product_id": "1",
        "name": "傲慢与偏见",
        "type":"book",
        "price": 9.8,
        "cover": "http://127.0.0.1/picture_url1",
        "link": "http://127.0.0.1/test1",
        "num":114
      },
      {
        "product_id": "2",
        "name": "T-shirt",
        "type":"clothes",
        "price": 88.88,
        "cover": "http://127.0.0.1/picture_url2",
        "link": "http://127.0.0.1/test2",
        "num":514
      }
    ],
    "accout":114514
  }
}
获取商品详情
请求路径：GET /product/info/{product_id}
请求头：
请求参数：query
暂时无法在飞书文档外展示此内容
返回参数：
暂时无法在飞书文档外展示此内容
返回示例：
{
  "status": 10000,
  "info": "success",
  "product_id": "1",
  "name": "傲慢与偏见",
  "description": "一本书",
  "type":"book",
  "comment_num": 35,
  "price": 9.8,
  "is_addedCart":true
  "cover": "http://127.0.0.1/picture_url1",
  "publish_time": "1980-11-07",
  "link": "http://127.0.0.1/test1"
}
获取相应标签的商品列表
请求路径 GET /product/{type}
请求头：
请求参数：query
暂时无法在飞书文档外展示此内容
返回参数：
暂时无法在飞书文档外展示此内容
返回示例：
成功返回
{
  "status": 10000,
  "info": "success",
  "data": {
    "products":[
     "product_id": "114",
        "name": "一个商品",
        "description": "仙贝",
        "type":"other",
        "comment_num": 35,
        "price": 9.8,
        "is_addedCart":true
        "cover": "xxxx",
        "publish_time": "2001-12-05",
        "link": "xxxxxx"
    ],
  }
}
评论相关
获取商品的评论
请求路径： GET /comment/{product_id}
请求头：
请求参数：query
暂时无法在飞书文档外展示此内容
返回参数：
暂时无法在飞书文档外展示此内容
返回示例:
成功返回
{
  "info": "success",
  "status": 10000,
  "comments": [
    {
      "post_id": "79",
      "publish_time": 503270377478,
      "content": "occaecat est voluptate qui officia",
      "user_id": "55",
      "avatar": "http://dummyimage.com/100x100",
      "nickname": "康静",
      "praise_count": 65,
      "is_praised": 1,（0为未处理， 1为点赞， 2为点踩）
      "product_id": "99"
    },
    {
      "post_id": "80",
      "publish_time": 503270377478,
      "content": "occaecat est voluptate qui officia",
      "user_id": "55",
      "avatar": "http://dummyimage.com/100x100",
      "nickname": "康静",
      "praise_count": 65,
      "is_praised": 1,
      "product_id": "99"
    }
  ]
}
给商品评论
请求路径： POST /comment/{product_id}
请求头：
暂时无法在飞书文档外展示此内容
请求参数：application/json
暂时无法在飞书文档外展示此内容
返回参数：
暂时无法在飞书文档外展示此内容
返回示例：
{
  "info": "success",
  "status": 10000,
  "data": "{comment_id}"
}
删除书评
删除评论的时候需要删除相关点赞信息
请求路径：DELETE /comment/{comment_id}
请求头：
暂时无法在飞书文档外展示此内容
请求参数：query
暂时无法在飞书文档外展示此内容
返回参数：
返回示例：
成功返回
{
  "info": "success",
  "status": 10000
}
更新评论
请求路径： PUT /comment/{comment_id}
请求头：
暂时无法在飞书文档外展示此内容
返回参数：
请求参数：application/json
暂时无法在飞书文档外展示此内容
返回示例：
成功返回
{
  "info": "success",
  "status": 10000
}
操作相关
点赞点踩
请求路径： PUT /comment/praise
请求头： 
暂时无法在飞书文档外展示此内容
请求参数：form-data/x-www-form-urlencoded
暂时无法在飞书文档外展示此内容
返回参数：
返回示例：
成功返回
{
    "info": "success",
    "status": 10000
}
下单
请求路径： POST /operate/order
请求头：
暂时无法在飞书文档外展示此内容
请求参数：application/json
暂时无法在飞书文档外展示此内容
返回参数：
暂时无法在飞书文档外展示此内容
返回示例：
成功返回
{
  "info": "success",
  "status": 10000,
  "order_id":233
}
结语
亲爱的同学们，我们本学期的课程伴随着本次的寒假作业的开始而结束了，很感谢同学们的坚持，即使我们的课讲得并不好，也依旧坚持学习（虽然可能有的同学中途因为种种事而脱节了）。无论如何，你们在过去的一学期里，都付出了努力。可能部分同学的基础较为薄弱，深受打击也不要因此而放弃，可以借此时机慢慢努力进行补充。每个人都有自己的节奏与学习方式，不要受到其他人的影响，只要在进步那么一切都是有意义的。
新的一年即将到来，在这里提前先祝福大家新年快乐，蛇年阖家幸福（希望不要大年初一还在敲代码，同家人一起开心过年才是重要的）。
                                                                                                                --by 红岩网校后端研发部
[图片]
