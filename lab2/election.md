### Tips

#### State

Simply and strictly follow figure 2 to make state transition.  

#### Lock

Just lock everywhere you modify or reach shared data, in this case, variables of `rf` , but remember to  follow lock ->unlock ->relock ->reunlock....  Follow the [rules](https://pdos.csail.mit.edu/6.824/labs/raft-locking.txt) Prof provided

#### Election timer loop

`rf.runElectionTimer` is the main long running loop. In this loop, if time elapsed > timeout, `rf` instance will run `rf.startElection()`, and in `rf.startElection()` if no leader is elected, simply run `rf.runElectionTimer()` in a new goroutine.  In this way, if `rf.runElectionTimer()` will continues until a leader is selected. 

#### Reset Timer

Given the paper described, there are only three cases you need to/must reset timer.  a) start a new election; b) received AppendEntries from current leader, and c) grant a vote to peer. Reset is simple, `rf.electionTimer = time.Now()`

#### Randomized timeout

The core to avoid vote split. 

#### Do not use time.Timer()

Simply employ time.Sleep() instead o time.Timer(). 

