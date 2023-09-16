### SPRING FRAMEWORK ######

### Полезные ссылки

Создание bean не во время поднятия контекста а когда идет обращение в bean  
[Lazy Initialization in Spring Boot](https://www.baeldung.com/spring-boot-lazy-initialization)  

[Spring-потрошитель: жизненный цикл Spring Framework](https://vk.com/@javatutorial-spring-potroshitel-zhiznennyi-cikl-spring-framework)  
[Proxying mechanisms](https://docs.spring.io/spring-framework/docs/3.0.0.M3/reference/html/ch08s06.html)  
[Injecting Prototype Beans into a Singleton](https://www.baeldung.com/spring-inject-prototype-bean-into-singleton)  
[@Lookup Annotation in Spring](https://www.baeldung.com/spring-lookup)  
[Разница между @Component, @Service, @Repository и @Controller?](https://itsobes.ru/JavaSobes/kakie-otlichiia-mezhdu-component-service-repository-i-controller/)  

### Что такое classpath

Classpath - набор путей java к классам.

### Что такое bean Scope
- singleton (по умолчанию) как и паттерн, создается в единственном экземпляре, все запросы вернут один и тот же объект
- prototype каждое обращение к бину будет отдавать новый экземпляр объекта
- request создает экземпляр бина для каждого HTTP запроса
- session создает экземпляр бина для каждого HTTP сессии
- application создает экземпляр бина для жизненного цикла ServletContext
- websocket создает экземпляр бина для каждого websocket сессии

### JDK dynamic proxies or CGLIB
Если целевой объект для проксирования реализует хотя бы один интерфейс, то будет использоваться JDK dynamic proxy.
Все интерфейсы, реализованные целевым типом, будут проксированы.
Если целевой объект не реализует никаких интерфейсов, будет создан CGLIB proxy.

### Инверсия управления (Inversion of Control)   
Это принцип, при котором фреймворк вызывает пользовательский код. Это отличается от случая с библиотеками, потому что пользовательский код вызывает код библиотеки.  

### Внедрение зависимостей (Dependency Injection)  
Это шаблон проектирования, в котором объект получает другие объекты, от которых он зависит. Это отделяет создание объектов от их использования.

### IoC Контейнер (IoC Container)  
Это реализация IoC и DI. Контейнер IoC создает и управляет bean-компонентами на основе мета-информации. Он также может решать и предоставлять зависимости для создаваемых им объектов.

### ClassPathBeanDefinitionScanner  
Сканер bean definition'ов, который определят кандидаты в bean в classpath. И регистрирует bean в BeanFactory или ApplicationContext Ищет кандидаты по аннотациям  @Component, @Repository, @Service, or @Controller

### BeanDefinition  
BeanDefinition описывает экземпляр компонента, который имеет значения свойств, значения аргументов конструктора и дополнительную информацию, предоставляемую конкретными реализациями.

### BeanFactory  

**_BeanFactory_** - Корневой интерфейс для доступа к контейнеру Spring.
Ядро ApplicationContext.
Загружает BeanDefinitions из источника конфигурации, например, из XML-файла, Java Configuration, Groovy Configuration...
И непосредственно создает bean'ы.
Все имплементации должны поддерживать lifecycle интерфейсы.
Полный список методов инициализации:

- BeanNameAware's setBeanName
- BeanClassLoaderAware's setBeanClassLoader
- BeanFactoryAware's setBeanFactory
- EnvironmentAware's setEnvironment
- EmbeddedValueResolverAware's setEmbeddedValueResolver
- ResourceLoaderAware's setResourceLoader (only applicable when running in an application context)
- ApplicationEventPublisherAware's setApplicationEventPublisher (only applicable when running in an application context)
- MessageSourceAware's setMessageSource (only applicable when running in an application context)
- ApplicationContextAware's setApplicationContext (only applicable when running in an application context)
- ServletContextAware's setServletContext (only applicable when running in a web application context)
- postProcessBeforeInitialization methods of BeanPostProcessors
- InitializingBean's afterPropertiesSet
- Сustom init-method definition
- postProcessAfterInitialization methods of BeanPostProcessors  

При закрытии фабрики компонентов применяются следующие методы жизненного цикла:

- postProcessBeforeDestruction методы DestructionAwareBeanPostProcessors
- Уничтожение DisposableBean
- Пользовательское определение метода уничтожения

### BeanFactoryPostProcessor 
Хук BeanFactory, который позволяет вносить изменения в BeanDefinition.
На момент работы BeanDefinition'ы уже загружены в контекст, но еще не проинициализированы (не созданы экземпляры bean'ов).
Это позволяет переопределять или добавлять поля в классы.

Содержит единственный метод:
- **_postProcessBeanFactory_**

**_Например аннотации @Value обрабатываются PlaceholderConfigurerSupport._**  

### ApplicationContext  

Центральный интерфейс Spring Context, дает возможности:
- BeanFactory обеспечивает доступ к компонентам приложения
- ResourceLoader обеспечивает доступ к загрузке ресурсов (файлов)
- ApplicationEventPublisher возможность публикации событий
- MessageSource возможность интернационализации
- Автоматическая регистрация BeanPostProcessor и BeanFactoryPostProcessor  

### BeanPostProcessor 
Хук BeanFactory позволяет кастомно модифицировать bean'ы, до и после инициализации.

Содержит два метода:
- **_postProcessBeforeInitialization_**
- **_postProcessAfterInitialization_**

Как правило, постпроцессоры которые заполняют bean-компоненты через интерфейсы маркеров и т.п., будут реализовывать postProcessBeforeInitialization,
в то время как постпроцессоры, которые обертывают bean'ы прокси, обычно реализуют postProcessAfterInitialization.
ApplicationContext может автоматически определять bean'ы BeanPostProcessor в BeanDefinition
и применять эти постпроцессоры к любым впоследствии создаваемым bean'ам.
Обычная BeanFactory позволяет программно регистрировать постпроцессоры, применяя их ко всем bean-компонентам, созданным с помощью bean factory.  

### @PostConstruct
Фактически init метод
Работает между сразу после метода BeanPostProcessor.postProcessBeforeInitialization()
После отрабатывает  BeanPostProcessor.postProcessAfterInitialization()

### Трехфазовый конструктор  
1. Java конструктор
2. @PostConstruct за который отвечает BeanPostProcessor
3. @AfterProxy после создания проксей спринга за него отвечает ContextListener  

### ApplicationListener  
Интерфейс, который позволяет обрабатывать ApplicationEvent события. Можно использовать аннотацию @EventListener, вместо интерфейса.  

### Lifecycle  
Интерфейс похожий на ApplicationListener, но в нем определено 2 метода, которые срабатывают во время запуск (start) и остановку (stop) контекста.  

### SmartLifcycle  
Это расширение Lifecycle интерфейса. Отличие в том, что он срабатывает во время обновление (refresh) и закрытия (close) контекста.  

### Жизненный цикл контекста Spring-а
Жизненный цикл контекста состоит из 4-ёх этапов:

- Этап обновления (refresh) - автоматический
- Этап запуска (start) - вызывается методом ApplicationContext#start
- Этап остановки (stop) - вызывается методом ApplicationContext#stop
- Этап закрытия (close) - автоматический

### Этап обновления контекста
1. BeanFactory создает BeanFactoryPostProcessor-ы используя конструктор без аргументов
2. ApplicationContext вызывает метод BeanFactoryPostProcessor#postProcessBeanFactory
3. BeanFactory создает BeanPostProcessor-ы  

### Этап запуска контекста
1. ApplicationContext проверяет флаг Lifecycle#isRunning и вызывает метод Lifecycle#start, если флаг имеет значение false
2. ApplicationContext проверяет флаг SmartLifecycle#isRunning и вызывает метод SmartLifecycle#start, если флаг имеет значение false. Да-да, контекст второй раз проходиться по объектам реализующие интерфейс SmartLifecycle
3. ApplicationContext публикует ContextStartedEvent
4. Методы обратного вызова, помеченные аннотацией @EventListener с типом параметра метода ContextStartedEvent, обрабатывают это событие. Также здесь может быть ApplicationListener

### Этап остановки контекста
1. ApplicationContext проверяет флаг SmartLifecycle#isRunning и вызывает метод SmartLifecycle#stop, если флаг имеет значение true
2. ApplicationContext проверяет флаг Lifecycle#isRunning и вызывает метод Lifecycle#stop, если флаг имеет значение true
3. ApplicationContext публикует ContextStoppedEvent
4. Методы обратного вызова, помеченные аннотацией @EventListener с типом параметра метода ContextStoppedEvent, обрабатывают это событие. Также здесь может быть ApplicationListener

### Этап закрытия контекста
1. ApplicationContext публикует ContextClosedEvent
2. Методы обратного вызова, помеченные аннотацией @EventListener с типом параметра метода ContextClosedEvent, обрабатывают это событие. Также здесь может быть ApplicationListener
3. ApplicationContext проверяет флаг SmartLifecycle#isRunning и вызывает метод SmartLifecycle#stop, если флаг имеет значение true

### Жизненный цикл bean-компонента
Жизненный цикл bean-компонента состоит из 2-ух этапов:

- Этап инициализации
- Этап уничтожения

### Этап инициализации bean-компонента
1. BeanFactory создает bean-компонент
2. Срабатывает статический блок инициализации
3. Срабатывает не статический блок инициализации
4. Внедрение зависимостей на основе конструктора
5. Внедрение зависимостей на основе setter-ов
6. Отрабатывают методы стандартного набора *Aware интерфейсов
7. BeanPostProcessor#postProcessBeforeInitialization обрабатывает bean-компонент
8. InitDestroyAnnotationBeanPostProcessor#postProcessBeforeInitialization вызывает методы обратного вызова, помеченные аннотацией @PostConstruct
9. BeanFactory вызывает метод InitializingBean#afterPropertiesSet
10. BeanFactory вызывает метод обратного вызова, зарегистрированный как initMethod.
11. BeanPostProcessor#postProcessAfterInitialization обрабатывает bean-компонент

### Коротко
1. BeanFactory создает bean-компонент
2. Статическая и нестатическая инициализация
3. Внедрение зависимостей конструктора и setter-ов
4. Обработка *Aware интерфейсов
5. BeanPostProcessor#postProcessBeforeInitialization
6. @PostConstruct
7. BeanPostProcessor#postProcessAfterInitialization

### Этап уничтожения bean-компонента
Этап уничтожения срабатывает только для singleton bean-компонентов, так как только эти компоненты храниться в BeanFactory.

1. InitDestroyAnnotationBeanPostProcessor.postProcessBeforeDestruction вызывает методы обратного вызова, отмеченные как@PreDestroy
2. BeanFactory вызывает метод InitializingBean#destroy
3. BeanFactory вызывает метод обратного вызова, зарегистрированный как destroyMethod

### Дополнения к жизненному циклу

**Ordered** - интерфейс, позволяющий управлять порядком работы компонентов. Например, если компоненты реализуют BeanPostProcessor/BeanFactoryPostProcessor и Ordered интерфейсы, то мы можем контролировать порядок их выполнения.

**FactoryBean** - интерфейс, позволяющий внедрить сложную логику создания объекта. Если у вас есть сложный код инициализации, который лучше выражается на Java, вы можете создать свой собственный FactoryBean, написать сложную инициализацию внутри этого класса, а затем подключить свой собственный FactoryBean к контейнеру.

**ApplicationStartup** - это инструмент, который помогает понять, на что тратится время на этапе запуска.