# 何时使用模拟（Mock）
仅在**系统边界**处模拟：
- 外部 API（支付、邮件等）
- 数据库（优先用测试数据库）
- 时间/随机数
- 文件系统

不要模拟：你自己的类、内部协作对象、任何由你控制的对象。

## 可模拟性设计

**1. 依赖注入**：外部依赖通过构造参数传入，不在内部创建。

```java
// 可模拟
public PaymentProcessor(PaymentClient client) { this.client = client; }
// 难模拟
public PaymentProcessor() { this.client = new StripeClient(key); }
```

**2. SDK 风格接口**：每个外部操作一个独立函数，而非通用获取器。

```java
// GOOD：独立函数 → 各自可独立 Mock
public interface ApiClient { User getUser(Long id); List<Order> getOrders(Long userId); }
// BAD：条件判断 → Mock 需内部处理
public interface ApiClient { <T> T fetch(String endpoint, RequestOptions opts); }
```