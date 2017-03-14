# raftImp
try to implement raft protocal.

归纳raft协议中各个Follwer,Leader,candidate三个角色分别做的事情以及状态机转换。着手用C++实现raft协议。

## 跟随者(Follower)
* 任何节点在初始状态下为Follower状态。
* 响应来自Leader的请求 TODO
1.  更新时间戳。（时间戳为最近一次leader,candidate的心跳消息）
* 响应来自Candidate的请求 TODO
1.  更新时间戳。
2.  fklsdkflasdf
* 超过选举超时时间的情况之前都没有收到领导人的心跳，或者是候选人请求投票的，就自己变成候选人 TODO
1.  更新自己的term。
* 响应来自客户端的请求，
1.  将请求重定向至Leader。

## 领导者(Leader)
* 周期性的向所有跟随者发送心跳包（不包含日志项内容的附加日志项 RPCs）

## 所有节点：
* 当前任期号比其他人小，那么他会更新自己的编号到较大的编号值。
* 候选人或者领导者发现自己的任期号过期了，那么他会立即恢复成跟随者状态。
* 一个节点接收到一个包含过期的任期号的请求，那么他会直接拒绝这个请求。


（RequestVote） RPCs 由候选人在选举期间发起
附加条目（AppendEntries）RPCs 由领导人发起，用来复制日志和提供一种心跳机制。
候选人的选举呢？
服务器之间传输快照增加了第三种 RPC。
