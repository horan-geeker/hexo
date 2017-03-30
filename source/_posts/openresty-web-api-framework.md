---
title: Openresty web api framework
date: 2017-03-30 13:10:48
tags: 后端
---
## 这是一个非常易用简单的 web api 框架，采取了一些较好的 php 框架的设计

#### 主要目录结构分为 lib（公共方法,核心方法）,models（下边的文件均与数据库相关,但并不是纯orm层,可以在内部定义一些业务逻辑,类似于laravel的model,真正的model放在lib目录下）controller（由 nginx location 指向的 lua文件也就是控制器）

以下是一些基本用法：
```
local cjson = require('cjson')
local conf = require('config.app')
local Model = require('models.model')
local request = require('lib.request')
local validator = require('lib.validator')

--use request to get all http args
ngx.say(cjson.encode(request:all()))
--curl "localhost:8001?id=1" -d name=foo     
--{"name":"foo","id":"1"}
local data = request:all()
local ok,msg = validator:check({
    name = {max=6,min=4},
    'id',
    data)

if not ok then
    ngx.say(msg)
end

local User = Model:new('users')
ngx.say('where demo:\n',cjson.encode(User:where('username','=','cgreen'):where('password','=','7c4a8d09ca3762af61e59520943dc26494f8941b'):get()))
-- {"password":"7c4a8d09ca3762af61e59520943dc26494f8941b","gender":"?","id":99,"username":"cgreen","email":"jratke@yahoo.com"}

ngx.say('orwhere demo:\n',cjson.encode(User:where('id','=','1'):orwhere('id','=','2'):get()))
-- {"password":"7c4a8d09ca3762af61e59520943dc26494f8941b","gender":"?","id":1,"username":"hejunwei","email":"hejunweimake@gmail.com"},
-- {"password":"7c4a8d09ca3762af61e59520943dc26494f8941b","gender":"?","id":2,"username":"ward.antonina","email":"hegmann.bettie@wolff.biz"}

local Admin = Model:new('admins')
local admin = Admin:find(1)
ngx.say('find demo:\n',cjson.encode(admin))
-- {"password":"d033e22ae348aeb5660fc2140aec35850c4da997","id":1,"email":"hejunwei@gmail.com","name":"admin"}
--Admin:update({name='update demo'}):where('id','=','3'):query()
Admin:update({
        name='update test',
        password="111111"
    }):where('id','=',3):query()

Admin:insert({
    id=3,
    password='123456',
    name='horanaaa',
    email='horangeeker@geeker.com',
})
```

## github:[nana framework](https://github.com/horan-geeker/nana)
