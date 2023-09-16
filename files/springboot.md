### SPRING BOOT ######

### Как создать свой starter?  
- Созадть класс автоконфигурации
- Создать дескриптор - META-INF/spring.factories, и в нем указать путь до нашей автоконфигурации

### Как работает Spring Boot и его стартеры?  

Во-первых, благодаря spring-boot-starter-parent, у которого родителем является spring-boot-dependencies,  
можно особо не париться о зависимостях и их версиях — большинство версии того,  
что может потребоваться прописано и согласовано в dependencyManagement родительского pom.   
Или можно заимпортировать BOM.

### Что под капотом @SpringBootApplication  

@EnableAutoConfiguration, @ComponentScan
