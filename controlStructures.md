[Главная](README.md)

# Control Structures

+ [Groovy Truth](#Groovy-Truth)
+ [Switch](#Switch)
+ [Looping](#Looping)
+ [Exception Handling](#Exception-Handling)








## Groovy Truth

>Если мы уберем `!` то будет false
 
```groovy
// Boolean
assert true
assert !false

// Matcher
assert ('a' =~ /a/)
assert !('a' =~ /b/)

// Collection
assert [1]
assert ![]//пустая коллекция false

// Map
assert [1:'one']
assert ![:] //пустая МАПА - false

// String
assert 'a'
assert !'' //пустая строка - false

// Number
assert 1
assert 3.5
assert !0

// None of the above 
assert new Object() // true
assert !null //false например мы инициализировали Person person; но не создали будет null

// in позволяет увидеть находится ли переменная слева в переменной справа
def validAges = 18..35
def someAge = 19
println someAge in validAges

```

[Вернуться в меню _Control Structures_](#Control-Structures)






## Switch

```groovy

def num = 12

switch( num ) {
    case 1 :
        println "1"
        break//если не ставить break по умолчанию он будет идти вперед заходя в каждый кейс и печатать
    case 2 :
        println "2"
        break
    case 1..3 :
        println "in range 1..3"    
        break
    case [1,2,12] : // если будет одно из этих чисел зайдет сюда
        println "num is in list [1,2,12]"
        break
    case Integer :// если число будет интом - зайдет сюда
        println "num is an Integer"
        break
    case Float :
        println "num is a float"
        break
    default :
        println "default..."    
}
```

[Вернуться в меню _Control Structures_](#Control-Structures)


## Looping

```groovy
// Looping
// --------------------------------------------

// while
List numbers = [1,2,3]
while( numbers ) {
    // do something
    numbers.remove(0)
}

assert numbers == []

// for 

for( variable in iterable ) {

}

List nums = [1,2,3]
for( Integer i in 1..10 ) {
    println i
}

for( i in 1..5 ) { 

}

Closure c = { } 


// return/break/continue

String getFoo() {
    "foo"
}

Integer a = 1
while( true ) { // infinite loop
    a++
    break
}
assert a == 2

for( String s in 'a'..'z' ){
    if( s == 'a') continue
    println s
    if( s > 'b' ) break
}
```

[Вернуться в меню _Control Structures_](#Control-Structures)





## Exception Handling

>В Groovy каждое исключение не является обязательным - это одно из важных отличий от Java

```groovy
// Exceptions 
// ---------------------------------------
/*
public void foo() throws Exception { //в Java мы должны вписывать в сигнатуру метода, что метод может бросить исключение
    throw new Exception()
}
*/

def foo() {// в Groovy этого делать ненужно
    // do stuff
    throw new Exception("Foo Exception")
}

List log = []

try {
  foo()  
} catch( Exception e ) {
    log << e.message
} finally {
    log << 'finally'
}

println log // [Foo Exception, finally]

// Java 7 introduced a multi catch syntax
try {
    // do stuff
} catch( FileNotFoundException | NullPointerException e ) {
    println e.class.name
    println e.message
} 


//Пример с обработкой исключения
class Account {
    BigDecimal balance = 0.0

    def deposit(BigDecimal amount) {
        if( amount < 0 ) {
            throw new Exception("Deposit amount must be greater than 0")
        }
        balance += amount
    }

    def deposit(List amounts) {
        for( amount in amounts ) {
            deposit(amount)
        }
    }
}

Account checking = new Account()
checking.deposit(10)
println checking.balance//10

try {
    checking.deposit(-1)
} catch( Exception e){
    println e.message
}

println checking.balance

checking.deposit([1,5,10,20,50])
println checking.balance
//10
//Deposit amount must be greater than 0
//96
```


[Вернуться в меню _Control Structures_](#Control-Structures)






## Groovy Truth


[Вернуться в меню _Control Structures_](#Control-Structures)