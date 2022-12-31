### Код для исследования
```java

public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}

```



### Выполнение
При запуске программы сначала происходит подгрузка классов с помощью *ClassLoader*. В данном коде подгружаются: **JvmComprehension**, **String**, **Object**, **Integer**, **System**. При этом **String**, **Object**, **Integer**, **System** - являются частью **JDK** и подгружаются в первую очередь.
Далее происходит связывание *Linking*, а именно:

 1.Verify проверка валидности и синтаксической консистентности;
 
 2.Prepare подготовка примитивов в статических классах (в данном коде проходит без реальной проверки);
 
 3.Опускается Resolve разрешение символьных ссылок. 
 
 Следующим этапом является *Inititalization* - выполняются инициализаторы static полей и методов - т.е. printAll и main.
Данные, которые были получены с помощью предыдущего этапа попадают в *Metaspace*, *Stack Memory* и *Heap*.
В момент вызова метода main создается фрейм в *Stack'е*.

//1 Во фрейме main создается переменная i со значением 1.

//2 В Heap создается объект Object и во фрейме main создается переменная o, которой присваивается ссылка на этот объект.

//3 В Heap создается объект Integer со значением 2, а во фрейме main появляется переменная ii со ссылкой на этот объект.

//4 В Stack'е создается фрейм printAll и в нем записываются переменные Object o, int i и Integer ii.

//7 В Stack'е создается фрейм println, которому передается ссылка на созданный в Heap объект String со значением "finished".

В ходе работы программы отрабатывает Garbage Collector. 

//5 В Heap создается объект Integer со значением 700,а во фрейме printAll появляется переменная uselessVar со ссылкой на этот объект.

// 6 В Stack'е создается фрейм println, куда передаются ссылки на Object o, int i и Integer ii. В Stack'е создается фрейм toString.
