### Что такое корутина?

**Корутина** — это обобщённая форма подпрограммы, которая, в отличие от обычных функций, может приостанавливать своё выполнение в произвольных точках и позже возобновлять его, сохраняя при этом своё внутреннее состояние. Это позволяет корутинам обмениваться данными и управлять потоком выполнения программы более гибко, чем обычные функции.

### Основные характеристики корутины:

1. **Приостановка и возобновление**:
    
    - Корутина может приостановить своё выполнение (например, с помощью ключевого слова `yield` в Python или `suspend` в Kotlin) и позже продолжить с того места, где остановилась.
2. **Сохранение состояния**:
    
    - При приостановке корутина сохраняет своё состояние (переменные, стек и т.д.), чтобы при возобновлении она могла продолжить работу, как будто ничего не произошло.
3. **Многопоточность без потоков**:
    
    - Корутины позволяют писать асинхронный код, который выглядит как синхронный, без необходимости создания отдельных потоков. Это делает их более лёгкими и эффективными по сравнению с потоками.
4. **Кооперативная многозадачность**:
    
    - Корутины управляют собой сами: они сами решают, когда приостановиться и когда продолжить выполнение. Это отличается от прерывания выполнения потоков операционной системой.


![Иллюстрация](images/sequrntalVsConcurrent.png)

### Пример последовательного выполнения кода:
```kotlin
fun main() {  
    milkCows()  
    feedChickens()  
}  
  
fun milkCows() {  
    var cow = 0  
    println("Milking cow #${++cow}")  
    println("Milking cow #${++cow}")  
    println("Milking cow #${++cow}")  
    println("Milking cow #${++cow}")  
}  
  
fun feedChickens() {  
    var chicken = 0  
    println("Feeding chicken #${++chicken}")  
    println("Feeding chicken #${++chicken}")  
    println("Feeding chicken #${++chicken}")  
    println("Feeding chicken #${++chicken}")  
}
```

![Иллюстрация](images/Concurrent.png)
### Пример выполнения кода разных задач на одном потоке:
Коротины могут выполнять разные задачи на одном потоке, переключаясь между задачами, в этом преимущество куритин - выполнение нескольких задач 'паралелльно'

```kotlin
import kotlin.coroutines.Continuation  
import kotlin.coroutines.EmptyCoroutineContext  
import kotlin.coroutines.intrinsics.COROUTINE_SUSPENDED  
import kotlin.coroutines.intrinsics.createCoroutineUnintercepted  
import kotlin.coroutines.resume  
import kotlin.coroutines.suspendCoroutine  
  
  
fun main() {  
    var cow = 0  
    log { "Milking cow #${++cow}" }  
    feedChickens.resume()  
    log { "Milking cow #${++cow}" }  
    feedChickens.resume()  
    log { "Milking cow #${++cow}" }  
    feedChickens.resume()  
    log { "Milking cow #${++cow}" }  
    feedChickens.resume()  
}  
  
val feedChickens = suspend {  
    var chicken = 0  
    log { "Feeding chicken #${++chicken}" }; yield()  
    log { "Feeding chicken #${++chicken}" }; yield()  
    log { "Feeding chicken #${++chicken}" }; yield()  
    log { "Feeding chicken #${++chicken}" }; complete()  
}.createCoroutineUnintercepted(Continuation(EmptyCoroutineContext) {})  
  
fun Continuation<Unit>.resume(): Unit = resume(Unit)  
  
suspend inline fun yield() = suspendCoroutine<Unit> { COROUTINE_SUSPENDED }  
  
suspend inline fun complete() = suspendCoroutine { it.resume(Unit) }  
  
fun log(message: () -> String) {  
    println("${Thread.currentThread().name}: ${message()}")  
}
```



