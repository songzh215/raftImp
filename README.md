# raftImp
try to implement raft protocal.

1. 归纳raft协议中各个Follwer,Leader,candidate三个角色分别做的事情以及状态机转换。着手用C++实现raft协议。

## 跟随者(Follower)
* 响应来自Leader的请求 TODO
* 响应来自Candidate的请求 TODO
* 超过选举超时时间的情况之前都没有收到领导人的心跳，或者是候选人请求投票的，就自己变成候选人 TODO
