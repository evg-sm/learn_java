### Полезные ссылки

[Разбор вопросов и ответов с собеседований на Java-разработчика. Часть 6](https://javarush.com/groups/posts/3341-razbor-voprosov-i-otvetov-s-sobesedovaniy-na-java-razrabotchika-chastjh-6)  


### Что такое hibernate  

### hibernate cache  


### Какие типы Query бывают  

### Типы связей между сущностями  
- OneToOne
- OneToMany
- ManyToOne
- ManyToMany

### Persistence context

### Проблема с утечкой соединений к БД в spring-data-jpa
[# Why is my DB connection leaking without @Transactional?](https://stackoverflow.com/questions/52337384/why-is-my-db-connection-leaking-without-transactional)

При использовании spring-data-jpa может возникать проблема с учеткой соединений к БД, из-за слишком долгих обращений к внешним ресурсам
##### Выводы:

1. Если вы используете `spring-boot-starter-web (Spring MVC) + spring-starter-data-jpa`, то у вас автоматически при старте конфигурируется `OpenEntityManagerInViewInterceptor` (`OpenSessionInViewInterceptor`) в `JpaWebConfiguration`. Он реализует паттерн `Open Session In View`. Суть его в том, что при старте клиентского запроса создаётся EntityManager и связывается с текущим потоком. При обращении к БД берётся connection из `ConnectionPool`, и возвращается назад в `ConnectionPool` только после завершения клиентского запроса. Независимо от наличия или отсутствия транзакций в приложении. Если клиентский запрос содержит долгоиграющие действия, то это может привести к исчерпанию свободных соединений в `ConnectionPool`. Для решения этой проблемы нужно в `application.properties` установить свойство: `spring.jpa.open-in-view=false`. Тогда соединение к БД будет браться из пула при начале транзакции и возвращаться в пул при её завершении.
    
2. Если вы не используете `spring-boot-starter-web (Spring MVC) + spring-starter-data-jpa` или свойство `spring.jpa.open-in-view=false`, то:
    
    - соединение к БД берётся из пула при обращении к БД и возвращается обратно при завершении метода обращения к БД. В **нетранзакционном** методе
    - если метод **транзакционный**, то соединение к БД берётся из пула при первом обращении к БД внутри транзакции и возвращается после завершения транзакции. Поэтому, если в тарнзакционном методе есть обращения к внешним ресурсам (вызов по REST внешнего сервиса), которые долго отвечают, то это может привести к исчерпанию пула соеднинений к БД