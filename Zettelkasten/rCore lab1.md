---
dg-publish: false
---
2025-05-04
Status: #idea
Tags: [[OS Camp Stage 2]]

# rCore lab1
需要实现sys_trace来记录任务的系统调用次数

## 数据结构
新增了TaskTrace
```rust
#[derive(Clone, Copy)]
pub struct TaskTrace {
    pub syscall_map: [(usize,usize); MAX_SYSCALL_NUM],
    pub syscall_count: usize,
}
```
### api
```rust
// src/task/mod.rs

/// 计数syscall
pub fn record_task_trace(syscall_id: usize);
/// 获取syscall调用次数
pub fn get_task_trace(syscall_id: usize) -> usize;
```
在trap_handler中调用record_task_trace即可

## syscall_trace
```rust
pub fn sys_trace(_trace_request: usize, _id: usize, _data: usize) -> isize {
    trace!("kernel: sys_trace");
    match _trace_request {
        0 => unsafe {
            (_id as *const u8).read_volatile() as isize
        },
        1 => {
            let ptr = _id as *mut u8;
            unsafe { 
                ptr.write_volatile(_data as u8); 
            };
            0
        },
        2 => {
            get_task_trace(_id) as isize
        },
        _ => {
            error!("kernel: sys_trace: invalid trace request {}", _trace_request);	
            -1
        }
    }
}
```


___
# References