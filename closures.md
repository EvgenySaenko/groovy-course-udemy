[Главная](README.md)

# Closures

+ [About Closures](#About-Closures)
+ [Basic Closures](#Basic-Closures)
+ [Closure Parameters](#Closure-Parameters)
+ [Collections Methods](#Collections-Methods)
+ [Curry Methods](#Curry-Methods)
+ [Closure Scope and Delegates](#Closure-Scope-and-Delegates)





## About Closures

>Официальная документация 
> + http://groovy-lang.org/closures.html
> + http://docs.groovy-lang.org/latest/html/api/groovy/lang/Closure.html

>Без понимания замыкания мы не поймем Groovy полностью, замыкания везде и они сделали Groovy таким популярным и крутым =)

> Замыкания похож на метод, но в отличие от метода - это еще и объект и может и спользоваться и передаваться в программе
> 
>Замыкание в Groovy — это открытый анонимный блок кода, который 
+ может принимать аргументы
+ возвращать значение 
+ присваиваться переменной
+ ссылаться на переменные объявленные в окружающей его области. 
>В отличие от формального определения замыкания, 
> Closure в языке Groovy также могут содержаться свободные переменные, определенные вне окружающей 
> его области видимости. Нарушая формальную концепцию замыкания, оно предлагает множество 
> преимуществ, описанных в этой главе.

### Где применяются и используются замыкания?

+ iterators - дает возможность перебора коллекций и других объектов
+ Callback - нам не нужны анонимные классы как в джава - можем создавать Callback
+ Higher-order function - имеем функции высокого порядка, функции, 
которые вызывают другие
+ Specialized Control Structure - используя замыкания можем создать собственную структуру управления
+ Builders - можем создать различные структуры HTML, XML
+ Resource allocation - помогает не беспокоится о закрытии ресурсов(типа чтения файлов итд)
+ Threads -  создание потоков через замыкания
+ DSLs - позволяют создавать собственный DSL
+ Fluent Interfaces - свободные интерфейсы

[Вернуться в меню _Closures_](#Closures)



## Basic Closures

```groovy
def c = { } // альтернатива Closure c = { }
println c.class.name //Basics$_run_closure1 - как в джава имя анонимного класса ничего нам не даст по сути это анонимный блок кода
println c instanceof Closure // но при этом это объект Closure


def sayHello = {//по сути этот блок является телом метода - это анонимная функция
    println "Hello"
}
sayHello()//Hello

//это похоже на функцию которая может принимать аргументы
def sayHello = { name -> //стрелкой мы отделяем аргументы от тела метода, все что после стрелки - это тело метода(тело замыкания)
    println "Hello, $name"
}
sayHello('Dan')//Hello, Dan
//!!! Главное отличие замыкания от метода, метод не может передаваться - замыкание может
```
> Примеры определения замыканий
```groovy
{ item++ } //Замыкание, ссылающееся на переменную с именемitem                                         

{ -> item++ } //Можно явно отделить параметры замыкания от кода, добавив стрелку ( ->)                                       

{ println it } //Замыкание с использованием неявного параметра ( it)                                     

{ it -> println it }//Альтернативная версия, где itуказан явный параметр                                

{ name -> println name }//В этом случае часто лучше использовать явное имя параметра.                            

{ String x, int y ->  //Замыкание, принимающее два типизированных параметра                              
    println "hey ${x} the value is ${y}"
}

{ reader -> //Замыкание может содержать несколько операторов                                        
    def line = reader.readLine()
    line.trim()
}
//Замыкания как объект 
def listener = { e -> println "Clicked on $e.source" } //Вы можете назначить замыкание переменной, и это экземплярgroovy.lang.Closure
assert listener instanceof Closure
Closure callback = { println 'Done!' } //Если не используется defили var, используйте groovy.lang.Closureв качестве типа
Closure<Boolean> isTextFile = {//возвращаемый тип замыкания Closure<Boolean> 
    File it -> it.name.endsWith('.txt')//При желании вы можете указать тип возвращаемого значения замыкания, используя общий тип groovy.lang.Closure
}

```

```groovy
it - это ключевое слово в замыкании, означает элемент с которым мы работаем, если в замыкание не передано других

List nums = [1,4,7,4,30,2]
nums.each({ //в Collection метод each(Closure closure) принимает замыкание
    println it//это ключевое слово в замыкании, означает элемент с которым мы работаем, если в замыкание не передано других
})

nums.each({num -> //явное указание параметра
    println num
})

//closures are objects & last param
def timeTen(num, closure) {//
    closure(num * 10)
}

timeTen(10, { println it }) //100 - замыкание вызовет println перед этим умножив на 10

//когда замыкание является последним, мы можем удалить скобки и вынести его на ружу
//эта запись говорит, что метод принимает 2 аргумента, 1- число, 2- замыкание
//поэтому нам не нужно было держать его в этих () скобках 
timesTen(2) {
    println it
}

//Поэтому так как в метод приходил одно замыкание мы можем убрать скобки ()
nums.each { num -> 
    println num
}

// возьмем любой метод для числа например times() он принимает замыкание, поэтому убираем скобки
10.times {
    println it// напечатает от 0-9 включительно
}

import java.util.Random

Random rand = new Random()

3.times {
    println rand.nextInt() // выведет 3 рандомных числа int
}

```

[Вернуться в меню _Closures_](#Closures)



## Closure Parameters

```groovy
// не явный параметр
def foo = {
    println it
}

foo('dan')

//Если мы хотим чтобы замыкание не принимало параметры ставим стрелку ->
def noparams = { ->
    println "no params..."
}
//скажет что нет метода который бы принимал параметры с таким именем
noparams(1)// при вызове будет groovy.lang.MissingMethodException No signature of method

//Несколько параметров - нам не важен тип, все равно что написали бы (def first, def last)
def sayHello = { first, last ->
    println "Hello, $first $last"
}



def sayHello = { String first, String last ->
    println "Hello, $first $last"
}

sayHello("Dan","Vega")// Hello, Dan Vega

// дефолтный параметр
def greet = { String name, String greeting = "Howdy" ->
    println "$greeting, $name"
}
greet("Dan","Hello") //Hello, Dan
greet("Joe")// Howdy, Joe - не передали 2-й параметра приветствие - он заменился дефолтным


//var-arg - Аргумент переменной длины в джава по сути
def concat = { String... args ->
    args.join('')
}

println concat('abc','def')//abcdef
println concat('abc','def','123','456')//abcdef123456

//методы который принимает замыкание
def someMethod(Closure c) {
    println "..."
    println c.maximumNumberOfParameters// выводит максимальное количество параметров которое может принимать переданное в метод замыкание
    println c.parameterTypes // типы параметров переданного в метод замыкания
}

def someClosure = { int x, int y -> x + y }
someMethod(someClosure)

```

[Вернуться в меню _Closures_](#Closures)


## Collections Methods

>[Documentation Groovy Collection](https://docs.groovy-lang.org/latest/html/groovy-jdk/java/util/Collection.html)

 + Collection	each(Closure closure)
Выполняет итерацию по коллекции, передавая каждый элемент данному замыканию.
 + Collection	eachWithIndex(Closure closure)
Выполняет итерацию по коллекции, передавая каждый элемент и индекс элемента (счетчик, начинающийся с нуля) в заданное замыкание
+ Object	find()
    Находит первый элемент, соответствующий замыканию IDENTITY (т. е. соответствующий истинности Groovy).
+ Object	find(Closure closure)
    Находит первое значение, соответствующее условию закрытия.
+ Collection	findAll()
    Находит элементы, соответствующие замыканию IDENTITY (т. е. соответствующие истинности Groovy).
+ Collection	findAll(Closure closure)
    Находит все значения, соответствующие условию закрытия.
+ 
```groovy
//===== each & eachWithIndex=====
List favNums = [2,21,44,35,8,4]

for(num in favNums) {
    println num
}
//each() - можно убрать () скобки, так как у метода 1 аргумент на вход и это замыкание
favNums.each { println it } 

//чтобы распечатать индекс - значение в нем, мы бы могли сделать так
for( int i=0; i<favNums.size(); i++){
    println "$i:${favNums[i]}"
}
// метод eachWithIndex() еще один способ сделать это
favNums.eachWithIndex { num, idx ->
    println "$idx:$num"
}

// findAll
// - Находит все значения, соответствующие условию закрытия
// - не влияет на основной список, создает новый список на основе условия
List days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]
List weekend = days.findAll { it.startsWith("S") } //перебирает элементы возвращает список всех совпадений

println days//["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]
println weekend//["Sunday","Saturday"]


// collect
List nums = [1,2,2,7,2,8,2,4,6]
// допустим нужно новый список но каждый элемент умноженый на 10
List numsTimesTen = []// мы бы создали пустой список
nums.each { num -> // перебрав его умножали и клали используя << вновый список
    numsTimesTen << num * 10
}
//версия метода collect() которая принимает замыкание
//изменит каждый элемент списка и вернет нам список с преобразованными значениями
List newTimesTen = nums.collect { num -> num * 10 } 

println nums//[1,2,2,7,2,8,2,4,6]
println numsTimesTen//[10,20,20,70,20,80,20,40,60]
println newTimesTen//[10,20,20,70,20,80,20,40,60] //то же самое

```

[Вернуться в меню _Closures_](#Closures)



## Curry Methods

>http://docs.groovy-lang.org/latest/html/api/groovy/lang/Closure.html
> 

+ Closure<V> curry(Object argument) Поддержка каррирования замыканий.
+ Closure<V> curry(Object... arguments) Поддержка каррирования замыканий.
+ Closure<V> ncurry(int n, Object argument) Поддержка каррирования замыканий по заданному индексу.
+ Closure<V> ncurry(int n, Object... arguments) Поддержка каррирования замыканий по заданному индексу.
+ Closure<V> rcurry(Object argument) Поддержка «правильного» каррирования замыканий.
+ Closure<V> rcurry(Object... arguments) Поддержка «правильного» каррирования замыканий.

```groovy
package com.evgenys.closures
//curry - чтобы не передевать постоянно одни и те же параметры по несколько раз
//создадим метод дебаг напрмиер как в Java
def log = { String type, Date createdOn, String msg ->
    println "$createdOn [$type] - $msg"
}
//вывод урока - Sat Apr 02 12:01:42 EDT 2016 [DEBUG] - This is  my first debug statement...
log("DEBUG",new Date(),"This is my first debug statement...")
log("DEBUG",new Date(),"This is my first debug statement...")
log("DEBUG",new Date(),"This is my first debug statement...")
//но если вызывать его постоянно то мы будем повторяться и как быть?

//тут мы вызываем наше замыкание log и затем используя curry передаем только один парамметр
def debugLog = log.curry("DEBUG")//в итоге возвращаем новое замыкание - которое ждет уже 2 аргумента
debugLog(new Date(), "This is another debug statement...")//дата и месседж
debugLog(new Date(), "This is one more...")//и у нас появляется гибкость

//тут я хочу передать дату и тип но месседж использовать каждый раз разный
def todayDebugLog = log.curry("DEBUG",new Date())
todayDebugLog("This is today's debug msg")// уже будет ждать один параметр
todayDebugLog("This is today's debug msg number two")


// right curry -  тут мы передаем правый параметр (последний)
def crazyPersonLog = log.rcurry("Why am I logging this way")
crazyPersonLog("ERROR",new Date())// и потом первые два

// index based currying индексация парметров 
// начинается с 0,1,2 итд - тут мы решили передать только дату она в 1 индексе(вторым параметром)
def typeMsgLog = log.ncurry(1,new Date())
typeMsgLog("ERROR","This is using ncurry...")// соответственно останется заполнить 2 параметра по индексу 0 и 2 (тип и месседж)
```

[Вернуться в меню _Closures_](#Closures)




## Closure Scope and Delegates

> __Делегирование__ — это ключевая концепция замыканий Groovy, которая не имеет эквивалента в лямбда-выражениях. Возможность изменить делегата или изменить стратегию делегирования замыканий позволяет создавать красивые предметно-ориентированные языки (DSL) в Groovy.

__Owner, delegate and this__
>Чтобы понять концепцию делегата, мы должны сначала объяснить значение слова 
> « this внутри замыкания». Замыкание фактически определяет 3 разные вещи:

+ `this` соответствует включающему классу , в котором определено замыкание 
+ `owner` соответствует включающему объекту , в котором определено замыкание, которое может быть классом или замыканием
+ `delegate` соответствует стороннему объекту, где вызовы методов или свойства разрешаются всякий раз, когда получатель сообщения не определен

```groovy
class ScopeDemo {

    def outerClosure = {
        println this.class.name //ScopeDemo
        println owner.class.name //ScopeDemo
        println delegate.class.name //ScopeDemo
        def nestedClosure = {
            println this.class.name //ScopeDemo
            println owner.class.name //ScopeDemo$_closure
            println delegate.class.name //ScopeDemo$_closure         
        }
        nestedClosure()
    }

}

def demo = new ScopeDemo()
demo.outerClosure()

//====================рассмотрим делегат==================
def writer = {
    append 'Dan'
    append ' lives in Cleveland'
}

def append(String s) {
    println "append() called with argument of $s"
}
//когда вы ывзвали замыкание, замыкание смотрит есть ли метод append в классе в котором оно находится
// и вызывает его
writer()
//append() called with argument of Dan
//append() called with argument of lives in Cleveland

//=====================назначим делегата====================
def writer = {
    append 'Dan'
    append ' lives in Cleveland'
}

StringBuffer sb = new StringBuffer()
writer.delegate = sb//скажем что стринг буфер наш делегат
writer()// и когда вызовется замыкание writer то метод append будет искаться у нашего стрингбуфера(делегата)
//Dan lives in Cleveland

//========что если назначен делегат и внутри класса есть метод append???====================
def writer = {
    append 'Dan'
    append ' lives in Cleveland'
}

def append(String s) {
    println "append() called with argument of $s"
}
//по умолчанию владельцем в замыкании - включающий класс
//поэтому вызоветься метод включающего класса
StringBuffer sb = new StringBuffer()
writer.delegate = sb
writer()
//append() called with argument of Dan
//append() called with argument of lives in Cleveland


// =========== Выставим приоритет для замыкания ====================
//Если мы поссмотрим на документацию замыкания(ссылка ниже)
//То в статических полях есть константы DELEGATE_FIRST,OWNER_FIRST итд

def writer = {
    append 'Dan'
    append ' lives in Cleveland'
}

def append(String s) {
    println "append() called with argument of $s"
}
// если мы установим стратегию writer.resolveStrategy = Closure.DELEGATE_FIRST
// это значит пусть наш делегат использует свой метод append и если не найдет, тогда будем использовать
//метод владельца(owner) а именно метод класса в котором находится замыкание

StringBuffer sb = new StringBuffer()
writer.resolveStrategy = Closure.DELEGATE_FIRST//устанавливаем стратегию
writer.delegate = sb
writer()
//Dan lives in Cleveland

```
https://docs.groovy-lang.org/latest/html/api/groovy/lang/Closure.html

[Вернуться в меню _Closures_](#Closures)
















## Basic Closures


[Вернуться в меню _Closures_](#Closures)


## Basic Closures


[Вернуться в меню _Closures_](#Closures)