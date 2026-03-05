

![Иллюстрация](images/Hexagonal.png)

 Компоненты

  1. Domain Layer (domain/)

  - Чистые бизнес-модели без зависимостей от фреймворков
  - Value objects: Amount, Currency, Provider, Subscription
  - Агрегаты и доменные сервисы

  2. Application Layer (application/)

  - port/ — интерфейсы (ports) для взаимодействия с доменом
  - service/ — бизнес-сервисы (use cases)
  - configuration/ — Spring конфигурация

  3. Adapter Layer (adapter/)

  - input (driving adapters) — контроллеры, MQ listeners
  - output (driven adapters) — репозитории, внешние API клиенты

input adapter/port точки входа в бизнес-логику приложения с инскапуляцией всей бизнес-логики и интеграцией с внешними сервисами 

input adapter: Controller/Listener/Scheduler

business layer implements input ports 
business layer использует output ports 
business layer могут быть вспомогательные сервисы, которые не имлементируют input ports, но могут быть использованы в бизнес-логике