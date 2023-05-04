# Синтаксис и особенности PHP

```php
// 1. Получение информации о системе из PHP
phpinfo();

// 2.  htmlspecialchars() обеспечивает правильную кодировку "особых" HTML-символов так, чтобы вредоносный HTML или Javascript не был вставлен на вашу страницу. Поле age можем просто преобразовать в int, что автоматически избавит нас от нежелательных символов.
echo htmlspecialchars($_POST['name']); 
echo (int)$_POST['age'];

/* 3. Два эквивлентных варианта печати строк
<?= 'напечатать эту строку' ?>
<?php echo 'напечатать эту строку' ?>
*/

// 4. Если файл содержит только код PHP, предпочтительно опустить закрывающий тег в конце файла. 

// 5. Продвинутое изолирование с использованием условий
/*<?php if ($expression == true): ?>
  Это будет отображено, если выражение истинно.
<?php else: ?>
  В ином случае будет отображено это.
<?php endif; ?>
*/

// 6. Пример определения heredoc-строки

$str = <<<EOD
Пример строки,
охватывающей несколько строк,
с использованием heredoc-синтаксиса.
EOD;

/* Более сложный пример с переменными. */
class foo
{
    var $foo;
    var $bar;

    function __construct()
    {
        $this->foo = 'Foo';
        $this->bar = array('Bar1', 'Bar2', 'Bar3');
    }
}

$foo = new foo();
$name = 'Имярек';

echo <<<EOT
Меня зовут "$name". Я печатаю $foo->foo.
Теперь я вывожу {$foo->bar[1]}.
Это должно вывести заглавную букву 'A': \x41
EOT;

// Nowdoc - это то же самое для строк в одинарных кавычках, что и heredoc для строк в двойных кавычках. Nowdoc похож на heredoc, но внутри него не осуществляется никаких подстановок. Эта конструкция идеальна для внедрения PHP-кода или других больших блоков текста без необходимости его экранирования.

class foo
{
    public $foo;
    public $bar;

    function __construct()
    {
        $this->foo = 'Foo';
        $this->bar = array('Bar1', 'Bar2', 'Bar3');
    }
}

$foo = new foo();
$name = 'Имярек';

echo <<<'EOT'
Меня зовут "$name". Я печатаю $foo->foo.
Теперь я печатаю {$foo->bar[1]}.
Это не должно вывести заглавную 'A': \x41
EOT;


// 7. Использование фигурных скобок для правильного указания переменной

$juice = "apple";

// Некорректно. 
echo "He drank some juice made of $juices.";

// Корректно. Строго указан конец имени переменной с помощью скобок:
echo "He drank some juice made of ${juice}s.";


// 8. Объявление типа возвращаемого значения

function sum($a, $b): float {
    return $a + $b;
}

// Обратите внимание, что будет возвращено число с плавающей точкой.
var_dump(sum(1, 2));


// 9.Использование instanceof 

// Пример #1 
class MyClass
{
}

class NotMyClass
{
}
$a = new MyClass;

var_dump($a instanceof MyClass); 
var_dump($a instanceof NotMyClass);

// Выведет
// bool(true)
// bool(false)


// Пример 2 - наследуемые классы 
class ParentClass
{
}

class MyClass extends ParentClass
{
}

$a = new MyClass;

var_dump($a instanceof MyClass);
var_dump($a instanceof ParentClass);

// Выведет
// bool(true)
// bool(true)

// Пример 3 - интерфейсы
interface MyInterface
{
}

class MyClass implements MyInterface
{
}

$a = new MyClass;

var_dump($a instanceof MyClass);
var_dump($a instanceof MyInterface);

// Выведет
// bool(true)
// bool(true)


// 10. Массивы

$array = array(
    "foo" => "bar",
    "bar" => "foo",
);

// Использование синтаксиса короткого массива
$array = [
    "foo" => "bar",
    "bar" => "foo",
];

// Индексированные массивы без ключа
$array = array("foo", "bar", "hallo", "world");


// Доступ к элементам массива
$array = array(
    "foo" => "bar",
    42    => 24,
    "multi" => array(
         "dimensional" => array(
             "array" => "foo"
         )
    )
);

var_dump($array["foo"]);
var_dump($array[42]);
var_dump($array["multi"]["dimensional"]["array"]);


// Разыменование массива
function getArray() {
    return array(1, 2, 3);
}

$secondElement = getArray()[1];

// или так
list(, $secondElement) = getArray();


// Создание/модификация с помощью синтаксиса квадратных скобок

$arr = array(5 => 1, 12 => 2);

$arr[] = 56;    // В этом месте скрипта это
                // то же самое, что и $arr[13] = 56;

$arr["x"] = 42; // Это добавляет к массиву новый
                // элемент с ключом "x"

unset($arr[5]); // Это удаляет элемент из массива

unset($arr);    // Это удаляет массив полностью


// Создаём простой массив.
$array = array(1, 2, 3, 4, 5);
print_r($array);

// Теперь удаляем каждый элемент, но сам массив оставляем нетронутым:
foreach ($array as $i => $value) {
    unset($array[$i]);
}
print_r($array);

// Добавляем элемент (обратите внимание, что новым ключом будет 5, вместо 0).
$array[] = 6;
print_r($array);

// Переиндексация:
$array = array_values($array);
$array[] = 7;
print_r($array);

// Результат выполнения

// Array
// (
//     [0] => 1
//     [1] => 2
//     [2] => 3
//     [3] => 4
//     [4] => 5
// )
// Array
// (
// )
// Array
// (
//     [5] => 6
// )
// Array
// (
//     [0] => 6
//     [1] => 7
// )



$b = array();
$b[] = 'a';
$b[] = 'b';
$b[] = 'c';

// $b будет array('a', 'b', 'c')


// Изменение элемента в цикле
$colors = array('red', 'blue', 'green', 'yellow');

foreach ($colors as &$color) {
    $color = strtoupper($color);
}
unset($color); /* это нужно для того, чтобы последующие записи в
$color не меняли последний элемент массива */

print_r($colors);

// Сортировка массива
sort($files);
print_r($files);

// Использование ссылки при копировании массива
$arr1 = array(2, 3);
$arr2 = $arr1;
$arr2[] = 4; // $arr2 изменился,
             // $arr1 всё ещё array(2, 3)

$arr3 = &$arr1;
$arr3[] = 4; // теперь $arr1 и $arr3 одинаковы



// 11. Callback функции

// Пример callback-функции 

// Пример callback-функции
function my_callback_function() {
    echo 'Привет, мир!';
}

// Пример callback-метода
class MyClass {
    static function myCallbackMethod() {
        echo 'Привет, мир!';
    }
}

// Тип 1: Простой callback
call_user_func('my_callback_function');

// Тип 2: Вызов статического метода класса
call_user_func(array('MyClass', 'myCallbackMethod'));


// Пример callback-функции с использованием замыкания 

// Наше замыкание
$double = function($a) {
    return $a * 2;
};

// Диапазон чисел
$numbers = range(1, 5);

// Использование замыкания в качестве callback-функции
// для удвоения каждого элемента в нашем диапазоне
$new_numbers = array_map($double, $numbers);

print implode(' ', $new_numbers);

// Результат выполнения данного примера:
// 2 4 6 8 10





// 12. Итераторы
// Пример использования iterable в качестве параметра с значением по умолчанию

function foo(iterable $iterable = []) {
    foreach ($iterable as $value) {
        // ...
    }
}


// Пример использования iterable в качестве возвращаемого значения генератора 
function gen(): iterable {
    yield 1;
    yield 2;
    yield 3;
}




// 13. Генераторы

function gen_one_to_three() {
    for ($i = 1; $i <= 3; $i++) {
        // Обратите внимание, что $i сохраняет своё значение между вызовами.
        yield $i;
    }
}

$generator = gen_one_to_three();
foreach ($generator as $value) {
    echo "$value\n";
}

// Для получения null нужно вызвать "yield" без аргументов. 


// Получение пар ключ/значение
/* $input содержит пары ключ/значение разделённые точкой с запятой */

$input = <<<'EOF'
1;PHP;Любит знаки доллара
2;Python;Любит пробелы
3;Ruby;Любит блоки
EOF;

function input_parser($input) {
    foreach (explode("\n", $input) as $line) {
        $fields = explode(';', $line);
        $id = array_shift($fields);

        yield $id => $fields;
    }
}

// array_shift() извлекает первое значение массива array и возвращает его, 
// сокращая размер array на один элемент

// explode — Разбивает строку с помощью разделителя

foreach (input_parser($input) as $id => $fields) {
    echo "$id:\n";
    echo "    $fields[0]\n";
    echo "    $fields[1]\n";
}

// Результат выполнения данного примера:

// 1:
//     PHP
//     Любит знаки доллара
// 2:
//     Python
//     Любит пробелы
// 3:
//     Ruby
//     Любит блоки



// 14. Вывод времени
date_default_timezone_set('Europe/Moscow');
echo date('H:i:s');



// 15. Трейты

// Пример использования трейта

trait ezcReflectionReturnInfo {
    function getReturnType() { /*1*/ }
    function getReturnDescription() { /*2*/ }
}

class ezcReflectionMethod extends ReflectionMethod {
    use ezcReflectionReturnInfo;
    /* ... */
}

class ezcReflectionFunction extends ReflectionFunction {
    use ezcReflectionReturnInfo;
    /* ... */
}


// Пример использования трейта

class Base {
    public function sayHello() {
        echo 'Hello ';
    }
}

trait SayWorld {
    public function sayHello() {
        parent::sayHello();
        echo 'World!';
    }
}

class MyHelloWorld extends Base {
    use SayWorld;
}

$o = new MyHelloWorld();
$o->sayHello();



// Пример использования нескольких трейтов

trait Hello {
    public function sayHello() {
        echo 'Hello ';
    }
}

trait World {
    public function sayWorld() {
        echo 'World';
    }
}

class MyHelloWorld {
    use Hello, World;
    public function sayExclamationMark() {
        echo '!';
    }
}

$o = new MyHelloWorld();
$o->sayHello();
$o->sayWorld();
$o->sayExclamationMark();



// 16. Обработка ошибок

// Отслеживание и отображение ошибок
error_reporting(E_ALL);
ini_set('display_errors', 1);


// 17. Область видимости


// 1) Область видимости свойства

/**
 * Определение MyClass
 */
class MyClass
{
    public $public = 'Public';
    protected $protected = 'Protected';
    private $private = 'Private';

    function printHello()
    {
        echo $this->public;
        echo $this->protected;
        echo $this->private;
    }
}

$obj = new MyClass();
echo $obj->public; // Работает
echo $obj->protected; // Неисправимая ошибка
echo $obj->private; // Неисправимая ошибка
$obj->printHello(); // Выводит Public, Protected и Private


/**
 * Определение MyClass2
 */
class MyClass2 extends MyClass
{
    // Мы можем переопределить общедоступные и защищённые свойства, но не закрытые
    public $public = 'Public2';
    protected $protected = 'Protected2';

    function printHello()
    {
        echo $this->public;
        echo $this->protected;
        echo $this->private;
    }
}

$obj2 = new MyClass2();
echo $obj2->public; // Работает
echo $obj2->private; // Неопределён
echo $obj2->protected; // Неисправимая ошибка
$obj2->printHello(); // Выводит Public2, Protected2, Undefined



// 2) Область видимости метода

/**
 * Определение MyClass
 */
class MyClass
{
    // Объявление общедоступного конструктора
    public function __construct() { }

    // Объявление общедоступного метода
    public function MyPublic() { }

    // Объявление защищённого метода
    protected function MyProtected() { }

    // Объявление закрытого метода
    private function MyPrivate() { }

    // Это общедоступный метод
    function Foo()
    {
        $this->MyPublic();
        $this->MyProtected();
        $this->MyPrivate();
    }
}

$myclass = new MyClass;
$myclass->MyPublic(); // Работает
$myclass->MyProtected(); // Неисправимая ошибка
$myclass->MyPrivate(); // Неисправимая ошибка
$myclass->Foo(); // Работает общедоступный, защищённый и закрытый


/**
 * Определение MyClass2
 */
class MyClass2 extends MyClass
{
    // Это общедоступный метод
    function Foo2()
    {
        $this->MyPublic();
        $this->MyProtected();
        $this->MyPrivate(); // Неисправимая ошибка
    }
}

$myclass2 = new MyClass2;
$myclass2->MyPublic(); // Работает
$myclass2->Foo2(); // Работает общедоступный и защищённый, закрытый не работает

class Bar
{
    public function test() {
        $this->testPrivate();
        $this->testPublic();
    }

    public function testPublic() {
        echo "Bar::testPublic\n";
    }

    private function testPrivate() {
        echo "Bar::testPrivate\n";
    }
}

class Foo extends Bar
{
    public function testPublic() {
        echo "Foo::testPublic\n";
    }

    private function testPrivate() {
        echo "Foo::testPrivate\n";
    }
}

$myFoo = new Foo();
$myFoo->test(); // Bar::testPrivate
                // Foo::testPublic


// 3) Область видимости констант

/**
 * Объявление класса MyClass
 */
class MyClass
{
    // Объявление общедоступной константы
    public const MY_PUBLIC = 'public';

    // Объявление защищённой константы
    protected const MY_PROTECTED = 'protected';

    // Объявление закрытой константы
    private const MY_PRIVATE = 'private';

    public function foo()
    {
        echo self::MY_PUBLIC;
        echo self::MY_PROTECTED;
        echo self::MY_PRIVATE;
    }
}

$myclass = new MyClass();
MyClass::MY_PUBLIC; // Работает
MyClass::MY_PROTECTED; // Неисправимая ошибка
MyClass::MY_PRIVATE; // Неисправимая ошибка
$myclass->foo(); // Выводятся константы public, protected и private


/**
 * Объявление класса MyClass2
 */
class MyClass2 extends MyClass
{
    // Публичный метод
    function foo2()
    {
        echo self::MY_PUBLIC;
        echo self::MY_PROTECTED;
        echo self::MY_PRIVATE; // Неисправимая ошибка
    }
}

$myclass2 = new MyClass2;
echo MyClass2::MY_PUBLIC; // Работает
$myclass2->foo2(); // Выводятся константы public и protected, но не private



// 18. Работа с json

// Вывод json в форматированном виде
echo '<pre>';
print_r($data);



// 19. Проверка условия

$a = null;
//Если $a пустое, то...
if (empty($a)) echo 'Верно!'; else echo 'Неверно!'; //выведет 'Верно!'

$a = 0;
//Если $a пустое, то...
if (empty($a)) echo 'Верно!'; else echo 'Неверно!'; //выведет 'Верно!'

$a = '';
//Если $a пустое, то...
if (empty($a)) echo 'Верно!'; else echo 'Неверно!'; //выведет 'Верно!'

$a = 'hi';
//Если $a пустое, то...
if (empty($a)) echo 'Верно!'; else echo 'Неверно!'; //выведет 'Неверно!', так как $a не пустая


$a = 'hello';

//Если $a существует, то...
if (isset($a)) echo 'Верно!'; else echo 'Неверно!';

/*
Выведет 'Верно!', так как $a существует.
*/



$a = 0;

//Если $a существует, то...
if (isset($a)) echo 'Верно!'; else echo 'Неверно!';

/*
Выведет 'Верно!', так как $a существует.
*/


/*
        Пусть переменную $a вообще не определяли выше в коде
        (это все равно, что присвоить ей null).
        Если $a существует, то...
*/

if (isset($a)) echo 'Верно!'; else echo 'Неверно!'; //выведет 'Неверно!'



$a = true;

//Если $a равно true, то...
if ($a == true) echo 'Верно!'; else echo 'Неверно!';

/*
    Выведет 'Верно!', так как $a равно true.
*/


$a = true;

//Если $a равно true, то...
if ($a) echo 'Верно!'; else echo 'Неверно!'; //выведет 'Верно!', так как $a равно true


//Данное выражение всегда будет выводить 'Верно'
if (true) echo 'Верно!'; else echo 'Неверно!';

//Данное выражение всегда будет выводить 'Верно'
if (!false) echo 'Верно!'; else echo 'Неверно!';



// Вложенные условия

if (empty($a)) { //если переменная $a пуста
    echo 'Введите $a!';
} else { //если переменная $a НЕ пуста
    if ($a > 0) { //спрашиваем, больше ли нуля переменная $a
        echo 'Больше нуля!'; 
    } else {
        echo 'Меньше нуля!'; 
    }
}

//Решение предыдущей задачи через конструкцию elseif
if (empty($a)) {
    echo 'Введите $a!';
} elseif ($a > 0) { //одновременно выполняется else для empty($a) и спрашивается больше ли $a нуля
    echo 'Больше нуля!';
} else {
    echo 'Меньше нуля!';
}



// 20. Циклы

foreach ($arr as $elem) {
    echo $elem; //выведет: '1', '2', '3' и так далее...
}


foreach ($arr as $elem): //обратите внимание на двоеточие!
    echo $elem; //выведет: '1', '2', '3' и так далее...
endforeach;


// Если нам необходимо выполнить несколько команд в круглых скобках - указываем их через запятую

for ($i = 0, $j = 2; $i < 10; $i++, $j++,  $i = $i + $j) {}


// ceil, floor - функции округляют в большую или меньшую сторону до целого


mt_rand(5, 15); // сгенерирует случайное число от 5 до 15



// Функция htmlspecialchars позволяет вывести теги в браузер так, чтобы он не считал их командами, а выводил как строки.
// В данном примере теги выведутся на экран, а не преобразуются в команды браузера

echo htmlspecialchars('<b>жирный текст</b>');


// Функция range создает массив с диапазоном элементов
range('a', 'e'); // ['a', 'b', 'c', 'd', 'e']

range(5, 1); // [5, 4, 3, 2, 1]



// Функции

//Функция date выводит текущие дату и время в заданном формате. 

//Функция strtotime - это аналог функции mktime (тоже возвращает timestamp), только в отличие от нее принимает дату в более свободном формате.

//К примеру, я могу передать ей строку '2025-12-31' и функция сама разберет, где тут год, где месяц, а где день, и вернет эту дату в формате timestamp.

//Что можно делать еще: можно написать так - strtotime('now') - и мы получим текущий момент времени, или так - strtotime('next Monday') - и мы получим следующий понедельник (Monday по-английски 'понедельник'). 
```

[Назад](./)
