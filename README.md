# Milestone1
Milestone1 documents

order-service
│
├── domain
│   ├── model
│   │     Order.java
│   │     Product.java
│   │     Money.java
│   │
│   ├── service
│   │     PriceCalculator.java
│   │     DiscountPolicy.java
│   │
│   ├── port
│   │   ├── in
│   │   │     CreateOrderUseCase.java
│   │   │     CancelOrderUseCase.java
│   │   │
│   │   └── out
│   │         PaymentPort.java
│   │         InventoryPort.java
│   │         OrderRepositoryPort.java
│   │
│   └── dto
│         CreateOrderCommand.java
│         CancelOrderCommand.java
│
│
├── application
│   └── service
│         CreateOrderService.java         ← implements CreateOrderUseCase
│         CancelOrderService.java         ← implements CancelOrderUseCase
│
│
├── adapter
│   ├── in
│   │   └── web
│   │         OrderController.java
│   │
│   └── out
│       ├── payment
│       │     PaymentRestAdapter.java     ← implements PaymentPort
│       │     PaymentClient.java          ← Feign/WebClient client
│       │
│       ├── inventory
│       │     InventoryRestAdapter.java   ← implements InventoryPort
│       │     InventoryClient.java        ← Feign/WebClient client
│       │
│       └── persistence
│             OrderRepositoryAdapter.java ← implements OrderRepositoryPort
│             JpaOrderEntity.java
│             SpringDataOrderRepository.java
│
│
└── infrastructure
      ├── config
      │     FeignConfig.java
      │     KafkaConfig.java
      │     PersistenceConfig.java
      │
      └── application.yml
