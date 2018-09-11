title:  Java多线程
date: 2018年7月28日23:33:26
categories: 多线程
tags: 

	 - Synchronized
	 - ReentrantLock
	 - Volitale
	 - Semaphore
	 - CountDownLatch
	 - CyclicBarrier
cover_picture: /images/JavaThread.jpg
---

### Synchronized 

- 内置锁，互斥性。
- JVM关键字，修饰符。
- 内置条件队列操作接口Object.waie()、notify()、notifyAll()。

ReentrantLock

- 默认非公平锁。

- 内置锁类似功能。

- 公平锁：等待时间最长的会最先被唤醒获取锁 。

- 重入锁：线程可以重复获取**已经持有**的锁。

- Synchronized中，所有的线程都在同一个object的条件队列上等待。而ReentrantLock中，每个condition都维护了一个条件队列。

-  每一个**Lock**可以有任意数据的**Condition**对象，**Condition**是与**Lock**绑定的，所以就有**Lock**的公平性特性：如果是公平锁，线程为按照FIFO的顺序从*Condition.await*中释放，如果是非公平锁，那么后续的锁竞争就不保证FIFO顺序了。

- Condition接口定义的方法，**await**对应于**Object.wait**，**signal**对应于**Object.notify**，**signalAll**对应于**Object.notifyAll**。特别说明的是Condition的接口改变名称就是为了避免与Object中的*wait/notify/notifyAll*的语义和使用上混淆。

  ```java
  /**
  * ReentrantLock 的使用
  */
  public class Queue<T> {
      private final T[] mItems;
      private final Lock mLock = new ReentrantLock();
      private Condition mNotFull = mLock.newCondition();
      private Condition mNotEmpty = mLock.newCondition();
      private int head, tail, count;
  
      public Queue(int maxSize) {
          mItems = (T[]) new Object[maxSize];
      }
  
      public Queue() {
          this(10);
      }
  
      public void put(T t) throws InterruptedException {
          mLock.lock();//获取锁
          try {
              while (count == mItems.length) {
                  //数组满时，线程进入等待队列挂起。线程被唤醒时，从这里返回。
                  mNotFull.await();
              }
              mItems[tail] = t;
              if (++tail == mItems.length) {
                  tail = 0;
              }
              ++count;
              mNotEmpty.signal();
          } finally {
              mLock.unlock();
          }
      }
  
      public T take() throws InterruptedException {
          mLock.lock();
          try {
              while (count == 0) {
                  mNotEmpty.await();
              }
              T o = mItems[head];
              mItems[head] = null;//GC
              if (++head == mItems.length) {
                  head = 0;
              }
              --count;
              mNotFull.signal();
              return o;
          } finally {
              mLock.unlock();
          }
      }
  
  }
  ```

  

### Volatile

- 内存可见性。
- 放置指令重排。

### Semaphore 

- 信号量，跟锁机制存在一定的相似性。
- Semaphore也是一种锁机制。
- ReentrantLock是只允许一个线程获得锁。
- Semaphore允许多个线程同时获得执行许可。

```java
 private static void semaphore() {
        final Semaphore semaphore = new Semaphore(5);
        for (int i = 0; i < 20; i++) {
            Thread thread = new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        semaphore.acquire();
                        Thread.sleep(3000);
                        semaphore.release();
                        System.err.println(Thread.currentThread().getName() + ":" + new Date());
                    } catch (InterruptedException e) {
                    }
                }
            });
            thread.setName("Thread:" + i);
            thread.start();
        }
    }
```

### CountDownLatch 

- 一个线程等其它一个或多个线程。

- 计数器实现，一个线程完成任务计数器减1.

  ```java
   private static void countDownLatch() {
          final CountDownLatch countDownLatch = new CountDownLatch(5);
          for (int i = 0; i < 5; i++) {
              Thread thread = new Thread(new Runnable() {
                  @Override
                  public void run() {
                      try {
                          Thread.sleep(2000);
                          System.out.println(Thread.currentThread()
                                  .getName() + ":" + System.currentTimeMillis());
                          countDownLatch.countDown();
                      } catch (InterruptedException e) {
                          System.out.println(e.toString());
                      }
  
                  }
              });
              thread.setName("Thread:--:" + i);
              thread.start();
          }
  
          Thread thread1 = new Thread(new Runnable() {
              @Override
              public void run() {
                  try {
                      System.err.println(countDownLatch.getCount() + "start---------" + System
                              .currentTimeMillis());
                      countDownLatch.await();
                      System.err.println("end-----------" + countDownLatch.getCount());
                  } catch (InterruptedException e) {
                      System.err.println(e.toString());
                  }
  
              }
          });
          thread1.setName("Main Thread:");
          thread1.start();}
  ```

  ### CyclicBarrier

  - 所有线程必须同时到达栅栏位置才能继续执行下一步操作。

  - 可以循环使用。

    ```java
     private static void cyclicBarrier() {
           final CyclicBarrier barrier = new CyclicBarrier(5, new Runnable() {
                @Override
                public void run() {
                    System.out.println("All Over");
                }
            });
            
            for (int i = 0; i < 5; i++) {
                Thread thread = new Thread(new Runnable() {
                    @Override
                    public void run() {
                        try {
                            System.out.println(Thread.currentThread().getName() + "over");
                            barrier.await();
                        } catch (Exception e) {
                            System.out.println(e.toString());
                        }
                    }
                });
                thread.setName("Thread:" + i);
                thread.start();
            }
        }
    
    ```