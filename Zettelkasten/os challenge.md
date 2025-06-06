---
dg-publish: true
---
2025-05-04
Status: #idea
Tags: [[OS Camp Stage 3]]

# 挑战题目
## 分析
### 题目优化点
#### 需要dealloc的
- align不为1的
- base为2^i的(i为奇数且i>=5)
- 由于delta为`[0..256)`,所以base小于256时无法区别是否需要dealloc,故base小于256时一律dealloc处理
#### 不需要dealloc的
- base为2^i的(i为偶数且i>=6)
### 数据分离
当ByteAllocator的alloc操作返回内存不足时,GlobalAllocator会尝试分配页,分配的大小是ByteAllocator的total_bytes与当前
可以尝试维护两个内存管理器

#### 偶数内存管理器(不需要dealloc)
由于不需要dealloc,每次alloc申请时判断是否需要dealloc,不需要则通过当前链表的next指针为申请的大小分配,如果内存不够用则进入下一个页
##### 申请内存
申请内存是按页为单位申请的,申请到内存后可以查看一下起始地址是否和已有的页表最大地址相同,相同则合并为一个大块

#### 奇数内存管理器(需要dealloc)
这里的内存会在每一轮完成之后释放,需要的空间要小一些
由于会出现:![[Pasted image 20250505185510.png]]
所以该管理器不是只存奇数内存,不过align8的内存每一轮也会全部清空,所以不用担心

### 实现
#### [[challenge lab1_3_tlfs|3个Tlsf]]/2个Tlsf
![[Pasted image 20250505185855.png]]
删去align8,用2个Tlsf效果是一样的
瓶颈应该在align8上,因为奇数内存每轮都会被清理掉,但是align8貌似不是

#### Bump+Buddy/Slab
![[Pasted image 20250505203552.png]]
两个差不多都是350左右,
瓶颈可能也是在align8吧

### 未解决的问题
align8的问题没有解决,按理说每一轮应该都会释放的,但我debug的时候发现有几轮没有释放

___
# References