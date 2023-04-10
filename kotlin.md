# Синтаксис и особенности Kotlin 

```kotlin
// Основная функция...
fun main(args: Array<String>) {

}


// Объявление и инициализация переменной...
var age: Int = 27

// Сдвиг влево...
var age2 = 30 shl 1

// Сдвиг другого типа...
var age3 = 30 ushr 1

// Объявление символьной переменной...
var gender = 'm'

// Объявление константы для чтения...
const val maxAge = 150


// Операция прибавления числа к элементу...
age += 1


// Вывод переменной...
println(age)

// Вывод строки...
println("Hello world!")


// Объявление переменной, позволяющее содержать любой тип...
var anyVariable: Any

anyVariable = "10"
anyVariable = 5
anyVariable = '4'


// Остаток от деления...
println(50%3)


// Условный оператор...
if (age === age2)
   println("Не ссылаются на один объект")


// Аналог switch...
val result = when(age) {
  6, 18, 21 -> {
    println("New stage")
  }
  // Интервал значений...
  in 17.. 27 -> println("Your age between 17 and 27")

  // Противоположный интервал...
  !in 17.. 27 -> println("Your age less than 17 or more than 27")
   
  else -> println("Else condition")
}


// Цикл с одним переменной счетчика...
for (i in 1..10)
   print("${i*i} \t")

// Цикл с двумя переменными счетчика...
for (i in 1..10) {
   for (j in 1..10)
       print("${i * j} \t")
   println();
}


// Цикл с предусловием...
var i = 10
while (i > 0) {
   println(i)
   i--
}

// Цикл с постусловием...
i = 10
do {
   i--
   println(i)

   // Выход из цикла...
   break

   // Перейти к следующей итерации...
   continue

   println();
} while (i > 0)


// Генерация чисел и символов в диапозоне...
var range = 1..100
var charRange = 'a'..'z'

// Обратная последовательность...
var rangeBack = 1000 downTo 100

// Последовательность с шагом...
var intRangeStep = 1..100 step 3

// Последовательность, не включающая границу...
range = 1 until 100

// Проверка нахождения числа в последовательности...
var isRange = 5 in range
println("Is range: $isRange")

// Создание массива...
val mass: Array<Int> = arrayOf(1, 2, 3, 4, 5)

// Получение элемента массива по индексу...
val n = mass[1]

// Cоздание массива из 5 нулей...
val massZeros = Array(5) {0}

// Создание целочисленного массива...
val intMass = IntArray(7)

// Проверка числа в массиве...
println(4 in mass)


// Создание двумерного массива 3x5, заполненного нулями...
val matrix: Array<Array<Int>> = Array(3) { Array(5, { 0 }) }

// Инициализация первой строки массива...
matrix[0] = arrayOf(1, 2, 3, 4, 5)

// Присовение элементу массива числа...
matrix[0][4] = 21


// Обход одномерного массива и вывод элементов...
for (element in mass)
   println(element)

// Обход двумерного массива и вывод элементов...
for (row in matrix) {
   for (cell in row)
       print("$cell ")
   println()
}


// Присвоение переменной значение функции и передача переменной в строку вывода...
var f = factorial(4)
println("Factorial off 4 = ${factorial(4)}")
println()
println("Factorial off 4 = $f")

// Перегрузка функции, для нескольких параметров...
printStrings("str1")
printStrings("str1", "str2")
printStrings("str1", "str2", "str3")


val names = arrayOf("Tom", "Bob", "Alice")

// Для передачи массива в функцию используется знак *
printNames(3, *names)

// Перегрузка функции, для разных типов параметров...
println("Square of 17 is ${square(17)}")
println("Square of 3.5 is ${square(3.5f)}")


// Функция принимает переменное число параметров...
fun printStrings(vararg strings: String){
    for (str in strings)
        println(str)
}

// Функция принимает и возвращает целое число...
fun factorial2(n: Int): Int {
  var result = 1

  for (d in result..n)
      result *= d

  return result
}

// Функция принимает в начале определенное количество параметров, затем неопределенное...
fun printNames(count: Int, vararg names: String){
  println("Count: $count")
  for (name in names)
      print("$name ")
}

// Однострочная функция...
fun square(x: Int) :Int = x*x

// Перегрузка функции square...
fun square(x: Float) = x*x


// Lambda function, printing message...
val msg = { println("Test message") }
msg()

// Run lambda expression...
run { println("Second message") }

// Example of getting params from lambdas and sum 2 params in 1 line...
val sum = {x: Int, y: Int -> println(x + y)}
sum(5,7)

// More complicated lambda expression for summing.
// Showing params and it's sum and returning result of summing...
val sum = {x: Int, y: Int ->
  val res = x + y
  println("$x + $y = $res")
  res
}
val a = sum(2,5)
println("a = $a")


// Example of using high order functions...
val add = {x: Int, y: Int -> x + y}
val mul = {x: Int, y: Int -> x * y}

action(5,3, add)
action(45,77,mul)
action(50,25) { x: Int, y: Int -> x - y }

fun action(n1: Int, n2: Int, operation: (Int,Int) -> Int) {
  val res = operation(n1, n2)
  println(res)
}


// Using anonumous functions(preferable lambdas, more laconic)...
operation(9, 10, fun(x: Int, y: Int): Int = x + y)
operation(15, 20, fun(x: Int, y: Int): Int {
  return x * y
})

fun operation(x:Int, y:Int, op:(Int,Int)->Int){
    val result = op(x,y)
    println(result)
}


// Getting key to chose operation and then pass parameters(9 + 4)...
var action = selectionKey(1)
println(action(9,4))

fun selectionKey(key: Int): (Int, Int)->Int{
    when(key){
        1 -> return {x:Int, y:Int -> x + y}
        2 -> return {x:Int, y:Int -> x - y}
        3 -> return {x:Int, y:Int -> x * y}
        else -> return {x:Int, y:Int -> 0}
    }

}



// Example of using class.
// Use modifier open to extends from other class...
open class Person {
  private var name: String = ""
  private var age: Int = 0

  constructor(_name:String) {
    name = _name
  }

  constructor()

  constructor(_name:String, _age:Int): this(_name){
    age = _age
  }

  val getName: String get() = name.toUpperCase()
  val info: String get() = "Name: $name  Age: $age"

  open fun setName(value: String){
    if(value.length > 2)
      name = value
  }

  open fun setAge(value: Int){
    if((value > 0) and (value < 150))
      age = value
  }

  open fun sayMyName(){
    println("My name is $name")
  }
}


// Extends class Person...
class Employee: Person
{
  var company: String = ""

  constructor(name:String, comp:String):super(name){
    company = comp
  }

  override fun sayMyName(){
    println("My name is $getName. I work in $company")
  }
}


fun main(args: Array<String>) {
  val nick: Person = Person()
  nick.setName("Nick")
  println(nick.getName)
  println(nick.info)
  nick.setAge(20)
  println(nick.info)

  val bob: Person = Person("Bob")
  bob.sayMyName()
  println(bob.info)
  bob.setAge(27)
  println(bob.info)
  val alice: Person = Person("Alice",19)
  println(alice.info)
}


// Create interface and using or override methods in other classes...
interface Movable{
  fun move()
  fun stop(){
    println("Stop")
  }
}

class Car: Movable{
  override fun move() {
    println("Car is moving")
  }
}

class Aircraft: Movable{
  override fun move() {
    println("Aircraft is moving")
  }
  override fun stop(){
    println("Landing")
  }
}


// Create abstract class for implementing and using for other classes...
abstract class Human(val name:String)


// Using enums...
enum class DayOfWeek{Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday}

val today: DayOfWeek = DayOfWeek.Saturday
print("Today: $today")
```

[Назад](./)

