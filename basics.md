[Главная](README.md)

# Basics
+ [Установка Groovy](#Установка-Groovy)
+ [SDK](#SDK)
+ [groovyc](#groovyc)
+ [groovysh](#groovysh)
+ [Groovy Console](#Groovy-Console)
+ [Импорт по умолчанию в Groovy](#Импорт-по-умолчанию-в-Groovy)
+ [Ключевые слова](#Ключевые-слова)
+ [Комментарии](#Комментарии)
+ [Assertions](#Assertions)
+ [Class и модификаторы](#Class-и-модификаторы)
+ [Numbers](#Numbers)
+ [if else while for each switch](#if-else-while-for-each-switch)
+ [Аннотации](#Аннотации)
+ [Операторы](#Операторы)
+ [Grape](#Grape)



## Установка Groovy
Качаем отсюда стабильная версия на данный момент 4.+
https://groovy.apache.org/download.html

+ Выбираем bibary или mirror
+ Распаковываем на диск С в папку groovy
+ Как и с Java прописываем переменные среды путь
+ ```groovy -version``` проверяем версию в консоле

Доп материал https://groovy-lang.org/install.html

[Вернуться в меню _Basics_](#Basics)



## SDK

Можно установить SDK так называемые менеджер версий, и поставить груви через него.
А потом управлять версиями, в таком случае прописывать в переменные среды ненадо и все просто, управляется
через консоль.

Возможно потребуется впн, с linux лучше работает чем с windows
sdk
установка https://sdkman.io/install
```bash
sdk help
sdk list (qw - exit)
sdk list groovy
sdk install groovy 4.0.15
sdk default groovy 4.0.10 - сделать версию груви по умолчанию
```
[Вернуться в меню _Basics_](#Basics)



## groovyc
https://groovy-lang.org/groovyc.html

groovyc - чтобы скомпилировать файл groovy в файл класса
(он компилируется в байткод Java и чтобы запустить скомпилированный код groovy нам нужна установленная Java)

```bash
groovyc MyClass.groovy
```

В результате создастся класс - MyClass.class

[Вернуться в меню _Basics_](#Basics)



## groovysh
https://groovy-lang.org/groovysh.html

[Вернуться в меню _Basics_](#Basics)



## Groovy Console
https://groovy-lang.org/groovyconsole.html
Groovy Console - позволяет пользователю вводить и запускать сценарии Groovy.

+ позволяет дебажить используя браузер AST
+ ```groovuConsole``` - так можно запустить груви консоль из командной строки
+ script-inspect AST - output - как выглядит наш код внутри
+ ```groovyConsole main.groovy``` - запустить скрипт или файл груви в консоле

[Вернуться в меню _Basics_](#Basics)



## Импорт по умолчанию в Groovy

Все эти пакеты импортируются по умолчанию - вам не нужно их импортировать

+ java.io.*
+ java.lang.*
+ java.math.BigDecimal
+ java.math.BigInteger
+ java.net.*
+ java.util.*
+ groovy.lang.*
+ groovy.util.*

Остальное импортируем все как в Java

```groovy
// importing the class MarkupBuilder
import groovy.xml.MarkupBuilder

// using the imported class to create an object
def xml = new MarkupBuilder()//можно тыкнуть на класс и нажать ctrl+space появится список импортов
```

```groovy
import groovy.xml.*//импорт всего
import static Boolean.FALSE//импорт статики
```

```groovy
import static java.lang.String.format//	статический импорт метода

class SomeClass {

    String format(Integer i) {//	объявление метода с тем же именем, что и метод, статически импортированный выше, но с другим типом параметра
        i.toString()
    }

    static void main(String[] args) {
        assert format('String') == 'String'//ошибка компиляции в Java, но это правильный отличный код
        assert new SomeClass().format(Integer.valueOf(1)) == '1'
    }
}
```
>Если у вас одинаковые типы, импортированный класс имеет приоритет

```groovy
//чтобы повычить читаемость кода можем присваивать понятный нам псевдоним 
import static Calendar.getInstance as now//статический импорт с псевдонимом

assert now().class == Calendar.getInstance().class

//обычный импорт с псевдонимом
import java.util.Date
import java.sql.Date as SQLDate

Date utilDate = new Date(1000L)
SQLDate sqlDate = new SQLDate(1000L)

assert utilDate instanceof java.util.Date
assert sqlDate instanceof java.sql.Date
```
Документация по импорту https://groovy-lang.org/structure.html#_imports

[Вернуться в меню _Basics_](#Basics)



## Ключевые слова

>Груви использует почти все ключевые слова JAVA и немного своих

Java
https://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html

Groovy
https://groovy-lang.org/syntax.html#_keywords

[Вернуться в меню _Basics_](#Basics)



## Комментарии
```groovy
//this is a single line comment
```

```groovy
def msg =  "hello comment" // this is message
```

```groovy
/*
This is multiline comment
*/
```

```groovy
Groovy duck comment
Используется для документации кода
/**
* For class or method
*
*/
```

>hash bang (shebang)
специальный комментарий к строке понятный Unix системам
который позволяет запуск скрипта из командной строки если у вас установлен груви

Пример
```groovy
#!/usr/bin/env groovy
println "Hello from the shebang line"
```

статья примеры комментариев
https://medium.com/@webseanhickey/the-evolution-of-a-software-engineer-db854689243#.mrzv788hj

[Вернуться в меню _Basics_](#Basics)



## Assertions

```groovy
// you must provide an assetion an expression that evaluates to true
assert true

// we can provide a full expression on the right hand side
// note that unlike Java and more like Ruby or Scala == is equality
assert 1 == 1

// like the example above we are evaluating an expression
def x = 1
assert x == 1

// what happens when the expression doesn't evaluate to true?
assert false

// The power assertion output shows evaluation results from the outer to the inner expression
assert 1 == 2

// complex debug output
assert 1 == (3 + 10) * 100 / 5 * 20

// The power assertion statements true power unleashes in complex Boolean statements,
// or statements with collections or other toString-enabled classes:
def x = [1,2,3,4,5]
assert (x << 6) == [6,7,8,9,10]
```


[Вернуться в меню _Basics_](#Basics)


## Class и модификаторы

>Модификаторы доступа на примере класса groovy
 
```groovy
package com.evgenys.basics.classes

@groovy.transform.ToString()//дефолтный toString 
class Developer {//классы и методы по умолчанию public

    String first//все переменные по умолчанию private
    String last
    def languages = []
    
    void work() {//классы и методы по умолчанию public
        println "$first $last is working..."
    }

}
```

```groovy
// create a new instance of a developer
Developer d = new Developer()
d.first = "Dan"//груви внутри все равно вызывает сеттер
d.setLast("Vega")

// assign some languages
d.languages << "Groovy"//оператором левого сдвига добавляем в массив элемент
d.languages << "Java"
```

[Вернуться в меню _Basics_](#Basics)



## Numbers
>Судя по примерам ниже в Groovy нет примитивов и он работает с классами обертками, подбирая автоматом нужный.

```groovy
1.getClass().getName()// java.lang.Integer
111222333444555.getClass().getName()// java.lang.Long
1112223334445553838383838388383838383883..getClass().getName()// java.math.BigInteger

//если мы заходим присвоить примитив груви все равно использует Integer(класс обертку)
int x = 1
x.class // class java.lang.Integer

//если использовать def нам не важен тип
def y = 2
y.class // class java.lang.Integer

5.5.class // class java.math.BigDecimal

def x = 5.5d
x.class // class java.lang.Double
```

[Вернуться в меню _Basics_](#Basics)





## if else while for each switch

```groovy
// if 
if( true ) 
  println "value is true"

// false | null | empty strings | empty collections
if( !false )
  println "value is false" 
  
String name = "Dan"
if( name ) 
  println "name has a value"  
  
String last = ""
if( last ) 
  println "last has a value"
  
  
// if/else 
def x = 5
if( x  == 10 ){ 
  println "x is 10"
} else {
  println "x is NOT 10"
}

// classic while
def i = 1
while( i <= 10 ) {
    println i
    i++
}


// for in list
def list = [1,2,3,4]
for( num in list ){
    println num
}

// closure
def list2 = [1,2,3,4]
list2.each { println it }

// switch
def myNumber = 1

switch(myNumber) {

    case 1:
        println "number is 1"
        break
    default:
        println "we hit the default case"    

}
```

[Вернуться в меню _Basics_](#Basics)




## Аннотации

>Например мы можем создать неизменяемый класс при помощи аннотации из пакета transform
>Создастся класс с финальными полями, equals, hashcode, toString сгенерятся и конструктор с полями
```groovy
import groovy.transform.Immutable

@Immutable
class Customer {

    String first, last
    int age
    Date since
    Collection favItems

}
```

> Или же создать обычный класс но с красивым toString , hashcode и equals

```groovy
 import groovy.transform.Canonical
 @Canonical class Customer {
     String first, last
     int age
     Date since
     Collection favItems = ['Food']
     def object
 }
```

```groovy
 def d = new Date()
 def anyObject = new Object()
 def c1 = new Customer(first:'Tom', last:'Jones', age:21, since:d, favItems:['Books', 'Games'], object: anyObject)
 def c2 = new Customer('Tom', 'Jones', 21, d, ['Books', 'Games'], anyObject)
 assert c1 == c2
```

>Sortable
  
```groovy
import groovy.transform.*

@ToString
@Sortable//добавляет метод compareTo если посомтреть через AST
class Person {

    String first
    String last

}

def p1 = new Person(first:"Joe",last:"Vega")
def p2 = new Person(first:"Dan",last:"Vega")

def people = [p1,p2]
println people

def sorted = people.sort(false /* do not mutate original collection */ )
println sorted
```
https://groovy-lang.org/objectorientation.html#_annotations

Вверху находим пакет groovy.transform далее нужную нам аннотацию
https://docs.groovy-lang.org/next/html/gapi/

[Вернуться в меню _Basics_](#Basics)

## Операторы
>Groovy поддерживает обычные арифметические операторы,
> которые можно встретить в математике и других языках программирования, например Java.
https://groovy-lang.org/operators.html

<details> 
<summary>Примеры </summary>

```groovy
// Arithmetic operators

assert 10 + 1 == 11
assert 10 - 1 == 9
assert 10 * 2 == 20
assert 10 / 5 == 2
assert 10 % 3 == 1
assert 10 ** 2 == 100

// The binary arithmetic operators we have seen above are also available in an assignment form:
// += -= *= /= %= **=

def a = 10
a += 5 // a = a + 5
assert a == 15

// Relational operators
assert 1 + 2 == 3
assert 3 != 4

assert -2 < 3
assert 2 <= 2
assert 3 <= 4

assert 5 > 1
assert 5 >= -2

// Logical Operators
assert !false           
assert true && true     
assert true || false
// Conditional Operators
    // Ternary Operator
    String s = ""
    if( s != null && s.length() > 0 ) {
        result = 'Found'
    } else {
        result = 'Not found'
    }
    
    String s = ""
    result = ( s != null && s.length() > 0 ) ? 'Found' : 'Not Found'
// можно написать еще проще, так как в груви null или пустая строка определяется как false
    result = string ? 'Found' : 'Not found'

    // Elvis Operator
    //«Оператор Элвиса» — это сокращение тернарного оператора. Одним из примеров, 
    // когда это удобно, является возврат значения «разумного значения по умолчанию»
    displayName = user.name ? user.name : 'Anonymous'   
    displayName = user.name ?: 'Anonymous' //то же самое если имя пользователя существует используйте его - в другом случае Anonymous  

// Object Operators

    // Safe Navigation Operator

    // Java
    Person p = new Person()
    if( p.address != null ) {
        Address address = p.address
        address.first = "1234 Main"
    }
    
    // Groovy Нулевой оператор "?" позволяет нам не получить NullPointerException
    // Если поле null мы достаем null  присваеваем его
    def address = p?.address//	использование нулевого оператора предотвращает NullPointerException
    assert address == null//результат null
```
</details>

[Вернуться в меню _Basics_](#Basics)



## Grape

>Grape — это менеджер зависимостей JAR, встроенный в Groovy. 
> Grape позволяет быстро добавлять зависимости репозитория Maven в ваш путь к классам, 
> что еще больше упрощает создание сценариев. 

> Простейшее использование — это простое добавление аннотации к вашему скрипту:
```groovy
@Grab(group='org.springframework', module='spring-orm', version='5.2.8.RELEASE')
import org.springframework.jdbc.core.JdbcTemplate
```
### @Grab также поддерживает сокращенную запись:

```groovy
@Grab('org.springframework:spring-orm:5.2.8.RELEASE')
import org.springframework.jdbc.core.JdbcTemplate
```
>Обратите внимание, что здесь мы используем аннотированный импорт, и это рекомендуемый способ.
> Вы также можете выполнить поиск зависимостей на сайте mvnrepository.com , и он предоставит
> вам @Grabформу аннотации записи pom.xml.

### Укажите дополнительные репозитории

>Не все зависимости находятся в maven Central. Вы можете добавить новые, например:
> 
```groovy
@GrabResolver(name='restlet', root='http://maven.restlet.org/')
@Grab(group='org.restlet', module='org.restlet', version='1.1.6')
```

### Один из примеров подключения зависимости

+ Создали скрипт app.groovy 
+ запускаем -> groovyConsole app.groovy (нужно быть в дирректории файла)
+ Запустится консоль груви, далее Script -> Run
+ Вывод будет: DAV


```groovy
@Grapes(
    @Grab(group='org.apache.commons',module='commons-lang3',version='3.4')
)

import org.apache.commons.lang3.text.WordUtils

String name = "Daniel Anthony Vega"
WordUtils wordUtils = new WordUtils()

println wordUtils.initials(name)//принимает строку а возвращает инициалы
```

#### Документация Groovy намного подробнее о Grape
http://docs.groovy-lang.org/latest/html/documentation/grape.html


[Вернуться в меню _Basics_](#Basics)





## Grape


[Вернуться в меню _Basics_](#Basics)




## Grape


[Вернуться в меню _Basics_](#Basics)
