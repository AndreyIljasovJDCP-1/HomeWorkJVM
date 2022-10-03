![picture](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ7_umj5vNG9nZJhWF4L6xr8wrAWjEaKmHaMg&usqp=CAU)
![picture](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRUJyG8AqZnqex_ZT3daHPyKO_yf7QGwopoFg&usqp=CAU)

### Домашнее задание по теме «JVM. Организация памяти, сборщики мусора, VisualVM»
##### Задача: описать каждую строку кода для исследованияс точки зрения происходящего в JVM.

### 1. Компиляция. 
![picture](https://user-images.githubusercontent.com/109555411/193608737-33c7752b-0be2-42d8-9645-811efd658029.png)

### 2. Загрузка классов.
![picture](https://user-images.githubusercontent.com/109555411/193609410-bf33f90d-f864-4f62-8f71-092e3df53581.png)

* **Подгрузка (Loading).**

* **Cвязывание(Linking).** Подготовка классов к выполнению:
> проверка, что код валиден  
подготовка примитивов в статических полях  
связывание ссылок на другие классы  

* **Инициализация (Initialization) классов:**
> выполняются инициализаторы static и инициализаторы полей static

### 3. Runtime Data Areas.

![classloader](https://user-images.githubusercontent.com/109555411/193610708-32bb125c-9542-4ab5-a9b2-53b439d66f27.png)

![memoryDataArea](https://user-images.githubusercontent.com/109555411/193615346-e36116d3-0ab0-4a43-b414-c9420cf64170.png)

#### Выполнение кода по строкам. Интерпретатор интерпретирует байт-код в файле .class строку за строкой, а затем выполняет:  

1. int i = 1 -> Переменная int i и ее значение сохраняются во фрейме Main().  
В момент вызова метода создаётся фрейм Main() в стеке.  
2. Object o = new Object() -> Сссылка на объект о во фрейме Main(), сам объект в Heap.
3. Integer ii = 2-> Сссылка на объект ii во фрейме Main(), сам объект в Heap.  
4. printAll(o, i, ii). В момент вызова метода создаётся новый фрейм printAll() в стеке.  
Во фрейм printAll() сохраняются параметры: ссылки на o и ii, и копия i;
5. Integer uselessVar = 700 -> Сссылка на объект о во фрейме printAll(), сам объект в Heap.
6. System.out.println(o.toString() + i + ii).  
В момент вызова метода создаётся новый фрейм в стеке System.out.println, куда передаются параметры ссылки на o и ii, и копия i.  
Дополнительно создается фрейм toString(), куда параметром передается ссылка на о.  
По мере выполнения методов, они удаляются из стека, то есть первым освободит место в стеке метод toString,  
потом System.out.println(),затем printAll().
7. System.out.println("finished"). Фрейм, параметры, освобождение места в стеке.

### 4. Garbage Collection: сборка мусора.
![garbage](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTP_s1ClZlM0xteYtDT3vGaHq0tgmKR-_Jr7Q&usqp=CAU)
  > Подключается периодически во время работы программы.  
  > Недостижимые объекты удаляются, достижимые объекты группируются по времени жизни (поколения).  
Чем дольше объект живёт, тем реже он проверяется, нужно ли его удалить.  
В нашем случае, первым на удаление будет Integer uselessVar, так как после завершения метода PrintAll на него нет ссылки.


