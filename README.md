# 大屏和场控注册机制以及数据格式
> 抽奖socket改造

## 身份(role)和状态
### 身份
1. 场控
2. 大屏

### 状态
1. 每个连接端都有一个唯一的id

## 注册流程

1. 首次注册
    1. 接入端向socket server发送一个注册请求
    2. socket server 给这个请求端发送一个接收响应，并给它一个唯一id
    3. socket server 向所有已接入的端广播接入事件，并告诉它们接入的角色(即场控还是大屏)
    
2. 断开后注册
    1. 接入端携带断开前的id向socket server发送一个注册请求
    2. socket server 给这个请求端发送一个接收响应，并返回它的id
    3. socket server 向所有已接入的端广播接入事件，并告诉它们接入的角色(即场控还是大屏)
    
## 断开通知机制
1. socket server 触发断开事件，并获取断开端的信息
2. socket server 向所有已接入的端断开事件


## 数据格式

1. 向socket server注册的数据格式

```json
{
    "action"  : "register",
    "meet_id" : $MEET_ID,
    "sequence": $SEQUENCE,
    "role"    : $ROLE,
    "id"      : $ID?
}
```

2. socket server 响应注册并返回的数据格式

```json
{
    "action": "accept",
    "id"    : $ID
}
```
3. socket server 向所有端广播接入事件的数据格式

```json
{
    "action": "connect",
    "role"  : $ROLE,
    "screen_number": $SCREEN_NUMBER,
    "controller_number": $CONTROLLER_NUMBER
}
```
4. socket server 向所有端广播断开事件的数据格式

```json
{
   "action": "disconnect",
   "role"  : $ROLE,
   "screen_number": $SCREEN_NUMBER,
   "controller_number": $CONTROLLER_NUMBER
}
```

## 数据说明
### ACTIONS
1. register  
    发送注册请求的action  
2. accept  
    发送注册成功的action
3. connect  
    广播接入的action
4. disconnect  
    广播断开的action
5. data  
    具体业务逻辑的action

### ROLES
1. screen  
    大屏
2. controller  
    场控
