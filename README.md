# raftImp
try to implement raft protocal.

归纳raft协议中各个Follwer,Leader,candidate三个角色分别做的事情以及状态机转换。着手用C++实现raft协议。

## 所有节点：
* 任期单调增长。
* 一个节点接收到一个包含过期的任期号的请求，那么他会直接拒绝这个请求。
* 会对同一个任期内的投票响应一次。有其他投票则忽略。
## 跟随者(Follower)
* 任何节点在初始状态下为Follower状态。
* 响应来自Leader的请求 
    1.  更新时间戳。（时间戳为最近一次leader,candidate的心跳消息）
    2.  对比term，term更新则更新本地term。
* 响应来自Candidate的请求 
    1.  更新时间戳。
    2.  比较term，投票（具体细节后续补充）
* 超过选举超时时间的情况之前都没有收到领导人的心跳，或者是候选人请求投票的，就自己变成候选人（具体见候选者的行为）
    2.  设置状态为Candidate状态。
    
* 响应来自客户端的请求，
    1.  将请求重定向至Leader。

## 领导者(Leader)
* 周期性的向所有跟随者发送心跳包（不包含日志项内容的附加日志项 RPCs）
* term过期，状态转为Follower。更新自己的term。

## 候选者(Candidate)
* 接收到领导的心跳，如果term过期（Leader的term >= Candidate的term），状态转为Follower。更新自己的term。


* 接收到其他候选人的投票请求，
* 发起投票
    1.  更新自己的term。
    2.  自己的投票数+1。（自己给自己投票），记录当前投票的任期。
    3.  发送RequestVote 给集群中其他的所有服务器。返回值的处理：
        1.  依次遍历，检查投票结果，是否为任期内结果，是 + 1。直到选票数目 > 1/2 * 节点数，状态转为Leader状态。发送心跳信息（其中附带term）。
        2.  不满足选票过半，则等待一个超时时间，重新投票。
（RequestVote） RPCs 由候选人在选举期间发起
附加条目（AppendEntries）RPCs 由领导人发起，用来复制日志和提供一种心跳机制。
服务器之间传输快照增加了第三种 RPC。
