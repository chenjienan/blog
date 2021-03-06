---
title: OOD 学习笔记
date: 2019-04-18 21:37:56
tags:
category: design
---

## 目的: 设计一个能正常运行,并容易维护的系统

### 没有标准答案

- OOA (analysis): 讨论, 分析, 获得需求
- OOD (Design): 搭建框架
- OOP (Programming): 写代码

### 坑

- 先手
  - 先下手为强: 不要先入为主. 需要通过和面试官有效交流
  - 指鹿为马: 问清题目要求

- 解题
  - 朝出夕改: 先给出一个可行的结果, 忌讳修修改改

- 收手
  - 虎头蛇尾: 检查设计是否能支持所需功能

### 5C 解题法

1. Clarify: 除去题中歧义, 确定答题范围
   - What
     - 取关键字(名词)
     - 考虑属性(不需要考虑与题目不相干的属性)
   - How
     - 确定规则,明确方向
     - 从简单方案开始
   - Who
2. Core Objects: 确定题目涉及的类, 以及之间的关系
3. Cases: 确定题目需要实现的场景和功能
4. Classes: 通过UML图, 填充题目涉及的类
5. Correctness: 检查设计, 是否满足关键点