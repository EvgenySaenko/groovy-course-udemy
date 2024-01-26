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
+ [Operator overloading](#Operator-overloading)
+ [String](#String)



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
//Groovy duck comment используется для документации кода
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

>Факты о числах в Groovy
 
+ Groovy все объекты у него нет примитивов
+ def x = 10 - groovy по умолчанию присвоит Integer
+ 1.23 или другое число с плавающей точкой в groovy это BigDecimal
+ Когда мы используем def Groovy подбирает автоматот тип данных
+ Тип данных в переменной объявленной через `def` - может меняться

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

> ### def - `используется, если нам не важен тип и он может меняться`
+ ВАЖНО!! - тип переменной объявленной через `def` может меняться

```groovy
def x = 10
x.getClass().getName()//class java.lang.Integer

x = "Dan"
x.class// java.lang.String
```


```groovy
def myFloat = (float) 1.0//так называемый каст
println myFloat.class//class java.lang.Float здесь мы явно говорим что нам не нужен BigDecimal а нужен Float

// в groovy если хоть один из операндов является числом с плавающей точкой - результатом будет всегда Double
// в Java бы результатом был бы Float
Float f = 5.25
Double d = 10.50

def result = d / f
println result
println result.class//class java.lang.Double

Float a = 10.75
FLoat b = 53.75
def result2 = b / a
println result2
println result2.class//class java.lang.Double

// If either operand is a big decimal 

def x = 34.5 // BigDecimal
def y = 15 //Integer
def bigResult = x / y //результатом груви делает в сторону большего числа - BigDecimal
println bigResult
println bigResult.class//class java.math.BigDecimal

// If either operand is a BigInteger the result is a BigInteger
// If either operand is a Long the result is a Long
// If either operand is a Integer the result is an Integer

// Double division
println 5.0d - 4.1d//0.9000000000000004 - и тут будет немного ошибка
println 5-4.1//0.9

// Integer Division

def intDiv = 1 / 2// в груви мы получаем 0.5, потому что идем на повышение типа - BigDecimal
println intDiv // this is much different than Java where we would get 0 
println intDiv.getClass().getName()
println 1.intdiv(2)//используя метод intdiv мы видим как бы было в Java ответ был бы - 0 


// times | upto | downto | step

20.times {
    print '-'//напечатает 20 "-" вот так: ---------------------
}

// пройти от 1 до 10(включая 10)  и распечатаем это 12345678910
1.upto(10) { num ->
    println num
}

// противоположный верхнему методу, распечатает 10987654321
10.downto(1) { num ->
    println num
}
// переход от 0-1 с шагом 0.1, вывод: 0  0.1  0.2  0.3  0.4  0.5  0.6  0.7  0.8  0.9
0.step(1,0.1) { num ->
    println num
}
```

>### Приведение типов

```groovy
// GDK Methods
assert 2 == 2.5.toInteger() // conversion
assert 2 == 2.5 as Integer  // enforced coercion
assert 2 == (int) 2.5 // cast

assert '5.50'.isNumber()//является ли строка числом будет true
assert 5 == '5'.toInteger()
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





## Operator overloading

https://groovy-lang.org/operators.html#Operator-Overloading
>Все операторы Groovy (не компараторы) имеют соответствующий метод, который вы можете реализовать
в своих собственных классах. Единственные требования :

+ чтобы ваш метод был общедоступным
+ имел правильное имя и правильное количество аргументов.
+ Типы аргументов зависят от того, какие типы вы хотите поддерживать в правой части оператора.

```groovy

def a = 1
def b = 2

//ниже две записи одинаково правильные, в первой числовой класс использует метод plus закулисами
println a + b//3
println a.plus(b)//3

def s1 = "Hello"
def s2 = ", World!"

//ниже две запись одинаковые, просто первая класс Стринг закулисами использует метод plus
println s1 + s2//"Hello, World"
println s1.plus(s2)//"Hello, World"

//Но если нам нужно сложить просто два объекта 
class Account {
    BigDecimal balance
}
Account savings = new Account(balance:100.00)
Account checking = new Account(balance:500.00)

println savings + checking// мы получим ошибку MissingMethodException: No signature of method: Account.plus() is applicable for argument types итд

//Но если мы объясним как складывать, добавив метод plus то все будет работать
class Account {
    BigDecimal balance

    Account plus(Account other) {
        new Account( balance: this.balance + other.balance )//можно написать так this.balance + other.balance
    }

    String toString(){
        "Account Balance: $balance"
    }
}

Account savings = new Account(balance:100.00)
Account checking = new Account(balance:500.00)

println savings
println checking
println savings + checking// Account Balance: 600.00

```

[Вернуться в меню _Basics_](#Basics)



## String
####  Ниже описал как смог самое основное, подробнее можно прочитать в документации ниже
https://groovy-lang.org/syntax.html#all-strings

> Groovy поддерживает java.lang.String и groovy.lang.GString
> String это простые строки в которых ничего не вставлено
> GString называют интерполированными строками, когда в строку вставляем поле или выражение.

+ Строки в одинарных кавычках являются простыми java.lang.String и не поддерживают интерполяцию.
+ Строки с тройными одинарными кавычками являются простыми java.lang.String и не поддерживают интерполяцию.
+ Все строки Groovy можно объединить с помощью `+` оператора как и в Java(конкатенация)
+ Интерполировать(вставить) переменную или выражение можно только в строки с двойными кавычками `"`  `"""` в одинарную или мульти строку, так же созданные через `/`

### GString и строковые хеш-коды

> Обычные строки Java неизменяемы, тогда как результирующее строковое представление GString может варьироваться 
> в зависимости от его интерполированных значений. Даже для одной и той же результирующей строки GString и Strings
> не имеют одинакового хэш-кода.

```groovy
assert "one: ${1}".hashCode() != "one: 1".hashCode()//хэш коды не равны
```
>!!!Важно для ключа Map лучше использовать строго String

>В Java имеет значение как мы создаем символ или строку

+ символ в одинарных кавычках `char c = 'c'`
+ строку в двойных `String str = "this is a string"`

>В Groovy все будет String неважно в каких кавычках мы создадим

```groovy
// Java ::
char c = 'c'
println c.class//class java.lang.Character

String str = "this is a string"
println str.class//class java.lang.String

// Groovy ::
def c2 = 'c'
println c2.class//class java.lang.String

def str2 = 'this is a string'
println str2.class//class java.lang.String
```

### В Groovy есть два типа строк
+ String
+ GString

### Итнерполяция строк

```groovy
String name = "Dan"//так мы делаем в Java
String msg = "Hello " + name + "..."
println msg

//Groovy имеет более чистое решение
String msg2 = "Hello ${name}"//Важно делать это в двойных кавычках
println msg2

String msg3 = 'Hello ${name}'//С одинарными это не сработает, мы просто распечатаем фактически то что есть
println msg3

String msg4 = "We can evaulate expressions here: ${1 + 1}"
println msg4//We can evaulate expressions here: 2
```
 
### Multiline strings
+ Если нам нужно просто вывести многострочное сообщение то используем три одинарные кавычки `'''`
+ Если же мы будем передавать туда переменную или выражение то используем двойные `"""`
```groovy

def aLargeMsg = """
A 
Msg
goes 
here and 
keeps going ${1+1}
"""

println aLargeMsg
```

### dollar slashy

+ $/ .. /$ открытие и закрытие доллара
+ все что находится между ними, является строкой

```groovy
def folder = "C:\groovy\dan\foo\bar"//мы могли бы сделать конечно C:\\groovy\\dan\\foo\\bar но есть другой способ
println folder//когда мы попытаемся распечатать: unexpected char '\' at...

// используем конструкцию $/ .. /$ открытие и закрытие доллара
// все что между ними будет восприниматься как строка
def folder2 = $/C:\groovy\dan\foo\bar/$

def slash = /double slesh/ // так тоже можно создать строку
```

### Slashy string

+ Строки с косой чертой особенно полезны для определения регулярных выражений и шаблонов, поскольку нет необходимости экранировать обратную косую черту.
+ Косые строки являются многострочными
+ Косые строки можно рассматривать как еще один способ определения GString, но с другими правилами экранирования.
+ Пустую строку нельзя создать через двойной слеш `//`- он воспринимается как комментарий 

```groovy
def slash = /double slesh/ // так тоже можно создать строку

def fooPattern = /.*foo.*/
assert fooPattern == '.*foo.*'

//Только прямая косая черта должна быть экранирована обратной косой чертой:
def escapeSlash = /The character \/ is a forward slash/
assert escapeSlash == 'The character / is a forward slash'

//Косые строки являются многострочными:
def multilineSlashy = /one
    two
    three/

assert multilineSlashy.contains('\n')

//Косые строки можно рассматривать как еще один способ определения GString, но с другими правилами экранирования. Следовательно, они поддерживают интерполяцию:

def color = 'blue'
def interpolatedSlashy = /a ${color} car/

assert interpolatedSlashy == 'a blue car'
```


[Вернуться в меню _Basics_](#Basics)


## Grape


[Вернуться в меню _Basics_](#Basics)



## Grape


[Вернуться в меню _Basics_](#Basics)