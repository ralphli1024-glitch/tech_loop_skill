---
name: design-an-interface
description: 接口方案设计，适用于API设计、接口和方法设计、接口方案选型、模块架构比对，或是用户要求多重方案设计的场景。
author:   
---

# 接口设计方案制定

## 触发示例
- "帮我设计一个XX模块的接口"
- "这个功能应该怎么设计API"
- "给出几种不同的接口方案"

## 执行流程

### 1. 梳理需求
- 模块解决什么业务问题
- 调用方（内部模块/外部使用/测试）
- 核心业务操作
- 硬性约束（性能/兼容性/编码规范）
- 内部封装与对外接口边界

### 2. 多方案并行
启动至少3个子智能体，产出风格完全不同的方案。
每个智能体提示：模块说明 + 业务需求 + 专属约束（方案一精简接口/方案二最大化扩展/方案三贴合高频场景/方案四参考指定范式）

输出格式（表格或章节）：
- 接口定义结构（数据类型、方法）
- 调用示例
- 内部封装逻辑
- 方案利弊

### 3. 展示方案
逐条呈现，每条包含：接口定义、调用示例、内部封装。

### 4. 方案对比
从接口简洁度、适用范围、性能、模块深度、容错性分析差异。

### 5. 敲定方案
选最贴合业务场景的方案，或融合多方案优势。

### 6. 约束
- 遵循项目RESTful规范
- 遵守命名约定和RULE.md

## 规避误区
- 各方案必须大幅区分，不许同质
- 必须做方案对比
- 仅设计接口架构，不写实现代码
- 不参考开发工作量评判

## 可测试性设计
```java
// 可测试：接收依赖
public Discount calculateDiscount(Cart cart) {}
// 难测：内部创建依赖
public void applyDiscount(Cart cart) { cart.setTotal(cart.getTotal() - discount); }

// 可测试：返回结果
public OrderResult processOrder(Order order, PaymentGateway gateway) {}
// 难测：内部创建 + 副作用
public void processOrder(Order order) { StripeGateway gw = new StripeGateway(); }
```

设计时思考：能否减少方法、简化参数、封装复杂逻辑到模块内部？