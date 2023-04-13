---
title: Spring 问题集
date: 2023-04-06 14:49:25
categories:
tags:
---
## Spring 答疑

### Spring如何解决循环依赖问题

使用 @Lazy 注解 设置某一个 service 延迟加载就可以了.
```
@Component
public class Subscriber implements MessageListener {
@Lazy
@Autowired
private VerificationSchemeService schemeService;
}
```

### service 多个实现类问题

```
@Service("PayService")
PaymentServiceImpl implements  PayService{}
@Service("WxPayService")
WxServiceImpl implements  PayService{}

  @Autowired
    @Qualifier("WxPayService")
    PaymentService paymentService;

```
