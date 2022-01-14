# RUBY HELPEX
[На головну](../../README.md)

## Проки, блоки і лямбди
* **block** - конструкция языка
  * можно передать в качестве аргумента
  * в качестве аргумента - можно передать только 1шт
  * записать в переменную или сохранить нельзя
* **proc** - объект
  * можно сохранить в переменную
  * можно передать в качестве аргумента
  * в качестве аргумента - можно передавать больше 1шт
  * элемент класса `Proc`
  * не проверяют кол-во элементов, которые в него передаются
  * `return` завершает выполнение кода прока и кода метода где вызван
* **lambda** - объект
  * элемент класса `Proc`
  * проверяет кол-во элементов, которые в нее передаются
  * `return` завершает выполнение кода лямбды, код метода где вызвана - продолжает выполняться

## Області видимості змінних і констант
* `var` - локальна змінна
  * доступна тільки в області видимості, де була об'явлена
* `@var` - інстанс змінна
  * доступна в экземплярі і вкладених областях видимості
* `@@var` - класова змінна
  * доступна в класі
  * доступна в класових методах
  * і у всіх інстанс-методах классу
  * також в наслідниках класу
* `$var` - глобальна змінна, доступна у всій программі
* `CONST` - константа область видимості
  * клас + потомки - `A::ABC`
  * модуль - `M::ABC`, а також через вкладені модулі `M::M2::ABC` 
  * якщо модуль `prepend`/`include` в класс, то видно через:
    * клас `A::ABC`
    * потомки класу `B:ABC`
    * через модуль `M:ABC`
* `@var` в контексті класу - класова інстанс змінна
  * не буде доступна всередині наслідників
  * не буде доступна всередині екземплярів
  * буде доступна в класових методах
  * не буде доступна в інстанс методах

## Доступність методів
* **public**
  * по умолчанию
  * открытые методы - могут вызываться из любых других объектов
* **private**
  * доступны только в классе
  * также доступны наследникам класса
  * также доступны из `included` модуля и классу и наследникам
  * не могут вызываться из объектов
* **protected**
  * вызывать можно в классе и в экземпляре
  * при вызове метода, вызывающий класс объекта должен быть того же класа или потомком
    * можно вызывать на потомках
    * можно вызывать на самом классе
    * если в классе - ребенок этого же класа, можно вызывать `protected` методы класса в ребенке

## Шукання методів
* йде по ієрархії, доходить до `basicObject`
* потім вертається
* потім друге коло - йде знов і шукає `method_missing` по всій ієрархії

## Модулі
* **основная инфа**
  * це структура даних, яка дозволяє реалізовувати неймспейси і міксіни
  * Совокупность методов и констант
  * Методы модуля
    * могут быть методами экземпляра или методами модуля
    * когда модуль включен, меторды экземпляра появляются как методы класса, а методы модуля - нет
    * когда модуль включен, методы модуля могут вызываться без создания инкапсулирующего объекта
  * нужны для
    * **пространства имен**
    * **миксинов** - общие методы для нескольких классов или модулей
  * создается с помощью `module`
  * может быть повторно открыт для добавление добавления, изменения, удаления функционала
  * екземпляр модуля:
    * не можна створювати, але є один прикол:
    * Lol = Module.new - вот і екземпляр 
    * далі вже не можна створювати інстанси

## Класи
* **основная инфа**
  * клас - екземпляр класу class 
  * клас - структура даних, містить в одному місці і дані і методи обробки даних
* **створення класу**
  * створюється безіменний екземпляр класу Class з нашим описаловом
  * створюється константа
    * те що ми вказуємо в якості назви нашого класу
    * їй присвоюється цей безіменний екземпляр (об'єкт) класу Class 
  * коли створюємо екземпляр нашого класу - то це ми створюємо екземпляр цього безіменного класу
* **динамічне створення методів і змінних, `attr_accessor`**
  * `define_method(name) { method body }` - створює новий метод в класі
  * `send(name)` - виконує метод класу
  * `send_public(name)` - виконує публічний метод класу
  * `instance_variable_get(name)` - повертає значення змінної
  * `instance_variable_set(name, val)` - створює змінну і пише значення в неї
* class << self
  * дает доступ к методам `singleton`
* `singleton`
  * Методы экземпляра обычно определяются в классах.
  * Все экземпляры одного класса используют одни и те же методы экземпляров.
  * Класс `singleton` находится между объектом и его классом.
  * Это позволяет каждому экземпляру иметь свой собственный набор методов, независимый от других экземпляров
  * ієєрархія: `myObj` -> `singleton` -> `myClass` -> ...

## модуль vs клас
* з модуля не можна зробити екземпляр
* в модулях немає наслідування
* модулі - можна використовувати для міксінів
* модулі використовуються для неймспейсів

## Extend, prepend, include
* **include**
  * Добавляет в цепочку предков перед текущим классом - дополнительный ближайший предок.
  * Добавляет функционал инстанса
  * ієрархія: # BasicObject -- # Object -- # PlayGuitar (included module) -- # Person
* **prepend**
  * Добавляет после текущего класса - дополнительный ближайший потомок
  * Добавляет функционал инстанса
  * ієрархія: # BasicObject -- # Object -- # Person -- # PlaySax (prepended module)
* **extend**
  * Добавляет функционал в классовые методы

## Символи
* об’єкти скалярного значення, що використовуються як ідентифікатори, що відображають незмінні рядки на фіксовані внутрішні значення
* По суті, це означає, що символи є immutable strings.
* константа, яка не потребує ініціалізації
* пам'ять виділяється до кінця процесу
* Якщо текстовий вміст об’єкта важливий, використовуйте String. Якщо ідентичність об’єкта важлива, використовуйте символ
* використовувати в хешах - в якості ключів
* символи не є вказівниками на значення, вони і є значення

## == === equal? eql?
* **==** - общее сравнение
* **===**
  * специальная тулза, которая придумана для `case`
  * можно описать этот метод в классе, что б потом он заюзался в `case`
  * вопрос "как оно сравнивает?" не корректен, потмоу что в каждом классе реализован по-своему
  * на кастомном классе
    * > def ===(other)
      > 
      >   self == other
      > 
      > end
    * выражение после ключевого слова `case` является правой частью выражения ===
    * выражение после ключевого слова `when` находится в левой части выражения ===
* **equal?** - проверяется идентичность объектов - прям по id
* **eql?** - сравнивает хеши объектов

## is_a?, kind_of?, instance_of? - методи об'єкту (екземпляру)
* **instance_of?** - true, если объект является экземпляром конкретно этого класса
* **is_a?**
  * синоним с `kind_off?`
  * `a.kind_off? A` - true, если `a` - экземпляр класса `A`
  * `b.kind_off? A` - true, если `b` - экземпляр потомка класса `A`
  * `a.kind_off? ModA` true, если `a` - экземпляр класса, в котором `prepend` или `include` модуль `ModA`
  * `b.kind_off? ModA` true, если `b` - экземпляр потомка, в родителе которого `prepend` или `include` модуль `ModA`
* **kind_of?**
  * синоним с `is_a?`
  * работает так же

## Приорітетність операторів
* Приоритет по убыванию
* `!`, `~`, unary `+`
* `**`
* unary `-`
* `*`, `/`, `%`
* `+`, `-`
* `<<`, `>>`
* `&`
* `|`, `^`
* `>`, `>=`, `<`, `<=`
* `<=>`, `==`, `===`, `!=`, `=~`, `!~`
* `&&`
* `||`
* `..`, `...`
* `?`, ':'
* modifier-rescue
* `=`, `+=`, `-=`, etc
* `defined?`
* `not`
* `or`, `and`
* modifier-if, modifier-unless, modifier-while, modifier-until
* `{ }` blocks

## Safe navigation і method_name!
* Safe navigation
  * если текущее звено в цепочке методов = `nil` то дальше не идет
  * `User&.address&.street` - не упадет, если нету address или `User` пустой
* `User.create` vs `User.create!`
  * первое не создаст и вернет `nil`
  * второе не создаст и даст ошибку
* `'abs'.gsub('a','b')` vs `'abc'.gsub!('a', 'b')`
  * первое - не изменит строку, а создаст новую
  * второе - изменить саму строку - мутация

## Оператори
* Присваивания
  * `a += b` -- `a = a + b`
  * `a -= b` -- `a = a - b`
  * `a += b` -- `a = a * b`
  * `a \= b` -- `a = a \ b`
  * `a %= b` -- `a = a % b`
  * `a **= b` -- `a = a**b`
  * > a = 10
    > 
    > b = 20
    > 
    > c = 30
    > 
    > a, b, c = 10, 20, 30
* Побитовые
  * > a = 0011 1100
    > 
    > b = 0000 1101
    > 
    > a&b = 0000 1100
    > 
    > a|b = 0011 1101
    > 
    > a^b = 0011 0001
    > 
    > ~a = 1100 0011
  * `&` - двоичный AND - копирует бит в результат, если он существует в обоих операндах
  * `|` - двоичный OR - копирует бит, если он существует в любом из операндов
  * `^` - двоичный XOR - копирует бит, если он установлен в один операнд, но не в обоих
  * `~` - Binary Ones - оператор дополнения является унарным и имеет эффект «переворачивание» бит
  * `<<` - Двойной левый оператор сдвига.
    * Значение левых операндов перемещается влево на количество бит, заданных правым операндом
    * `a << 2 = 240`, что составляет `1111 0000` 
  * `>>` - Двоичный оператор правого сдвига.
    * Значение левых операндов перемещается вправо на количество бит, заданных правым операндом.
    * `a >> 2 = 15`, что составляет `0000 1111`
* Операторы диапазона
  * `..` - Создает диапазон от начальной до конечной точки включительно. 1..10 -> 1-10
  * `...` - Создает диапазон от начальной точки до конечной точки. 1...10 -> 1-9
* Оператор `defined?`
  * `defined? variable` - True, если переменная инициализирована
  * `defined? method_call` - True, если метод определен
  * `defined? super` - True, если существует метод и он может быть вызван
  * `defined? yield` - True, если передан блок кода
* массивы
  * `<<`
    * добавляет в конец массива элемент, можно цепочку: `a << 1 << 'b' << [3, 4]`
    * еще может быть описан по-разному в разных классах
* `>>`
  * по-разному реализован в разных классах
* `=~`
  * сопоставление регулярному выражению, возвращает `false` или позицию первого вхождения

## BEGIN END, begin end
* `BEGIN { block code }` - блок, который выполняется перед выполнением программы
* `END { block code }` - блок, который выполняется после выполнения программы
* `begin` `end`
  * блок кода
  * не создает новую область видимости
  * в последней строке - результат, который можно присвоить переменной, присвоив ей весь блок

## Виключення
* ключевое слово `rescue` - можно вставлять в блок кода `begin end`
* можно писать несколько `rescue` подряд
* последним можно написать `else` - выполняется только если не было исключений
* после этого еще можно написать `ensure` - этот блок будет выполнен в любом случае
* ключевое слово `retry` - используется в блоке `rescue` - возвращает выполнение на `begin`
* ключевое слово `raise` - чтобы сгенерировать исключение

## Виключення throw catch
* `throw` - генерирует исключение, и когда оно встречается, управление переходит к `catch`
* `catch` - определяет блок, который помечен с данным именем (может быть символ или строка)
* реализовано с помощью меток

## Константи
* Константи - все, що починається з великої літери, або повністю великими літерами
* Назви модулів та класів - теж константи

## dup vs clone
* `clone` копирует класс `singleton`, в то время как `dup` нет
* `clone` сохраняет замороженное состояние, в то время как `dup` нет
* при работе с `ActiveRecord` - `dup` создает новый объект без id, `clone` - копирует с id

## Rack і вся схема роботи
* Rack - це специфікація
  * протокол, як має комунікувати між собою аплікейшн сервер і аплікуха
  * набір описаних правил
  * Rack сумісний сервіс має описувати такий-то метод, такий-то респонс,...
* Rack middleware
  * Rack - в центрі запитів - і якісь маніпуляції робити з цими запитами - це воно і є
  * реалізація шаблону проектування конвеєра для веб-серверів, що юзають Rack
* Rack application приймає:
  * puma прийняла хеш з хттп реквестом і запихнула в хеш env
  * Rack application приймає env
  * Rack application розпарсює env
* Rack application вертає масив з трьома елементами:
  * код відповіді
  * хедери
  * боді
* Puma - це app server
* Nginx - це web server
* Rails - аплікуха, Rack-сумісний сервіс
* ось як все працює:
  * browser - nginx - puma - rack - rails

## Шлях реквесту
* Браузер
* GET-реквест
* Nginx - web server
* Puma - application server
* Rack (+Middleware)
* Rails
  * config.ru
  * application.rb
  * енвайрнменти
  * routes
  * Controller
* Response to Rails
* to Rack
* to Puma
* to Browser

## Rake
* автоматизація процесів
* типу Makefile
* наприклад `rake db:migrate`
* в rake таски можна передавати параметри такими способами:
  * засобами rake - `$ rake add\[1,2\]`
  * environment variable - `$ RAILS_ENV=production bundle exec rake ...`
  * за допомогою ARGV - `$ rake add 1 2`
  * за допомогою Ruby OptionParser (найважчий спосіб) - `$ rake add -- --one 1 --two 2`

## Потоки
* в рубі - зелені потоки
  * мова дає інструменти для роботи з потоками
  * але потоки виконуються послідовно
  * почучуть кожний
  * рубі сам приймає рішення коли пригати між ними
  * нема ніякої паралельності
* GIL - контролює шо б в рубі були зелені потоки
  * Global Interpreter Lock
  * Рубі інтерпретатор написаний на сі, Сі - потоконебезпечний, тому і інтерпретатор - теж
  * Потоконебезпечний - з потоками нема проблем, поки вони не використовують одні й ті самі дані
* Є купа реалізацій Ruby, окрім Сішного: JRuby, TruffleRuby,..
  * і вот наприкдад в JRuby - інтерпретатор на Java - і от там - є реальні потоки і паралельність

## require require_all require_relative
* `require`
  * хочу посилатися та виконувати код, який не записаний у поточному файлі, сторонні файли, gems
  * производит поиск в папках, указанных в `$LOAD_PATH`
* `require_relative`
  * для файлу, який відноситься до поточного файлу (загалом, у тому самому каталозі проекту)
  * производит поиск от текущей папки (файл из которой на данный момент исполняется, в константе `__FILE__`)
* `require_relative`
  * це gem
  * дозволяє працює як `require` але дозволяє конфігурувати на більш високому рівні

## Ітератори
* `each` - біжить по елементам і робить дії в блоці, не трогає об'єкт
* `map` - біжить по елементам і робить дії над елементами об'єкту, повертає новий об'єкт
* `map!` - біжить по елементам і робить дії над елементами об'єкту, змінює існуючий об'єкт
* `collect` - аліас `map`
* `select` - фільтрує елементи, якщо блок вертає `true` елемнт лишається, повертає новий об'єкт
* `select!` - фільтрує елементи, якщо блок вертає `true` елемнт лишається, змінює існуючий об'єкт
* `find` - теж фильтрує, але повертає лише один перший, який задоволняє умові
* `detect` - аліас `find`
* `reduce` - Застосовує бінарну операцію (суму, ділення, ..) до кожного елемента, зберігає значення у змінній
* `inject` - аліас `reduce`

## Різне
* в рубі не є об'єктами: блоки, ключові слова, методи
* передача параметрів в рубі: по ссилці все, крім символів та чисел