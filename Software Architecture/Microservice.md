# SOA

- 标准化的服务契约 Standardized service contract

- 服务的松耦合 Service loose coupling 

- 服务的抽象 Service abstraction 

- 服务的可重用性 Service reusability 

- 服务的自治性 Service autonomy 

- 服务的无状态性 Service statelessness 

- 服务的可发现性 Service discoverability 

- 服务的可组合性 Service composability

# Domain Driven Design

## Rich / Anemic Domain Model

- Rich Domain Model - Domain Driven
  - Object contains both logic and data
  - Object Oriented
  - Easy to understand and easy for testing
  - More suitable with OLTP (Online Transaction Processing), complex business system
- Anemic Domain Model - Data Driven
  - Database Busincess Object (BO) only have getter and setter and have simple function logic
  - Process Oriented
  - Most business logic are simple and history makes ADM more popular
  - More suitable for OLAP (Online analytical processing), analysis system