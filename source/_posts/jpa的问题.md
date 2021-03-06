---
title: jpa 的 lazy 加载问题
date: 2022-01-07 18:12:55
tags:
  - Java
  - jpa
  - Springboot
  - eager
  - lazy
categories:
  - [技术]
---

## 问题

- **在同一个 `session` 中**，`LAZY` 模式下，并不会将标记为懒加载的元素取出，只有在显式调用 `.get()` 方法时，才会取出。
- 在 `EAGER` 模式下，则会一股脑取出，但是如果取出了一个非常大的 `List`，而我们并不需要这个属性，这里就会有很严重的性能问题

## 参考

- [difference-between-fetchtype-lazy-and-eager-in-java-persistence-api](https://stackoverflow.com/questions/2990799/difference-between-fetchtype-lazy-and-eager-in-java-persistence-api#:~:text=Suppose%20you%20are%20using%20Spring)
