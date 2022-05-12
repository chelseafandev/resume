## Logger 컴포넌트를 위한 Lock Free 큐의 구현

[**Implementing a lock-free queue (for a Logger component)**](https://stackoverflow.com/questions/8735800/implementing-a-lock-free-queue-for-a-logger-component)

2012.01.04에 나와 비슷한 고민을 했던 누군가가 stackoverflow에 올린 질문이다.
- producer : consumer = N : 1
- how to implement lock free queue

> I am designing a new improved Logger component (.NET 3.5, C#). I would like to use a lock-free implementation. Logging events will be sent from (potentially) multiple threads, although only a single thread will do the actual output to file/other storage medium. <br><br>In essence, all the writers are * enqueuing* their data into some queue, to be retrieves by some other process (LogFileWriter).<br><br>Can this be achieved in a lock-less manner? i could not find a direct reference to this particular problem on the net.

<br>

채택된 답변의 내용은 `Enqueue or Dequeue` 에만 lock을 걸어 사용(enq deq 둘 다 필요하지 않나?)한다면 성능이 그리 떨어지지는 않을거라고 보고있으며, lock free queue에 데이터가 채워지는 속도가 매우 빠른 경우라면 I/O 처리가 그 속도를 따라가지 못할것이기때문에 lock free의 도입이 큰 도움이 되지는 않을거라 언급했다.(lock이 아니라 다른 부분에서 훨씬 큰 문제가 있을 가능성을 제기함)

> If you find that using a lock in this case is too slow, you have a much bigger problem. A lock, when it's not contended, takes about 75 nanoseconds on my system (2.0 GHz Core 2 Quad). When it's contended, of course, it's going to take somewhat longer. But since the lock is just protecting a call to Enqueue or Dequeue, it's unlikely that the total time for a log write will be much more than that 75 nanoseconds.<br><br>If the lock is a problem--that is, if you find your threads lining up behind that lock and causing noticeable slowdowns in your application--then it's unlikely that making a lock-free queue is going to help much. Why? Because if you're really writing that much to the log, your lock-free blocking queue is going to fill up so fast you'll be limited to the speed of the I/O subsystem.<br><br>I have a multi-threaded application that writes on the order of 200 log entries a second to a Queue<`string`> that's protected by a simple lock. I've never noticed any significant lock contention, and processing isn't slowed in the least bit. That 75 ns is dwarfed by the time it takes to do everything else.

<br>

---

[**Thread safety for STL queue**](https://stackoverflow.com/questions/4029448/thread-safety-for-stl-queue)

위 질문에서는 STL queue를 멀티쓰레드 환경에서 guarantee하기 위해 `queue swap` 방식 사용을 가이드하고 있다. `queue swap`방식은 lock를 효율적으로 사용하기 위한 방법으로 2개를 큐를 사용하여 하나의 큐에는 `writer`가 계속해서 push하고 `reader`가 큐에 접근하여 데이터를 pop하려고 할때 2개의 큐를 **swap**하는 방식이다.

> As others have already mentioned, standard containers are not required to guarantee thread safety so what you're asking for cannot be implemented portably. You can reduce the time your reader thread is locking the writers out by using 2 queues and a queue pointer that indicates the queue that is currently in use by the writers.<br><br>Each writer would:
> - Acquire lock
> - Push element(s) into the queue currently pointed to by the queue pointer
> - Release lock
> 
> The reader can then do the following:
> - Acquire lock
> - Switch queue pointer to point to the second queue
> - Release lock
> - Process elements from the first queue

<br>

---

[**c++ - Using Boost.Lockfree queue is slower than using mutexes**](https://qa.ostack.cn/qa/?qa=890294/)

구글링을 하다 Boost의 lock-free queue가 STL queue보다 성능적으로 떨어졌다는 포스팅을 발견했다.(2021.10.24에 올라온 글이니 비교적 최근임)

> Until now I was using std::queue in my project. I measured the average time which a specific operation on this queue requires. The times were measured on 2 machines: My local Ubuntu VM and a remote server. Using std::queue, the average was almost the same on both machines: ~750 microseconds.<br><br>Then I "upgraded" the std::queue to boost::lockfree::spsc_queue, so I could get rid of the mutexes protecting the queue. On my local VM I could see a huge performance gain, the average is now on 200 microseconds. On the remote machine however, the average went up to 800 microseconds, which is slower than it was before.<br><br>First I thought this might be because the remote machine might not support the lock-free implementation:<br><br> From the Boost.Lockfree page: 
> > Not all hardware supports the same set of atomic instructions. If it is not available in hardware, it can be emulated in software using guards. However this has the obvious drawback of losing the lock-free property.
> 
> To find out if these instructions are supported, boost::lockfree::queue has a method called bool is_lock_free(void) const;. However, boost::lockfree::spsc_queue does not have a function like this, which, for me, implies that it does not rely on the hardware and that is is always lockfree - on any machine.<br><br>What could be the reason for the performance loss?

<br>

queue가 빈번하게 사용되지 않는 경우라면 Lock free 알고리즘이 Lock 기반의 알고리즘보다 성능적으로 떨어질 수 있다는 답변을 확인할 수 있었다. 해당 답변에서는 lock을 사용하지 않음으로써 발생할 수 있는 문제에 대해서 언급하고 있으며, lock에서 완전히 해방되려고 하는 것 보다는 경쟁(lock으로 보호된 메모리에 접근하고자 하는 쓰레들간의 경쟁)을 최소화할 수 있는 적절한 lock의 위치를 고민하는 게 바람직한 방식일 수 있다고 말하고 있다.

> Lock free algorithms generally perform more poorly than lock-based algorithms. That's a key reason they're not used nearly as frequently.<br><br>The problem with lock free algorithms is that they maximize contention by allowing contending threads to continue to contend. Locks avoid contention by de-scheduling contending threads. Lock free algorithms, to a first approximation, should only be used when it's not possible to de-schedule contending threads. That only rarely applies to application-level code.<br><br>Let me give you a very extreme hypothetical. Imagine four threads are running on a typical, modern dual-core CPU. Threads A1 and A2 are manipulating collection A. Threads B1 and B2 are manipulating collection B.<br><br>First, let's imagine the collection uses locks. That will mean that if threads A1 and A2 (or B1 and B2) try to run at the same time, one of them will get blocked by the lock. So, very quickly, one A thread and one B thread will be running. These threads will run very quickly and will not contend. Any time threads try to contend, the conflicting thread will get de-scheduled. Yay.<br><br>Now, imagine the collection uses no locks. Now, threads A1 and A2 can run at the same time. This will cause constant contention. Cache lines for the collection will ping-pong between the two cores. Inter-core buses may get saturated. Performance will be awful.<br><br>Again, this is highly exaggerated. But you get the idea. You want to avoid contention, not suffer through as much of it as possible.<br><br>However, now run this thought experiment again where A1 and A2 are the only threads on the entire system. Now, the lock free collection is probably better (though you may find that it's better just to have one thread in that case!).<br><br>Almost every programmer goes through a phase where they think that locks are bad and avoiding locks makes code go faster. Eventually, they realize that it's contention that makes things slow and locks, used correctly, minimize contention.