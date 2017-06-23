# douban-api
我想写一个自动发送豆邮的脚本，具体思路是：
1. 获取指定一个或/多个小组内的成员。
2. 通过某种方式过滤成员， 例如用注册年份来过滤掉最新注册的成员，或者使用Google Vision API来选择头像为女性的用户。
3. 然后获取每个成员的user id。
4. 然后给这些人发送豆邮。然而这一步需要API Key。需要找一个方法来获取API key。




下面是我关于豆瓣API做的一些研究。
首先呢，我们希望获取一个小组内的所有成员的名称。我们可以通过如下获得。
`https://api.douban.com/v2/group/:group_id/members`

那我们如何获取小组id呢？

获取小组id可以通过浏览器的检查功能查看小组头像的名字。以深圳单身色这个小组为例，当我们查看小组头像的source的时候，source是这样的`https://img3.doubanio.com/icon/g237153-5.jpg`, 在这个例子中这个小组的小组id就是“237153”，g就代表group。我们也可以用同样的方法来获取用户的id，这里就不再举例子了。

那既然我们知道了如何获取小组id，我们就试着向端点发出一个GET请求`https://api.douban.com/v2/group/76728/members?count=100&start=10`

他的回复是：
```
{
count: 20,
start: 0,
total: 23455,
members: [
{
name: "X_inge",
is_suicide: false,
avatar: "https://img1.doubanio.com/icon/user_normal.jpg",
uid: "55644233",
alt: "https://www.douban.com/people/55644233/",
type: "user",
id: "55644233",
large_avatar: "https://img3.doubanio.com/icon/user_large.jpg"
},
...
]
}
```
我们可以看到默认的回复用户数是10个，但是我们可以更改获取的用户个数。经过测试，一次性最多可以获取100个用户，然而我们可以通过调整开始位置来获取所有的用户：
`https://api.douban.com/v2/group/76728/members?count=100&start=10`。

一些其他的指令包括：
`https://api.douban.com/v2/group/:group_id/` 该指令可以获得豆瓣小组的一些基本信息。

一个更完整的Reference可以通过这个[链接](https://www.douban.com/group/topic/33507002/)获得。

获取了加入小组的所有用户之后我们可以来查看用户的具体信息，例如说注册时间，头像等。这可以通过
```https://api.douban.com/v2/user/:user_id```

回复如下
```
{
    "loc_id": "118282",
    "name": "sophia曾",
    "created": "2012-01-08 21:07:22",
    "is_banned": false,
    "is_suicide": false,
    "loc_name": "广东深圳",
    "avatar": "https://img3.doubanio.com/icon/u57619152-3.jpg",
    "signature": "",
    "uid": "57619152",
    "alt": "https://www.douban.com/people/57619152/",
    "desc": "",
    "type": "user",
    "id": "57619152",
    "large_avatar": "https://img3.doubanio.com/icon/up57619152-3.jpg"
}
```

接下来我们需要采取步骤来过滤一下用户，因为我们不希望给所用的用户都发送豆邮。

然后我们需要通过脚本来实现自动发送豆邮的功能，然而发送豆邮的功能是需要api key的，因此我们需要寻找一个方法来获取API key。


