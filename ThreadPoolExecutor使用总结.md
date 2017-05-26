---
title: ThreadPoolExecutor 使用总结
tags: ThreadPoolExecutor,生产者消费者
grammar_footnote: true
grammar_cjkEmphasis: true
grammar_cjkRuby: true
---

## 简介

ThreadPoolExecutor 是一个基于线程池的异步任务执行框架。内部基于线程池和任务队列实现了一个基于“任务”的生产者消费者模式，所有提交给ThreadPoolExecutor的任务将被异步的执行。


## 优点

 - 基于线程池，减少线程初始化开销，提高执行大量异步任务的性能。
 - 提供了线程池生命周期管理，方便控制任务开始与结束。
 - 提供了一些基本的统计，比如线程数量、已完成任务数量等。

## 技术模型

执行：ThreadPoolExecutor 内部维护了一个线程池(THREAD_POOL)，线程池中的线程来执行被提交的任务；如果线程池中的线程过于忙碌，来不及处提交过来的任务，任务将会被放入任务队列(QUEUE)等待被执行。
停止：停止时ThreadPoolExecutor不再接收新的任务，如果调用的是shutdown方法，会继续处理已接受但未处理的任务；如果调用的是shutdownNow方法，任务队列中等待执行的任务将被丢弃，执行中的任务将会收到中断信号。
QUEUE: 用来存放等待执行的任务。
THREAD_POOL: 线程池，任务执行的载体。

## 运行状态

 - RUNNING:  接收新任务，处理队列中的任务
 - SHUTDOWN: 不接收新任务，处理队列中的任务
 - STOP:     不接收新任务，不处理队列中的任务，并且中断（interrupt ）在处理中的任务。
 - TIDYING:  所有的任务已经terminated, workerCount 是0,  线程状态变为 TIDYING，将会执行 terminated() 钩子方法
 - TERMINATED: terminated() 钩子方法执行完毕

## 状态转换

 RUNNING -> SHUTDOWN:  执行 shutdown() 方法
 (RUNNING or SHUTDOWN) -> STOP： 执行 shutdownNow() 方法
 SHUTDOWN -> TIDYING: QUEUE 已空并且 THREAD_POOL中没有运行中的任务
 STOP -> TIDYING: THREAD_POOL中没有运行中的任务
 TIDYING -> TERMINATED: terminated()执行完毕(TIDYING状态后，将会执行terminated()钩子方法，当terminated()方法执行完毕，变为TERMINATED状态)

## 任务提交逻辑

![enter description here][1]

## shutdown 逻辑

## shutdownNow 逻辑

## 使用须知



## 参数配置


  [1]: ./images/threadpoolexecutor.png "threadpoolexecutor.png"
