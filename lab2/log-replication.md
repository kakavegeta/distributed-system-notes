### Tips

Given the process, log replication is more complex than leader election. There are a lot of traps and if you do not notice you might lost for hours... Anyway, it passed~ 

I have to record one stupid bug... For a moment, I always got both success and failure from AE reply. As result, when I sent AE args... Just see code:

```go
func (rf *Raft) sendHeartbeat () {
    //......
    // when sent AE args to peers
    rf.mu.Lock()
    nextIndex := rf.NextIndex[peer]
    prevLogIndex := nextIndex-1
    prevLogTerm := 0
    if prevLogIndex > 0 {
        // Damn it...
        //prevLogIndex = rf.Logs[prevLogIndex].Term
        prevLogTerm = rf.Logs[prevLogIndex].Term
    }
    //......
}
```

When coding labs, the only suggestion is: FOLLOWING FIGURE 2!

#### Majority 

A leader decides to commit an entry only when having replicated it on a majority of servers.  One mistake I made is `count` initialization in function `sendHeartbeat()`... Another fool bug...

#### Followers append entries, inconsistency

#### Leader update NextIndex and CommitIndex, inconsistency

#### Transfer ApplyCh to info tester an entry is commited



#### Test2B: rejoin disconnected leader

I failed this test initially. After printing all server's logs, I found that the when the old leader is reconnected, with state as FOLLOWER, it still send AE to other followers. Then I figure out why: I do not immediately send heartbeat(AE) after a server become leader but sleep 50 millisecond, which is advised by HINTs to reach agreement. As result, the follower will append the old leader's entries, which causes bug. I fix this bug by sleep after sent heartbeat(AE), and current I do know exactly why... I guess it is the way I manage timer. In the sleep interval, a lot of things can happen, and when old leader is reconnected... em... I just go back to read paper... 

#### Test2B: Leader backtracking logs optimization

Follow the instruction from TA.

