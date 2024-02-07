[Главная](README.md)

# collections

+ [Range](#Range)
+ [List](#List)




## Range

>Range(диапазон) — это сокращение для указания последовательности значений.
> Диапазон обозначается первым и последним значениями в последовательности,
> и диапазон может быть включающим или исключающим. 
> Включающий диапазон включает все значения от первого до последнего,
> а исключительный диапазон — все значения, кроме последнего.
> 
>Официальная документация https://groovy-lang.org/api.html
> Слева выбрать пакет groovy.lang а ниже Интерфейсы и Range

> Вот несколько примеров литералов диапазона:

+ `1..10` – пример инклюзивного диапазона. `[1,2,3,4,5,6,7,8,9,10]`
+ `1..<10` — пример эксклюзивного диапазона.`[1,2,3,4,5,6,7,8,9]`
+ `'a'..'x'` — диапазоны также могут состоять из символов.
+ `10..1` – Диапазоны также могут быть в порядке убывания.
+ `'x'..'a'` — диапазоны также могут состоять из символов и располагаться в порядке убывания.

>Range это [Java] Interface Range<T extends Comparable>
>СОдержит большое количество методов https://docs.groovy-lang.org/latest/html/api/groovy/lang/IntRange.html

```groovy
Range r = 1..10// двойные точки указывают что это диапазон
println r // вывод - [1,2,3,4,5,6,7,8,9,10]
println r.class.name// groovy.lang.IntRange
println r.from//1 достали первый элемент
println r.to//10 достали последний

assert (0..10).contains(0)//использование метода contains
assert (0..10).contains(10)
assert (0..10).contains(-1) == false
assert (0..10).contains(11) == false

assert (0..<10).contains(0)
assert (0..<10).contains(10) == false

Date today = new Date()//дата на момент урока
Date oneWeekAway = today + 7 // thank the GDK for that simple statement

println today//Wed Feb 07 22:32:27 MSK 2024
println oneWeekAway//Wed Feb 14 22:32:27 MSK 2024 - добавило 7 дней

//допустим мы хотим создать диапазон дней
Range days = today..oneWeekAway
println days//выведет сокращенно Wed Feb 07 22:32:27 MSK 2024..Wed Feb 14 22:32:27 MSK 2024

for(def d: days) {// выведет весь диапазон дней 
    println d
}

Range letters = 'a'..'z'
println letters


enum Days {
    SUNDAY,MONDAY,TUESDAY,WEDNESDAY,THURSDAY,FRIDAY,SATURDAY
}

def dayRange = Days.SUNDAY..Days.SATURDAY//диапазон енумов

dayRange.each { day ->
    println day //SUNDAY MONDAY TUESDAY WEDNESDAY THURSDAY FRIDAY SATURDAY только в столбик
}

println dayRange.size()//7
println dayRange.contains(Days.WEDNESDAY)

// Bonus: next() and previous() are equivalent to
// ++ and -- operators.
def wednesday = Days.WEDNESDAY
assert Days.THURSDAY == ++wednesday
assert Days.WEDNESDAY == --wednesday
```


[Вернуться в меню _collections_](#collections)








## List

>__List(список)__
> 
> Официальная документация https://groovy-lang.org/gdk.html слева выбрать java.util а ниже List
>
>Можно почитать подробнее про методы plus и minus - создают новый список добавляя в него или удаляя элемент
https://docs.groovy-lang.org/latest/html/groovy-jdk/java/util/List.html#plus(int,%20java.lang.Iterable)
https://docs.groovy-lang.org/latest/html/groovy-jdk/java/util/List.html#minus(java.lang.Object)

>Ниже пример только основных методов, остальные можно посомтреть в документации их куча

```groovy
def nums = [1,2,3,6,7,9,4,5,3,6,8,9]//java.util.ArrayList по умолчанию 
List nums = [1,2,3,6,7,9,4,5,3,6,8,9]//так тожже можно и ArrayList тоже можно
def linkedList = [1,2,3,6,7,9,4,5,3,6,8,9] as LinkedList // если нам нужен линкедлист

println nums
println nums.class.name//java.util.ArrayList
println linkedList.class.name //java.util.LinkedList

// add | remove | get | clear

// add
nums.push(99)// [1,2,3,6,7,9,4,5,3,6,8,9,99]- добавит в конец
nums.putAt(0,77)// [77,2,3,6,7,9,4,5,3,6,8,9,99]- добавит указанную а именно в первую позицию
nums[0] = 78// аналогичный способ положить в ячейку определенного индекса
nums + [1,2,3]// обавит в конец - [1,2,3,6,7,9,4,5,3,6,8,9,1,2,3]
nums + 7// можно использовать plus метод но чтобы он добавился мы долны переприсвоить num = num + 7
nums << 66 // тут переприсваивать не нужно сразу добавит в список

// remove
nums.pop()//удалит последний элемент
nums.removeAt(0)//удалит элемент в этом индексе
def newList = nums - 3//удалит элемент в списке со значением 3 [1,2,4,3] -> [1,2,4]
println newList


//get [1,2,3,6,7,9,4,5,3,6,8,9]
nums[4]//достанем элемент по 4 индексу
println nums.getAt(0..3)//достанет диапазон [1,2,3,6]

//clear
nums = []//оба варианта очистят список
nums.clear()

//for перебрать список
for (x in nums) {
    println x
}

// flatten [1,2,3,6,7,9,4,5,3,6,8,9] список может содержать не только число но и вложенные списки
nums << [3,4,5] //[1,2,3,6,7,9,4,5,3,6,8,9,[3,4,5]] добавили вложенные списки
nums << [1,2]//[1,2,3,6,7,9,4,5,3,6,8,9, [3,4,5], [1,2]] 
println nums
//сделаем один большой список - сглаживанием
println nums.flatten()//уже без вложенных списков [1,2,3,6,7,9,4,5,3,6,8,9,3,4,5,1,2] 

// equals
def myNumbers = [1,2,3]
def myNumbers2 = [1,2,3]
println myNumbers.equals(myNumbers2)

// findAll
println nums.findAll { it == 4 }
println nums.flatten().findAll { it < 5 }

// getAt(Range)
println nums.getAt(0..5)

// reverse list
println nums.reverse()

// unique
println nums.unique()//выведет только уникальные элементы

// Java Collections List(LinkedList) (Set,SortedSet)
def evens = [10,2,8,4,24,14,2] as Set//если хотим использовать множество уникальных элементов 
println evens//[10,2,8,4,24,14]
println evens.class.name//java.util.LinkedHashSet

def evens = [10,2,8,4,24,14,2] as SortedSet
println evens//[2,4,8,10,14,24]
println evens.class.name//java.util.TreeSet

```


[Вернуться в меню _collections_](#collections)

















## Grape


[Вернуться в меню _collections_](#collections)



## Grape


[Вернуться в меню _collections_](#collections)