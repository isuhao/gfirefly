#gfirefly
firefly-gevent 是firefly的gevent版本。相比现在的firefly版本使用的twisted，gevent更加的精简。
gevent就是一个基于coroutine的python网络开发框架。协程是一种并发模型，但不同于thread和callback，它的所有task都是可以在一个线程里面执行，然后可以通过在一个task里面主动放弃执行来切换到另一个task执行，它的调度是程序级的，不像thread是系统级的调度。
Gevent最明显的特征就是它惊人的性能，尤其是当与传统线程解决方案对比的时候。在这一点上，当负载超过一定程度的时候，异步I/O的性能会大大的优于基于独立线程的同步I/O这几乎是常识了。同时Gevent提供了看上去非常像传统的基于线程模型编程的接口，但是在隐藏在下面做的是异步I/O。更妙的是，它使得这一切透明。（此处意思是你可以不用关心其如何实现，Gevent会自动帮你转换）
忽略其他因素，Gevent性能是线程方案的4倍左右（在这个测试中对比的是Paste，译者注：这是Python另一个基于线程的网络库）
与单进程多线程模型相比，多进程和协程是更加Scalable的模型。在高并发场景下，采用多进程模型编制的程序更加容易Scale Out，而协程模型可以使单机的并发性能大幅提升，达到Scale Up的目的。所以，未来服务器端并发模型的标配估计会是：每个核一个进程，每个进程是用协程实现的微线程。
在编码方面，多线程模型带来的共享资源加解锁的问题一直是程序员的梦魇。而用多进程模型编程时，会自然鼓励程序员写出避免共享资源的程序，从而提高扩展性。而Python目前的协程实现都为非抢占式调度，程序员自行控制协程切换时机，因此也可以避免绝大多数令人头疼的加解锁问题。这些都利于写出更稳定的代码。
另外，和同样具有很好并发性能的事件驱动模型相比，用协程实现的微线程，在逻辑表达上非常友好和直白，无须在不知道什么时候会发生的event和一层套一层的callback中纠结和扭曲（正如Twisted其名）。对于写过多线程程序的程序员而言，协程带来的微线程模型几乎可以实现无痛提高并发性能。
firefly-gevent结合了gevent的性能，封装了网络IO处理、数据库IO读写缓存、分布式进程间接口调用。这样使得游戏服务端的开发变得更加的轻松简单，开发者不必在面对这些的技术难题，专心致力于游戏玩法逻辑的开发。