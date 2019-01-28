
```flow
st=>start:  开 始
connect=>operation: 饮水机检测系统通过LoRa模组
发送水量过低数据
first=>operation: LoRa网关将数据通过广域网
发送至应用服务器
second=>operation: 应用服务器处理数据并告知用户
饮水机水量过低，询问是否预约送水（用户响应时长为 t ）
judge_warn=>condition: 检测剩余水量是否到达警戒值？
cond=>condition: 用户在 t 时长内作出相应?
user_agree=>condition: 用户同一预约送水？
inspect_balance=>condition: 检查用户账户余额是否充足？
later_send=>operation: 延时 T
third=>operation: 安排送水
recharge=>operation: 引导用户充值
recharge_agree=>condition: 用户同意充值？
end=>end: 结束

st->connect->first->second->cond
cond(yes)->user_agree
cond(no)->judge_warn

user_agree(yes)->inspect_balance
user_agree(no)->end

judge_warn(yes)->later_send->second
judge_warn(no)->end

inspect_balance(yes)->third->end
inspect_balance(no)->recharge->recharge_agree

recharge_agree(yes)->inspect_balance
recharge_agree(no)->end

```


```flow
st=>start: 开始
get_data=>operation: 称重传感器实时监测重量
wakeup=>operation: 饮水机水桶水量过低，称重传感器唤醒电路
first=>operation: 称重传感器将数据反馈
给单片机
cond=>condition: 水量是否小于临界值?
lora=>operation: 通过LoRa模块上行水量数据
end=>end: 结束

st->get_data->wakeup->first->cond
cond(yes)->lora->end
cond(no)->end
```
{#fig:flowmcu}


```flow
start=>start: 开始
oper1=>operation: 触发MAC_EVT_RADIO_TX_START事件
（此事件启动发送流程）
cond1=>condition: 检查SX1278是否空闲?
cond2=>condition: 判断发送是否会超时？
cond3=>condition: 冲突回避后是否存在空中干扰？
cond4=>condition: 2秒内接收到ACK？

oper2=>operation: 1秒之后触发
MAC_EVT_RADIO_RETRANSMIT事件
（此事件将重新尝试发送）
oper3=>operation: 触发MAC_EVT_RADIO_TX_START事件
（此事件调用SX1278冲突回避机制
接口进行空中信号检测）
oper4=>operation: 调用SX1278发送接口将数据发送出去
oper5=>operation: 开启2秒接收窗口接收ACK
oper6=>operation: 触发MAC_EVT_RADIO_TX_FINISH事件
（此事件完成复位发送流程变量，停止发
送流程事件，并将发送结果发送给网络
层）
oper7=>operation: 触发MAC_EVT_RADIO_CAD_RX_TIMEOUT事件
(此事件结束接收ACK功能)
lastOper=>operation: 触发MAC_EVT_RADIO_TX_TIMEOUT事件
（此事件完成复位发送流程变量，停
止发送流程事件，并将发送结果发送
给网络层）

end=>end: 结束

start->oper1->cond1
cond1(yes)->oper3->cond3
cond1(no)->cond2

cond2(no)->oper2->cond1
cond2(yes)->lastOper

cond3(yes)->lastOper
cond3(no)->oper4->oper5->cond4

cond4(yes)->oper6->end
cond4(no)->oper7->lastOper

lastOper->end


```
