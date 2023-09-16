#### **_Exceptions:_**  

Все исключения наследника класса Throwable.  
Исключения делятся на проверяемые (checked) и не проверяемые (uncheked).  
Проверяемые исключения обязательно надо перехватывать в коде, непроверяемые - нет.

#### **_Иерархия:_**

Throwable

- ERROR
  - StackOverflowError
  - NoSuchMethodError

- Exception
  - Checked - Обязательно надо перехватывать
  - InterruptedException
  - IOException
  - Unchecked - Не обязательно перехватывать, хотя и можно
  - NullPounterException
  - NumberFormatException
  - IndexOutOfBoundsException
  - ArithmeticException  

![Иерархия](/images/exception.jpg)