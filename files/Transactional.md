***Статья про прокси транзакций***:
[# Spring Data — Transactional Caveats](https://dev.to/kirekov/spring-data-transactional-caveats-19di)

[# Лучший способ использовать аннотацию Spring Transactional](https://habr.com/ru/companies/otus/articles/649093/)  

***Откат изменений дочерней транзакции, без отката родительской***:

В случае если, изменения в БД, могут быть некорректно обработаны, например может сработать constraint на БД, следует использовать дочернюю транзакцию

`@Transactional(propagation = Propagation.REQUIRES_NEW)`

Для игнорирования исключений. следует указать блок noRollbackFor

`@Transactional(
	propagation = Propagation.REQUIRES_NEW, 
	noRollbackFor = [RuntimeException::class]
	)`

Пример:
```kotlin
@Component  
class TransactionExample(  
    private val dataStoragePort1: DataStoragePort1,  
    private val dataStoragePort2: DataStoragePort2,  
) {  
  
    @Transactional    
    fun doInParentTransaction() {        
	    val data: String = dataStoragePort1.selectData()        
	    try {
		    dataStoragePort2.update(data)
		} catch (ex: Exception) { 
			logger.error (ex) { "log this error" }
			dataStoragePort1.updateData(data)         
		}
	}
}  
  
@Component  
class DataStoragePort1(private val repository: Repository) {  
  
    fun selectData(): String = repository.selectData()  
      
    fun updateData(data: String): String = repository.selectData()  
}  
  
@Component  
class DataStoragePort2(private val repository: Repository) {  
  
     /**  
     * Метод вызывается в рамках родительской транзакции 
     * и в случае ошибки - изменения НЕ будут закомичены в БД.     
     */    
     @Transactional(
     propagation = Propagation.REQUIRES_NEW, 
     noRollbackFor = [RuntimeException::class]
     )  
    fun update(data: String) {  
        repository.update(data)  
    }  
}
```

Для того, чтобы изменения транзакции были сразу применены к БД, а не после выполнения всего блока кода аннотированного транзакцией, следует использовать методы JPA/Hobernate:
*saveAndFlush*
*persistAndFlush*
Т.к. изменения к БД будет применены сразу, это позволит перехватывать исключения типа *DataIntegrityViolationException* , *ConstraintViolationException* и т.д.