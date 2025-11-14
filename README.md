order-service
│
├── domain                               ← Pure business logic (NO Spring, NO DB, NO HTTP)
│   ├── model                             ← Domain models (Entities, Value Objects)
│   │     Order.java                      ← Domain entity representing an order (business rules inside)
│   │     Product.java                    ← Domain entity representing a product
│   │     Money.java                      ← Value object for currency-safe calculations
│   │
│   ├── service                           ← Domain services (complex business logic)
│   │     PriceCalculator.java            ← Calculates final price using domain rules
│   │     DiscountPolicy.java             ← Applies discount logic / business policies
│   │
│   ├── port                              ← Domain ports (interfaces)
│   │   ├── in                            ← Inbound ports (use case definitions)
│   │   │     CreateOrderUseCase.java     ← Interface describing “create order” use case
│   │   │     CancelOrderUseCase.java     ← Interface describing “cancel order” use case
│   │   │
│   │   └── out                           ← Outbound ports (domain dependencies)
│   │         PaymentPort.java            ← Domain interface for payment external system
│   │         InventoryPort.java          ← Domain interface for inventory external system
│   │         OrderRepositoryPort.java    ← Domain interface for saving/fetching orders
│   │
│   └── dto                               ← Internal DTOs / Commands (immutable inputs for use cases)
│         CreateOrderCommand.java         ← Data needed to execute create order use case
│         CancelOrderCommand.java         ← Data needed to execute cancel order use case
│
│
├── application                           ← Application layer (use case implementations)
│   └── service
│         CreateOrderService.java         ← Implements CreateOrderUseCase; orchestrates domain + ports
│         CancelOrderService.java         ← Implements CancelOrderUseCase; orchestrates domain + ports
│
│
├── adapter                               ← Technical adapters (inbound & outbound)
│   ├── in                                ← Inbound adapters (entrypoints)
│   │   └── web                           ← REST controllers
│   │         OrderController.java        ← Handles HTTP requests; calls inbound ports
│   │
│   └── out                               ← Outbound adapters (implement external integrations)
│       ├── payment
│       │     PaymentRestAdapter.java     ← Implements PaymentPort; bridges domain → Feign/WebClient
│       │     PaymentClient.java          ← Feign/WebClient client for calling payment service
│       │
│       ├── inventory
│       │     InventoryRestAdapter.java   ← Implements InventoryPort; bridges domain → external API
│       │     InventoryClient.java        ← Feign/WebClient client for calling inventory service
│       │
│       └── persistence                   ← Persistence adapters
│             OrderRepositoryAdapter.java ← Implements OrderRepositoryPort; maps domain ↔ JPA
│             JpaOrderEntity.java         ← JPA entity mapped to database table
│             SpringDataOrderRepository.java ← Spring Data JPA repository interface
│
│
└── infrastructure                        ← Framework configurations & app-level setup
      ├── config
      │     FeignConfig.java              ← Configures Feign clients
      │     KafkaConfig.java              ← Kafka listener/producer configuration (if used)
      │     PersistenceConfig.java        ← Database/configuration beans
      │
      └── application.yml                 ← Application properties (ports, DB config, service URLs)
