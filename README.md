# Code style для iOS-разработчиков

## Содержание

## Язык

Предполагается использование английского языка для именования переменных, методов, классов.

Далее для каждого проекта отдельно команда решает, стоит ли использовать для комментариев, commit messages и документации русский или английский. Определяется язык проекта. При не очень хорошем английском хотя бы у **одного** члена команды, надо всеми использовать русский. Для иностранных проектов использование английского для документации обязательно.
Смотрим в репозиторий, спрашиваем у других, затем пишем commit message на нужном языке. Не нужно писать на ломаном английском в проекте, где все пишут комментарии на русском, а также оставлять непонятные русские комментарии в проекте с иностранным заказчиком.

**Желательно:**
```swift
let myColor = UIColor.white()
```

**Нежелательно:**
```swift
let zagruzkaColor = UIColor.white()
```

## Общее

### Табуляция
Используются отступы из 4 пробелов. Настроить это можно в меню XCode - Preferences - Text Editing - Indentation.

### Операторные скобки
Принято открывающую скобку ставить на той же строке. В конструкциях с цепочками if-else-if `else` не переносится:
```swift
if a > b {
	// do stuff
} else {
	// do another
}
```

### Разделение операторов
Точку с запятой (`;`) не ставить, использовать перенесение на новую строку для разделения операторов.

**Нежелательно:**
```swift
var a = 5; a = 10;
```

**Желательно:**
```swift
var a = 5
a = 10
```

### Объявление переменных
Использовать `let` при объявлении переменных по возможности всегда.

## Именование переменных

* Стараться не сокращать, давать понятные имена, отражающие роль переменной в контексте. 
* Не использовать венгерскую нотацию (например, `kCLLocationManagerFilterNone`), snake case (`like_so`), или macro case (`LIKE_THIS`).
* Опускать ненужные слова:

**Нежелательно:** 
```swift
print(myCart.cartWeight)
print(myCar.cartWeight)
```
**Желательно:** 
```swift
print(myCart.weight)
print(myCar.speed)
```

* Именование переменных должно вести к тому, чтобы вызовы методов складывались в корректные английские фразы:

**Нежелательно:** 
```swift
array.remove(x) // что такое x? Объект или индекс?

x.insert(y, position: z)
x.subViews(color: y)
x.nounCapitalize()
```
**Желательно:** 
```swift
array.remove(at: index)
array.remove(item)

x.insert(y, at: z)          // x, insert y at z
x.subViews(havingColor: y)  // x's subviews having color y
x.capitalizingNouns()       // x, capitalizing nouns
```

* Именование параметров должно выделять единый уровень абстракции:

**Нежелательно:** 
```swift
a.move(toX: b, y: c)
a.fade(fromRed: b, green: c, blue: d)
```
**Желательно:** 
```swift
a.moveTo(x: b, y: c)
a.fadeFrom(red: b, green: c, blue: d)
```

* Необходимо вводить дополнительные метки параметров, если это определяет предназначение:

**Нежелательно:** 
```swift
viewController.dismiss(false) 
words.split(12)
```
**Желательно:** 
```swift
viewController.dismiss(animated: false) 
words.split(maxCount: 12)
```

* Именование мутирующих и не мутирующих функций должны отражаться в их сигнатуре:

```swift
// мутирующие
array.sort()
x.append(y)

// немутирующие
let result = array.sorted()
let z = x.appending(y)
```

* Использовать значения параметров по умолчанию:

```swift
  extension String {
      public func compare(
	 other: String, options: CompareOptions = [],
	 range: Range? = nil, locale: Locale? = nil
      ) -> Ordering
  }
  
  // Желательно:
  let order = lastName.compare(royalFamilyName)
  
  // Нежелательно:
  let order = lastName.compare(royalFamilyName, options: [], range: nil, locale: nil)
```

### Перечислимые типы

Элементы перечислимого типа именовать в camelCase, начиная с маленькой буквы:

```swift
enum Direction {
    case up, down, left, right
}
```

### Протоколы

Имена протоколов должны быть выражены в виде имен существительных, если оно описывает **предназначение** (например, `Collection`), или добавляются суффиксы `-able`, `-ed`, `-ing`, если это описывает **поведение** (например, `Equatable`, `ProgressReporting`).

### Селекторы

Использовать синтаксис вида `#selector(doSomething)`, не включая имя собственного класса (вроде `#selector(MyViewController.doSomething)`).

### Генерики

Указывать не традиционное `T` или `U` в качестве имени обобщенного параметра, а более определенные, вроде `Element` или `Item`. Исключениями здесь могут быть, например, пользовательские операторы, являющиеся очень высокой абстракцией над данными.

## Организация кода

### Структура класса

Класс имеет следующую структуру:

```swift
// MARK: Global operators taking the class as a params
func == (lhs: MyClass, rhs: MyClass) -> Bool { ... }

// MARK: Class
class MyClass: BaseClass {
    
    // MARK: Declarations
    
    // 1.1. Outlets
    @IBOutlet private weak let titleLabel: UILabel!
    
    // [1.1]. Publish subjects and observables [only if Rx is used]
    var citySelected: PublishSubject<City>? = nil
    
    // 1.2. Public properties, calculated and lazy
    var text: String { get { ... } set { ... } }
    
    // 1.3. Private properties
    private let isUsingLasers = true
    
    // MARK: Lifecycle
    
    // 2.1. Initializers
    init?(city: anotherCity) { ... }
    
    // 2.2. Lifecycle
    override func awakeFromNib() { ... }
    override func viewDidLoad() { ... }
    
    // MARK: Public
    func fill(city: City) { ... }
    func getPopulation() -> Int { ... }
    
    // MARK: Private
    private func calculateOverpopulation() { ... }
}

// MARK: Extensions
extension MyClass: Comparable { ... }
extension MyClass: Equitable { ... }
```

* Старайтесь опираться на нее и разделять блоки кода с помощью `// MARK:` для лучшей читабельности.
* Соблюдение протоколов класса должны быть выделены в `extension`-блоки, как в примере выше.
* Убираем ненужные пустые методы вроде `override func viewDidRecieveMemoryWarning() { ... }`.

### Комментарии

В местах, где они нужны, комментарии должны объяснять, зачем некоторый код выполняет свою логику. Любые комментарии должны быть или релевантными и актуальными, или их не должно быть вовсе.

Также не стоит забывать, что комментарии в виде `///` и `/** */` подхватываются IDE и впоследствие видны в панели справа.

### Вычислимые свойства

Если свойство имеет только один аксессор, то лучше его опустить:

**Нежелательно:** 
```swift
var emptyString: String {
    get {
    	return ""
    }
}
```
**Желательно:** 
```swift
var emptyString: String {
    return ""
}
```

### Замыкания

* Пользуемся механизмом автовыведения типов: не указываем тип входных, и, если возможно, выходных параметров:

**Нежелательно:** 
```swift
[1, 2, 3, 4, 5].filter { (a: Int) -> Bool in
    return a > 3
}
```
**Желательно:** 
```swift
[1, 2, 3, 4, 5].filter { a in return a > 3 }
```

* Дополняя пример выше: не вводим именование входных параметров, если возможно, а так же используем неявный `return`:

**Еще желательнее:** 
```swift
[1, 2, 3, 4, 5].filter { $0 > 3 }
```

* Используем синтаксис trailing closures:

**Нежелательно:** 
```swift
a.perform(completion: { result in 
    // ...
})

UIView.animateWithDuration(0.3, completion: { finished in 
    // ... 
})
```
**Желательно:** 
```swift
a.perform { result in 
    // ...
}

UIView.animateWithDuration(0.3) { finished in 
    // ... 
}
```

* При использование цепочки методов, принимающих функции преобразования, код должен быть отформатирован соответствующе:

**Нежелательно:** 
```swift
["my", "gem", "deer"].map { $0.characters.count }.filter { $0 > 2 }.filter { $0 % 2 == 0 }
```
**Желательно:** 
```swift
["my", "gem", "deer"].map { $0.characters.count }
    .filter { $0 > 2 }
    .filter { $0 % 2 == 0 }
```

### Автовыведение типов

Оно есть. И важно про него не забывать. Не указываем тип до тех пор, пока компилятор уж совсем к стенке не прижмет.

**Нежелательно:** 
```swift
var obvslString: String = ""
var obvslArray: [String] = [String]()
var obvslDouble: Double = 14.57
```
**Желательно:** 
```swift
var obvslString = ""
var obvslArray: [String] = []
var obvslDouble = 14.57
var notSoObviouslyFloat: CGFloat = 13.012
```

### Константы

* Локальные константы класса достаточно объявить `let`-конструкцией.
* Общепроектные константы лучше не делать глобальные, а определить в рамках `enum`:

```swift
enum Colors {
    static let CustomRed = UIColor(red: 250/255.0, green: 15/255.0, blue: 20/255.0, alpha: 1.0)
    static let CustomBlue = UIColor(red: 10/255.0, green: 0, blue: 200/255.0, alpha: 1.0)
}
```

### Опционалы

* НЕ использовать `!` при объявлении переменных! Исключений всего 2: это взаимодействие с Objective-C кодом и невозможность вычислить инициализирующее значения до достижения некоторых условий. Например, нельзя прочитать `frame` некоторой `UIView` до того, как она загружена из `xib`:

```swift
class MyView : UIView {
    @IBOutlet var button : UIButton!
    var buttonOriginalWidth : CGFloat!

    override func awakeFromNib() {
        self.buttonOriginalWidth = self.button.frame.size.width
    }
}
```

* Не использовать `!` для force unwrapping. Вместо этого использовать `?.` или `if-let` конструкции.

```swift 
let latitude = me.homeTown?.location?.latitude
if let town = me.homeTown, location = town.location {
    // access latitude, longitude
}
```

### Структуры

При работе с CoreGraphics использовать инициализаторы структур вместо глобальных С-функций:

**Нежелательно:** 
```swift
CGPointMake(1, 1)
CGRectMake(0, 0, 100, 20)
```
**Желательно:** 
```swift
CGPoint(x: 1, y: 1)
CGRect(x: 0, y: 0, width: 100, height: 20)
```

### Ленивая инициализация

Если инициализирующий вычислимый код занимает много места, разумно выделить его в метод-фабрику с приставкой `make`. Важно отметить, что `[unowned self]` в таком случае использовать не нужно, так как не создается retain cycle.

```swift
lazy var myButton = self.makeButton()
// ...
private func makeButton() -> UIButton { ... }
```

### Синтаксический сахар

**Нежелательно:** 
```swift
let a: Array<String> = //...
```
**Желательно:** 
```swift
let a: [String] = //...
```
