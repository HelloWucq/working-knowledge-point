#一.对象池[https://blog.csdn.net/liuxiao723846/article/details/78881040](https://blog.csdn.net/liuxiao723846/article/details/78881040)
- 对于那些被频繁使用的对象，在使用完后，不立即将它们释放，而是将它们缓存起来，以供后续的应用程序重复使用，从而减少创建对象和释放对象的次数，进而改善应用程序的性能。事实上，由于对象池技术将对象限制在一定的数量，也有效地减少了应用程序内存上的开销。
##1.1.核心原理：缓存和共享

#二.Java和操作系统交互细节[https://my.oschina.net/u/4106059/blog/3029071](https://my.oschina.net/u/4106059/blog/3029071)