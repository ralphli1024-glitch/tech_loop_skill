---
name: tests
description: 优质测试与劣质测试参考
author:  
---

# 优质测试与劣质测试

## 优质测试
通过公共 API 测试可观测行为，不关心内部结构。

```java
@Test
void user_can_checkout_with_valid_cart() throws Exception {
    Cart cart = createCart(); cart.add(product);
    CheckoutResult result = checkout(cart, paymentMethod);
    assertThat(result.getStatus()).isEqualTo("confirmed");
}
```

要点：测试**做什么**、仅用公共 API、每个测试一个断言、重构时依然有效。

## 劣质测试
与实现强耦合：mock 内部对象、测私有方法、断言调用次数/顺序。

```java
@Test
void checkout_calls_paymentService_process() throws Exception {
    PaymentService mockPayment = Mockito.mock(PaymentService.class);
    checkout(cart, payment);
    Mockito.verify(mockPayment).process(cart.getTotal());
}
```

识别信号：重构但行为不变时测试却失败（坏味道）。