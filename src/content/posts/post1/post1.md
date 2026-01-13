---
title: 对Python异步编程的实践理解
published: 2026-01-14
description: 基于Asyncio与FastAPI框架的Python异步消息队列处理实践
image: ''
tags: ["Python", "异步编程"]
category: '技术心得'
draft: false 
lang: 'zh'
---

在处理高并发的网络请求或 IO 密集型任务时，传统的同步阻塞模型往往会成为性能瓶颈。本文简要记录我对 Python `asyncio` 异步编程模型的理解。

## 核心概念

- **同步 (Synchronous)**：任务按顺序执行。如果某个任务涉及网络请求或磁盘 IO，CPU 将进入等待状态，直到操作完成，期间无法处理其他任务。
- **异步 (Asynchronous)**：利用 **事件循环 (Event Loop)** 机制。当任务进入 IO 等待时，主动挂起并释放 CPU 控制权，由循环调度执行其他就绪任务。

## 关键语法

Python 3.5+ 引入了 `async/await` 关键字，使异步代码的编写更符合直觉：

```python
import asyncio

async def fetch_data(id: int):
    print(f"开始抓取数据: {id}")
    await asyncio.sleep(1) 
    return f"数据 {id}"

async def main():
    tasks = [fetch_data(i) for i in range(3)]
    results = await asyncio.gather(*tasks)
    print(f"执行结果: {results}")

if __name__ == "__main__":
    asyncio.run(main())
```
## 实践总结
非阻塞原则：在异步函数中，避免使用 time.sleep() 等同步阻塞函数，否则会阻塞整个事件循环。

适用场景：异步编程最适合 IO 密集型（网络、文件、数据库）场景；对于 CPU 密集型（复杂计算）任务，多进程模型通常是更好的选择。