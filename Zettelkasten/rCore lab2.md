---
dg-publish: false
---
2025-05-04
Status: #idea
Tags: [[OS Camp Stage 2]]

# rCore lab2
引入虚存机制后，需要重写sys_get_time和sys_trace函数实现，使其能够正确处理虚拟地址访问。

## 实现思路
在引入虚存机制后，用户程序和内核使用不同的地址空间，直接访问用户传入的指针会导致错误。因此需要将用户空间的虚拟地址转换为物理地址，再进行读写操作。

### sys_get_time 实现
对于sys_get_time，需要检查用户传入的指针是否有效，并将当前时间写入用户空间的结构体中。

```rust
pub fn sys_get_time(_ts: *mut TimeVal, _tz: usize) -> isize {
    if let Some(ts) = translated_to_mut::<TimeVal>(current_user_token(), _ts) {
        ...
    }
}
```
### sys_trace 实现
对于sys_trace，需要根据trace_request的值执行不同的操作，并且需要检查用户空间的地址是否合法可访问。

```rust
pub fn sys_trace(_trace_request: usize, _id: usize, _data: usize) -> isize {
    let pte = translated_pte(token, va);
    match _trace_request {
        0 => {
            // 读取操作
            if let Some(pte) = pte {
                // 检查pte标志位
                ...
                let addr = (ppn << 12) + offset;
                return (unsafe { (addr as *const u8).read_volatile() }) as isize;
            }
            ...
```

### sys_mmap实现
要点在于虚拟地址区域的检查
通过检查之后再创建映射,munmap同理
```rust
pub fn mmap(&self, start: usize, len: usize, prot: usize) -> isize {
    // 参数验证与对齐检查
    ...
    // 检查映射区域是否未被使用
    if !self.memory_set.is_all_unmapped(start_va.floor(), end_va.floor()) {
        return -1;
    }
	...
	// 解析prot
    let mut permission = MapPermission::U;
    // 创建映射
    self.memory_set.insert_framed_area(start_va, end_va, permission);
    0
}

pub fn munmap(&self, start: usize, len: usize) -> isize {
    // 参数验证与对齐检查
    ...
    // 检查要取消映射的区域是否以及被映射
    if !self.memory_set.is_all_mapped(s_vpn, e_vpn) {
        return -1;
    }
	// 删除映射
    self.memory_set.munmap(s_vpn, e_vpn);
    0
}
```



___
# References