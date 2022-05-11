## Logger 컴포넌트를 위한 Lock Free 큐의 구현


https://stackoverflow.com/questions/8735800/implementing-a-lock-free-queue-for-a-logger-component

2012.01.04에 나와 비슷한 고민을 했던 누군가가 stackoverflow에 올린 글을 가져옴
- producer : consumer = N : 1
- how to implement lock free queue


> I am designing a new improved Logger component (.NET 3.5, C#). I would like to use a lock-free implementation. Logging events will be sent from (potentially) multiple threads, although only a single thread will do the actual output to file/other storage medium. 

> In essence, all the writers are * enqueuing* their data into some queue, to be retrieves by some other process (LogFileWriter). 

> Can this be achieved in a lock-less manner? i could not find a direct reference to this particular problem on the net.

채택된 답변의 내용은 Enqueue or Dequeue 에만 lock을 걸어 사용한다면 성능이 그리 떨어지지는 않을거라고 보고있음. 또한 lock free queue에 데이터가 채워지는 속도가 매우 빠른 경우라면 I/O 처리가 그 속도를 따라가지 못할것으로 언급하고있음(lock이 아닌 다른 부분에서 문제가 있을 가능성을 제기하는군😨)

> If you find that using a lock in this case is too slow, you have a much bigger problem. A lock, when it's not contended, takes about 75 nanoseconds on my system (2.0 GHz Core 2 Quad). When it's contended, of course, it's going to take somewhat longer. But since the lock is just protecting a call to Enqueue or Dequeue, it's unlikely that the total time for a log write will be much more than that 75 nanoseconds.

> If the lock is a problem--that is, if you find your threads lining up behind that lock and causing noticeable slowdowns in your application--then it's unlikely that making a lock-free queue is going to help much. Why? Because if you're really writing that much to the log, your lock-free blocking queue is going to fill up so fast you'll be limited to the speed of the I/O subsystem.

> I have a multi-threaded application that writes on the order of 200 log entries a second to a Queue<string> that's protected by a simple lock. I've never noticed any significant lock contention, and processing isn't slowed in the least bit. That 75 ns is dwarfed by the time it takes to do everything else.


---

https://qa.ostack.cn/qa/?qa=890294/
구글링하다보니 Boost의 lockfree queue 사용하는 경우가 STL queue를 사용하는 것보다 성능적으로 떨어졌다는 글이 있던데 한번 가지고 와봤다. (2021.10.24에 올라온 글이니 비교적 최신이다)

> Until now I was using std::queue in my project. I measured the average time which a specific operation on this queue requires. The times were measured on 2 machines: My local Ubuntu VM and a remote server. Using std::queue, the average was almost the same on both machines: ~750 microseconds.

> Then I "upgraded" the std::queue to boost::lockfree::spsc_queue, so I could get rid of the mutexes protecting the queue. On my local VM I could see a huge performance gain, the average is now on 200 microseconds. On the remote machine however, the average went up to 800 microseconds, which is slower than it was before.

> First I thought this might be because the remote machine might not support the lock-free implementation:

> From the Boost.Lockfree page: 
> Not all hardware supports the same set of atomic instructions. If it is not available in hardware, it can be emulated in software using guards. However this has the obvious drawback of losing the lock-free property.

> To find out if these instructions are supported, boost::lockfree::queue has a method called bool is_lock_free(void) const;. However, boost::lockfree::spsc_queue does not have a function like this, which, for me, implies that it does not rely on the hardware and that is is always lockfree - on any machine.

> What could be the reason for the performance loss?

빈번하게 사용되지 않는 경우라면 Lock free 알고리즘이 Lock 기반의 알고리즘보다 성능적으로 떨어 질수 있다는 답변을 확인할 수 있었다. lock을 사용하지 않음으로해서 발생할 수 있는 문제에 대해서 언급하고 있으며, lock에서 완전히 free해지려기보다는 경쟁(lock으로 보호된 메모리에 접근하고자 하는 쓰레들간의 경쟁)을 최소화할 수 있는 적절한 lock의 위치를 고려해보는게 바람직한 방식일 수 있다고 말하고 있다. 

> Lock free algorithms generally perform more poorly than lock-based algorithms. That's a key reason they're not used nearly as frequently.

> The problem with lock free algorithms is that they maximize contention by allowing contending threads to continue to contend. Locks avoid contention by de-scheduling contending threads. Lock free algorithms, to a first approximation, should only be used when it's not possible to de-schedule contending threads. That only rarely applies to application-level code.

> Let me give you a very extreme hypothetical. Imagine four threads are running on a typical, modern dual-core CPU. Threads A1 and A2 are manipulating collection A. Threads B1 and B2 are manipulating collection B.

> First, let's imagine the collection uses locks. That will mean that if threads A1 and A2 (or B1 and B2) try to run at the same time, one of them will get blocked by the lock. So, very quickly, one A thread and one B thread will be running. These threads will run very quickly and will not contend. Any time threads try to contend, the conflicting thread will get de-scheduled. Yay.

> Now, imagine the collection uses no locks. Now, threads A1 and A2 can run at the same time. This will cause constant contention. Cache lines for the collection will ping-pong between the two cores. Inter-core buses may get saturated. Performance will be awful.

> Again, this is highly exaggerated. But you get the idea. You want to avoid contention, not suffer through as much of it as possible.

> However, now run this thought experiment again where A1 and A2 are the only threads on the entire system. Now, the lock free collection is probably better (though you may find that it's better just to have one thread in that case!).

> Almost every programmer goes through a phase where they think that locks are bad and avoiding locks makes code go faster. Eventually, they realize that it's contention that makes things slow and locks, used correctly, minimize contention.