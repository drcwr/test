PBFT是Practical Byzantine Fault Tolerance的缩写，即：实用拜占庭容错算法。该算法是Miguel Castro（卡斯特罗）和Barbara Liskov（利斯科夫）在1999年提出来的，解决了原始拜占庭容错算法效率不高的问题，算法的时间复杂度是O(n^2)，使得在实际系统应用中可以解决拜占庭容错问题。该论文发表在1999年的操作系统设计与实现国际会议上（OSDI99）。其中Barbara Liskov就是提出了著名的里氏替换原则（LSP）的人，2008年图灵奖得主。


request
c -> p

pre-prepare
p check

prepare
i check

commit
p+i


reply
p+i


https://blog.csdn.net/jfkidear/article/details/81275974
