#### **_Exceptions:_**  

Все исключения наследники классов Object -> Throwable.  
**_Подразделяются на два типа:_**  

* **_Error_** — это критическая ошибка во время исполнения программы, связанная с работой виртуальной машины Java. 
В большинстве случаев Error не нужно обрабатывать, поскольку она свидетельствует о каких-то серьезных недоработках в коде.  
* **_Exception_** - это, собственно, исключения: исключительная, незапланированная ситуация, которая произошла при работе программы.


Исключения (Exception), в свою очередь, делятся на **_проверяемые_** (checked) и не **_проверяемые_** (unchecked).  
**_Проверяемые исключения обязательно надо перехватывать в коде, непроверяемые - нет._**

#### **_Иерархия:_**

Throwable

- ERROR
  - StackOverflowError
  - NoSuchMethodError

- Exception
  - Checked - Обязательно надо перехватывать
    - InterruptedException
    - IOException
    - SQLException
  - Unchecked - Не обязательно перехватывать, хотя и можно
    - NullPointerException
    - NumberFormatException
    - IndexOutOfBoundsException
    - ArithmeticException  

![Иерархия](images/exception.jpg)