## PACKET_MMAP에 대한 고찰

## Why use PACKET_MMAP
In Linux 2.4/2.6/3.x if PACKET_MMAP is not enabled, the capture process is very
inefficient. It uses very limited buffers and requires one system call to
capture each packet, it requires two if you want to get packet's timestamp
(like libpcap always does).

In the other hand PACKET_MMAP is very efficient. PACKET_MMAP provides a size 
configurable circular buffer mapped in user space that can be used to either
send or receive packets. This way reading packets just needs to wait for them,
most of the time there is no need to issue a single system call. Concerning
transmission, multiple packets can be sent through one system call to get the
highest bandwidth. By using a shared buffer between the kernel and the user
also has the benefit of minimizing packet copies.

It's fine to use PACKET_MMAP to improve the performance of the capture and
transmission process, but it isn't everything. At least, if you are capturing
at high speeds (this is relative to the cpu speed), you should check if the
device driver of your network interface card supports some sort of interrupt
load mitigation or (even better) if it supports NAPI, also make sure it is
enabled. For transmission, check the MTU (Maximum Transmission Unit) used and
supported by devices of your network. CPU IRQ pinning of your network interface
card can also be an advantage.

**reference**
* https://www.kernel.org/doc/Documentation/networking/packet_mmap.txt
