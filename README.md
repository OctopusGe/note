## 优秀工程师的方法论
1. 故障现象

2. 导致原因

3. 解决方案

4. 优化建议（同样的错误不犯第二次）

## 代码案例：
```
// 开启30个线程往list里面添加数据
public static void main(String[] args) {
    List<String> list = new ArrayList<>();
    for (int i = 1; i <= 30; i++) {
       new Thread(() -> {
          list.add(UUID.randomUUID().toString().substring(0, 8));
          System.out.println(list);
      }, String.valueOf(i)).start();
   }
}
```

1. 故障现象
   <br>&nbsp;&nbsp;&nbsp;&nbsp; **java.util.ConcurrentModificationException** <br>
2. 导致原因
   <br>&nbsp;&nbsp;&nbsp;&nbsp;并发争抢修改导致，参考花名册签名情况：一个人正在写入，另一个人过来抢夺，导致数据不一致异常。并发修改异常。
   <br>
3. 解决方案
   <br>① `new Vector<>();`
   <br>② `Collections.synchronizedList(new ArrayList<>());`
   <br>③ `new CopyOnWriteArrayList<>();`<br>
4. 优化建议
  <br>写时复制
  <br>&nbsp;&nbsp;&nbsp;&nbsp;**CopyOnWrite** 容器即写时复制的容器，往一个容器添加元素的时候，不直接往当前容器 **Object[]** 添加，而是先将当前容器 **Object[]** 进行 **copy**，复制出一个新的容器`Object[] newElements`，然后往新的容器`Object[] newElements`里添加元素，添加完元素之后，再将原容器的引用指向新的容器`setArray(newElements)`.
  <br>&nbsp;&nbsp;&nbsp;&nbsp;这样做的好处是可以对 **CopyOnWrite** 容器进行并发的读，而不需要加锁，因为当前容器不会添加任何元素。所以 **CopyOnWrite** 容器也是一种读写分离的思想，读和写不同的容器。
