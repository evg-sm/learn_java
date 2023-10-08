### Полезные ссылки

[Baeldung - Dockerizing a Spring Boot Application](https://www.baeldung.com/dockerizing-spring-boot-application)  
[Baeldung - Introduction to Docker Compose](https://www.baeldung.com/ops/docker-compose)  
[Three Ways to Create Docker Images for Java](https://codefresh.io/blog/create-docker-images-for-java/)  
[Docker Swarm vs Kubernetes: выбираем фаворита](https://www.corpsoft24.ru/about/blog/docker-swarm-vs-kubernetes-vybiraem-favorita/#:~:text=Docker%20Swarm%20%E2%80%94%20%D1%8D%D1%82%D0%BE%20%D0%BF%D0%BB%D0%B0%D1%82%D1%84%D0%BE%D1%80%D0%BC%D0%B0%20%D0%BE%D1%80%D0%BA%D0%B5%D1%81%D1%82%D1%80%D0%BE%D0%B2%D0%BA%D0%B8,%D1%81%D0%BB%D1%83%D0%B6%D0%B1%D1%8B%20%D0%B8%20%D0%B7%D0%B0%D0%B4%D0%B0%D1%87%D0%B8%2C%20%D0%B1%D0%B0%D0%BB%D0%B0%D0%BD%D1%81%D0%B8%D1%80%D0%BE%D0%B2%D1%89%D0%B8%D0%BA%D0%B8%20%D0%BD%D0%B0%D0%B3%D1%80%D1%83%D0%B7%D0%BA%D0%B8.)  


### Зачем нужен docker file?
Docker может автоматически создавать образы, читая инструкции из файла Dockerfile. 
Dockerfile — это текстовый документ, содержащий все команды, 
которые пользователь может вызвать в командной строке для сборки образа.


### Зачем нужен docker compose?  
Docker Compose — это инструмент, который помогает определять и совместно использовать многоконтейнерные приложения. 
С помощью Docker Compose можно создать файл YAML для определения сервисов и с помощью одной команды
поднимать или опускать контейнеры.

**_Вот некоторые возможности docker compose:_**  
- Поднять один или несколько образов из одного compose файла (services).
- Сборка образа из dockerfile или из git'a (build).
- Конфигурирование взаимодействия контейнеров по сети (networking).
- Конфигурирование томов, общего или выделенного пространства на дисках (volumes).
- Конфигурирование зависимостей контейнеров друг от друга.
- Конфигурирование переменных контейнеров.
- Масштабирование и реплики. Возможность поднимать сразу несколько контейнеров с одним сервисом.


### Как сделать docker образ с jar файлом?

**_Базовый сценарий:_**  
1. Создаем SpringBoot приложение.
2. Создаем Dockerfile, например такой  
    ```dockerfile
    # use jdk with very small linux distro
    FROM openjdk:17-jdk-slim
    # copy the packaged jar file into our docker image
    COPY build/libs/message-service-0.0.1.jar message-service.jar
    # set the startup command to execute the jar
    ENTRYPOINT ["java", "-jar", "/message-service.jar"]
    ```  
3. Запускаем команду консоли для создания образа ```docker build --tag=message-server:1.0 .```  
В результате работы команды формируется образ docker image message-server:1.0
4. Запускаем образ командой ```docker run -p 8080:8080 message-server:1.0```.
В результате подниментся новый контейнер с message-server.

**_Опционально:_**  
Можем делегировать докеру сборку проекта:  
    
```dockerfile
# select parent image
FROM maven:3.6.3-jdk-8
# copy the source tree and the pom.xml to our new container
COPY ./ ./
# package our application code
RUN mvn clean package
# set the startup command to execute the jar
CMD ["java", "-jar", "target/demo-0.0.1-SNAPSHOT.jar"]
```  

### Зачем нужен docker swarm?  
Docker Swarm — это платформа оркестровки контейнеров с открытым исходным кодом, созданная и поддерживаемая компанией Docker. 
Под капотом Docker Swarm преобразует несколько экземпляров контейнеров в один виртуальный хост. 
Кластер платформы обычно содержит три элемента: ноды, службы и задачи, балансировщики нагрузки. 
Некий простой аналог Kubernetes.
