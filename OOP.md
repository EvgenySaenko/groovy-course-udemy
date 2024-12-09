[Главная](README.md)

# OOP

+ [Classes Fields Local Variables](#Classes-Fields-Local-Variables)
+ [Constructors and Methods](#Constructors-and-Methods)
+ [Наследование классов](#Наследование-классов)
+ [Интерфейсы](#Интерфейсы)
+ [Traits](#Traits)
+ [GroovyBeans](#GroovyBeans)









## Classes Fields Local Variables

```groovy

class Person {//public default

    String firstName, lastName //default private
    def dob
    // private | protected | public
    protected String f1,f2,f3
    private Date createdOn = new Date() 
    
    static welcomeMsg = 'HELLO'//Person.welcomeMsg -  к статик полям получаем так же доступ как в Java
    public static final String WELCOME_MSG = 'HELLO'    

    // local variables
    def foo() {
        String msg = "Hello"
        String firstName = "Dan"// одинаковое название не будет конфликтовать с полем класса потому что это локальная перменная(область действия тело метода)
        println "$msg, $firstName"
    }
    
    

}

def person = new Person()
println person.foo()
```


[Вернуться в меню _OOP_](#OOP)






## Constructors and Methods

+ в классе конструктор по умолчанию, если мы не создали создается по умолчанию
+ можно опускать ключевое слово `this` в конструкторе
+ чтобы не создавать под каждую ситуацию конструткор с нужным набором полей, мы можем использовать такую запись 
`new Employee(firstName: "Dan", secondName: "Vega")` указывая название поля которое хотим заполнить `firstName:`

```groovy
@ToString()
class Employee {
    String firstName
    String secondName

}
def employee1 =  new Employee()//в груви как и в Java создается конструктор по умолчанию, если мы не указали свой

//в Groovy не обязательно создавать разный набор конструкторов под каждую ситуацию, можно писать так
def employee2 = new Employee(firstName: "Dan", secondName: "Vega")

```

+ методы по умолчанию public
+ return можно не писать, в Groovy если метод возвращает какой-то тип, последняя строка метода выполнит return
+ можно не указывать конкретный типо возвращаемый методом объявив как `def`
+ если мы знаем, что конкретно(какой тип, объект или коллекцию) будем получать или возвращать, то лучше это указать
+ так же мы можем задать default значение переменной которую передаем в метод
+ можем передавать аргумент переменной длины, так называемый `var arg` 

```groovy
@groovy.transform.ToString
class Person {
    
    String first,last

    // constructor
    Person(String fullName) {
        List parts = fullName.split(' ')
        first = parts[0]
        last = parts[1]
    }
    
    // methods
    public void foo(String a, String b) {
        // do stuff
    }
    
    String getFullName(){
        "$first $last" //можно не писать return
    }
    
    def bar(){// можно не указывать конкретный тип
    
    }
    
    static String doGoodWork(){//статический метод, вызывается Person.doGoodWork()
        println "doing good work..."
    }

    //можем не указывать тип, и передевать что угодно - от числа, строки, заканчивая коллекцией
    def someMethod(item) {
        
    }
    
    //но если мы знаем, что будем возвращать и принимать(тип, объект, коллекцию), лучше это указать 
    List someMethod(List numbers = [1,2,3], Boolean canAccessAll = false ){//можем задать default значение переменным 
    
    }
    
    def concat(String... args) {
        println args.length
    }

}

// Person p = new Person(first:'Dan',last:'Vega')
// println p

//Person p = new Person("Dan Vega")
//println p

Person p = new Person("Dan Vega")
p.concat('a','b','c','d')
```

[Вернуться в меню _OOP_](#OOP)





## Наследование классов

> В Groovy наследование классов такое же как и в Java и служит для чистоты кода, 
> и переиспользования полей или методов верхнеуровневого класса, что уменьшает дублирование кода
> в классе наследнике

```groovy
class Phone {

    String name
    String os
    String appStore

    def powerOn(){}

    def powerOff(){}

    def ring(){}
}


@groovy.transform.ToString()
class IPhone extends Phone {

    String iosVersion

    def homeButtonPressed(){}

    def airPlay(){}

    def powerOn(){}
}
```


[Вернуться в меню _OOP_](#OOP)





## Интерфейсы

> Интерфейс в Groovy как и в Java это так называемый контракт
> https://groovy-lang.org/objectorientation.html#interfaces

+ содержит абстрактные методы, в которых определен возвращаемый тип и название метода
он не реализует логику метода
+ в Groovy один интерфейс может наследовать другой интерфейс
+ при помощи ключевого слово default можно создать метод с реализацией как в Java

```groovy

interface IPersonService {

    Person find()

    List<Person> findAll()

}

class PersonService implements IPersonService {

    @Override
    Person find() {
        Person p = new Person(first:"Dan",last:"Vega")
        return p
    }

    @Override
    List<Person> findAll() {
        Person p1 = new Person(first:"Dan",last:"Vega")
        Person p2 = new Person(first:"Joe",last:"Vega")
        [p1,p2]
    }
}


PersonService personService = new PersonService()

println personService.find()
```

[Вернуться в меню _OOP_](#OOP)





## Traits

>__Traits__ документация ниже для более подробного изучения
> 
>https://docs.groovy-lang.org/docs/groovy-2.3.0/html/documentation/core-traits.html

__Traits__ — это структурная конструкция языка, которая позволяет:

+ состав поведения
+ реализация интерфейсов во время выполнения
+ поведение, переопределяющее
+ совместимость с проверкой/компиляцией статического типа

>Их можно рассматривать как интерфейсы , несущие как реализации по умолчанию ,
> так и состояние . Признак определяется с помощью trait ключевого слова:
```groovy
trait FlyingAbility {                           
        String fly() { "I'm flying!" }

    //приватный метод не обязательно переопределять в классе который наследует интерфейс
    private String bar() {
        "bar"
    }
}

trait SpeakingAbility {
    public String a
    private String b

    String speak(){
        "I'm Speaking!"
    }
}


class Bird implements FlyingAbility, SpeakingAbility {

    @Override
    String foo() {
        return null
    }
}
//мы так же можем переопределить методы в самом классе Bird
def b = new Bird()
assert b.fly() == "I'm flying!" 
assert b.speak() == "I'm Speaking!"
```

>Трейты также могут определять частные методы. Эти методы не будут отображаться в интерфейсе контракта типажа:

```groovy
trait Greeter {
    private String greetingMessage() {
        'Hello from a private method!'
    }
    String greet() {
        def m = greetingMessage()
        println m
        m
    }
}
class GreetingMachine implements Greeter {}
def g = new GreetingMachine()
assert g.greet() == "Hello from a private method!"
try {
    assert g.greetingMessage()
} catch (MissingMethodException e) {
    println "greetingMessage is private in trait"
}
```

[Вернуться в меню _OOP_](#OOP)




## GroovyBeans


[Вернуться в меню _OOP_](#OOP)

>Тут показана разница между Java и Groovy бинами

> В Groovy все чище смотрится, если мы нажмем в IDEA build-> recompile или compile Employee,
> то мы увидим, что Groovy после компиляции все это делается под капотом, поля приватные, конструктор без аргументов, гетеры и сеттеры, ту стринг.
 
+ если мы хотим какой-то особенный гетер сеттер - можно прописать
+ можно и конструкторы писать разные, но зачем
+ если хотим обратиться на прямую к полю не вызывая геттер 

```groovy
//Java class
public class EmployeeBean implements Serializable {
    // private properties
    private String first;
    private String last;
    private String email;
    
    // public no-arg constructor
    public EmployeeBean(){
    
    }
    // getters & setters
    public String getFirst() {
        return first;
    }
    
    public void setFirst(String first) {
        this.first = first;
    }
    
    public String getLast() {
        return last;
    }
    
    public void setLast(String last) {
        this.last = last;
    }
    
    public String getEmail() {
        return email;
    }
    
    public void setEmail(String email) {
        this.email = email;
    }
    
    // toString
    
    @Override
    public String toString() {
        return "EmployeeBean{" +
        "last='" + last + '\'' +
        ", first='" + first + '\'' +
        '}';
    }
}

//Groovy class
@groovy.transform.ToString
class Employee implements Serializable {

    String first,last,email

    String fullName

    void setFullName(String name){
        fullName = name
    }

    String getFullName(){
        "Full Name: ${fullName}"
    }

}


DoubleBean db = new DoubleBean()
db.value = 100

println db.value//груви использует геттер под капотом
println db.@value//не использует геттер - обращение на прямую к полю
``` 






## Classes Fields Local Variables


[Вернуться в меню _OOP_](#OOP)



## Classes Fields Local Variables


[Вернуться в меню _OOP_](#OOP)