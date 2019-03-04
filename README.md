# Surf Swift Style Guide

## Цели

Этот стайлгайд создан с целью:

* Облегчить чтение и понимание незнакомого кода
* Облегчить поддержку кода
* Уменьшить вероятность совершения простых ошибок кодинга
* Снизить когнитивную нагрузку при кодинге
* Сфокусировать обсуждения в Pull Request-ах на логике, а не на стиле

Краткость кода не является основной целью. Код должен быть кратким только в том случае, если другие важные качества кода (такие как читаемость, простота и ясность) остаются равными или улучшаются.

## Принципы

* Этот гайд являеся дополнением официальному [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
* Мы стараемся сделать каждое правило проверяемым при помощи различных linter-ов

## Содержание

- [Surf Swift Style Guide](#surf-swift-style-guide)
  - [Цели](#%D1%86%D0%B5%D0%BB%D0%B8)
  - [Принципы](#%D0%BF%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF%D1%8B)
  - [Содержание](#%D1%81%D0%BE%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D0%BD%D0%B8%D0%B5)
  - [Xcode форматирование](#xcode-%D1%84%D0%BE%D1%80%D0%BC%D0%B0%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5)
  - [Именование](#%D0%B8%D0%BC%D0%B5%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5)
  - [Стиль](#%D1%81%D1%82%D0%B8%D0%BB%D1%8C)
    - [Функции](#%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8)
    - [Замыкания](#%D0%B7%D0%B0%D0%BC%D1%8B%D0%BA%D0%B0%D0%BD%D0%B8%D1%8F)
    - [Операторы](#%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D1%8B)
  - [Паттерны](#%D0%BF%D0%B0%D1%82%D1%82%D0%B5%D1%80%D0%BD%D1%8B)
  - [Организация файлов](#%D0%BE%D1%80%D0%B3%D0%B0%D0%BD%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F-%D1%84%D0%B0%D0%B9%D0%BB%D0%BE%D0%B2)
  - [Совместимость с Objective-C](#%D1%81%D0%BE%D0%B2%D0%BC%D0%B5%D1%81%D1%82%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D1%8C-%D1%81-objective-c)

## Xcode форматирование

_Вы можете добавить эти настройки воспользовавшись [этим скриптом](resources/xcode_settings.bash), как вариант, его вызов можно добавить в "Run Script" build phase._

* <a id='column-width'></a><a href='#column-width'>#</a> **Каждая строка должна иметь максимальную длину в 120 символов.** [![SwiftLint: line_length](https://img.shields.io/badge/SwiftLint-line__length-217D89.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#line-length)

* <a id='spaces-over-tabs'></a><a href='#spaces-over-tabs'>#</a> **Используйте 4 пробела для отступов.**

* <a id='trailing-whitespace'></a><a href='#trailing-whitespace'>#</a> **Строки не должны содержать пробелы в конце.**  [![SwiftFormat: trailingSpace](https://img.shields.io/badge/SwiftFormat-trailingSpace-7B0051.svg)](https://github.com/nicklockwood/SwiftFormat/blob/master/Rules.md#trailingSpace) [![SwiftLint: trailing_whitespace](https://img.shields.io/badge/SwiftLint-trailing__whitespace-217D89.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#trailing-whitespace)

## Именование

* <a id='use-camel-case'></a><a href='#use-camel-case'>#</a> **Используйте PascalCase для названий типов и протоколов, и lowerCamelCase для всего остального.** [![SwiftLint: type_name](https://img.shields.io/badge/SwiftLint-type__name-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#type-name)

  <details>

  ```swift
  protocol SpaceThing {
    // ...
  }

  class SpaceFleet: SpaceThing {

    enum Formation {
      // ...
    }

    class Spaceship {
      // ...
    }

    var ships: [Spaceship] = []
    static let worldName = "Earth"

    func addShip(_ ship: Spaceship) {
      // ...
    }
  }

  let myFleet = SpaceFleet()
  ```

  </details>

  _Исключение: Вы можете поставить префикс подчеркивания перед приватным свойством если оно повторяет свойство или метод с одинаковым именем с более высоким уровнем доступа_

  <details>

  #### Почему?
  Есть некоторые случаи при которых повторение названия свойства или метода может быть проще для чтения и понимания, чем использование другого имени.

  </details>

* <a id='bool-names'></a><a href='#bool-names'>#</a> **Называйте булевые переменные в формате `isSpaceship`, `hasSpacesuit`, и т.п.** Так становится понятнее, что это именно Bool тип данных, а не какой-либо другой.

* <a id='capitalize-acronyms'></a><a href='#capitalize-acronyms'>#</a> **Акронимы в названиях (например `URL`) должны быть в верхнем регистре за исключением случаев, когда это начало названия которое должно быть в lowerCamelCase**

  <details>

  ```swift
  // Неправильно
  class UrlValidator {

    func isValidUrl(_ URL: URL) -> Bool {
      // ...
    }

    func isUrlReachable(_ URL: URL) -> Bool {
      // ...
    }
  }

  let URLValidator = UrlValidator().isValidUrl(/* some URL */)

  // Правильно
  class URLValidator {

    func isValidURL(_ url: URL) -> Bool {
      // ...
    }

    func isURLReachable(_ url: URL) -> Bool {
      // ...
    }
  }

  let urlValidator = URLValidator().isValidURL(/* some URL */)
  ```

  </details>

* <a id='general-part-first'></a><a href='#general-part-first'>#</a> **Общая часть названия должна быть впереди, а более специфичная часть должна следовать за ней.** Значение "общая часть" зависит от конеткста, но должно примерно означать "то, что больше всего помогает вам сузить поиск нужного элемента." Самое главное, будьте последовательны с тем, как вы располагаете части имен.

  <details>

  ```swift
  // Неправильно
  let rightTitleMargin: CGFloat
  let leftTitleMargin: CGFloat
  let bodyRightMargin: CGFloat
  let bodyLeftMargin: CGFloat

  // Правильно
  let titleMarginRight: CGFloat
  let titleMarginLeft: CGFloat
  let bodyMarginRight: CGFloat
  let bodyMarginLeft: CGFloat
  ```

  </details>

* <a id='hint-at-types'></a><a href='#hint-at-types'>#</a> **Включите подсказку о типе в имя, если в противном случае оно будет неоднозначным.**

  <details>

  ```swift
  // Неправильно
  let title: String
  let cancel: UIButton

  // Правильно
  let titleText: String
  let cancelButton: UIButton
  ```

  </details>

* <a id='past-tense-events'></a><a href='#past-tense-events'>#</a> **Обработчики событий должны быть названы как предложения в настоящем времени.** Детали можно опустить, если они не нужны для ясности.

  <details>

  ```swift
  // Неправильно
  class SomeViewController {

    private func didTapLogin() {
      // ...
    }

    private func didTapBookButton() {
      // ...
    }

    private func modelDidChange() {
      // ...
    }
  }

  // Правильно
  class SomeViewController {

    private func login() {
      // ...
    }

    private func handleBookButtonTap() {
      // ...
    }

    private func modelChanged() {
      // ...
    }
  }
  ```

## Стиль

* <a id='use-implicit-types'></a><a href='#use-implicit-types'>#</a> **Не указываете типы там, где они легко могут быть выведены**

  <details>

  ```swift
  // Неправильно
  let host: Host = Host()

  // Правильно
  let host = Host()
  ```

  ```swift
  enum Direction {
    case left
    case right
  }

  func someDirection() -> Direction {
    // Неправильно
    return Direction.left

    // Правильно
    return .left
  }
  ```

  </details>

* <a id='use-implicit-types'></a><a href='#use-implicit-types'>#</a> **Условные операторы должны всегда вызывать `return` в следующей строке** [![SwiftLint: conditional_returns_on_newline](https://img.shields.io/badge/SwiftLint-trailing__comma-217D89.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#conditional-returns-on-newline)

  <details>

  ```swift
  // Неправильно
  guard true else { return }

  if true { return }

  // Правильно
  guard true else {
    return
  }

  if true {
    return
  }
  ```

  </details>

* <a id='omit-self'></a><a href='#omit-self'>#</a> **Не используйте `self` пока это не нужно для уточнения или пока того не требует язык.** Это правило не касается инициализаторов, там нужно использовать `self` всегда. [![SwiftFormat: redundantSelf](https://img.shields.io/badge/SwiftFormat-redundantSelf-7B0051.svg)](https://github.com/nicklockwood/SwiftFormat/blob/master/Rules.md#redundantSelf)

  <details>

  ```swift
  final class Listing {

    init(capacity: Int, allowsPets: Bool) {
      // Правильно
      self.capacity = capacity
      self.isFamilyFriendly = !allowsPets
    }

    private let isFamilyFriendly: Bool
    private var capacity: Int

    private func increaseCapacity(by amount: Int) {
      // Неправильно
      self.capacity += amount
      self.save()

      // Правильно
      capacity += amount
      save()
    }
  }
  ```

  </details>

* <a id='trailing-comma-array'></a><a href='#trailing-comma-array'>#</a> **Следует избегать закрывающей запятой в массивах и словарях** [![SwiftLint: trailing_comma](https://img.shields.io/badge/SwiftLint-trailing__comma-217D89.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#trailing-comma)

  <details>

  ```swift
  // Неправильно
  let rowContent = [
    listingUrgencyDatesRowContent(),
    listingUrgencyBookedRowContent(),
    listingUrgencyBookedShortRowContent(),
  ]

  // Правильно
  let rowContent = [
    listingUrgencyDatesRowContent(),
    listingUrgencyBookedRowContent(),
    listingUrgencyBookedShortRowContent()
  ]
  ```

  </details>

* <a id='name-tuple-elements'></a><a href='#name-tuple-elements'>#</a> **Именуйте свойства в кортеже для большей ясности** Эмпирическое правило: если у вас есть более 3 полей, вы, вероятно, должны использовать структуру.

  <details>

  ```swift
  // Неправильно
  func whatever() -> (Int, Int) {
    return (4, 4)
  }
  let thing = whatever()
  print(thing.0)

  // Правильно
  func whatever() -> (x: Int, y: Int) {
    return (x: 4, y: 4)
  }

  // Так тоже можно
  func whatever2() -> (x: Int, y: Int) {
    let x = 4
    let y = 4
    return (x, y)
  }

  let coord = whatever()
  coord.x
  coord.y
  ```

  </details>

* <a id='favor-constructors'></a><a href='#favor-constructors'>#</a> **Используйте конструкторы вместо Make() функций для CGRect, CGPoint, NSRange и других.** [![SwiftLint: legacy_constructor](https://img.shields.io/badge/SwiftLint-legacy__constructor-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#legacy-constructor)

  <details>

  ```swift
  // Неправильно
  let rect = CGRectMake(10, 10, 10, 10)

  // Правильно
  let rect = CGRect(x: 0, y: 0, width: 10, height: 10)
  ```

  </details>

* <a id='use-modern-swift-extensions'></a><a href='#use-modern-swift-extensions'>#</a> **Используйте современные Swift расширения методов вместо старых глобальных методов из Objective-C.** [![SwiftLint: legacy_cggeometry_functions](https://img.shields.io/badge/SwiftLint-legacy__cggeometry__functions-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#legacy-cggeometry-functions) [![SwiftLint: legacy_constant](https://img.shields.io/badge/SwiftLint-legacy__constant-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#legacy-constant) [![SwiftLint: legacy_nsgeometry_functions](https://img.shields.io/badge/SwiftLint-legacy__nsgeometry__functions-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#legacy-nsgeometry-functions)

  <details>

  ```swift
  // Неправильно
  var rect = CGRectZero
  var width = CGRectGetWidth(rect)

  // Правильно
  var rect = CGRect.zero
  var width = rect.width
  ```

  </details>

* <a id='colon-spacing'></a><a href='#colon-spacing'>#</a> **Ставьте двоеточие и пробел сразу после идентификатора.** [![SwiftLint: colon](https://img.shields.io/badge/SwiftLint-colon-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#colon)

  <details>

  ```swift
  // Неправильно
  var something : Double = 0

  // Правильно
  var something: Double = 0
  ```

  ```swift
  // Неправильно
  class MyClass : SuperClass {
    // ...
  }

  // Правильно
  class MyClass: SuperClass {
    // ...
  }
  ```

  ```swift
  // Неправильно
  var dict = [KeyType:ValueType]()
  var dict = [KeyType : ValueType]()

  // Правильно
  var dict = [KeyType: ValueType]()
  ```

  </details>

* <a id='return-arrow-spacing'></a><a href='#return-arrow-spacing'>#</a> **Ставьте пробел по обеим сторонам стрелки возвращаемого типа.** [![SwiftLint: return_arrow_whitespace](https://img.shields.io/badge/SwiftLint-return__arrow__whitespace-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#returning-whitespace)

  <details>

  ```swift
  // Неправильно
  func doSomething()->String {
    // ...
  }

  // Правильно
  func doSomething() -> String {
    // ...
  }
  ```

  ```swift
  // Неправильно
  func doSomething(completion: ()->Void) {
    // ...
  }

  // Правильно
  func doSomething(completion: () -> Void) {
    // ...
  }
  ```

  </details>

* <a id='unnecessary-parens'></a><a href='#unnecessary-parens'>#</a> **Избегайте лишних скобок.** [![SwiftFormat: redundantParens](https://img.shields.io/badge/SwiftFormat-redundantParens-7B0051.svg)](https://github.com/nicklockwood/SwiftFormat/blob/master/Rules.md#redundantParens) [![SwiftLint: empty_parentheses_with_trailing_closure](https://img.shields.io/badge/SwiftLint-empty__parentheses__with__trailing__closure-217D89.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#empty-parentheses-with-trailing-closure)

  <details>

  ```swift
  // Неправильно
  if (userCount > 0) { ... }
  switch (someValue) { ... }
  let evens = userCounts.filter { (number) in number % 2 == 0 }
  let squares = userCounts.map() { $0 * $0 }

  // Правильно
  if userCount > 0 { ... }
  switch someValue { ... }
  let evens = userCounts.filter { number in number % 2 == 0 }
  let squares = userCounts.map { $0 * $0 }
  ```

  </details>

* <a id='unnecessary-enum-arguments'></a> <a href='#unnecessary-enum-arguments'>#</a> **Опустите аргументы case, если они все без имени** [![SwiftLint: empty_enum_arguments](https://img.shields.io/badge/SwiftLint-empty__enum__arguments-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#empty-enum-arguments)

  <details>

  ```swift
  // Неправильно
  if case .done(_) = result { ... }

  switch animal {
  case .dog(_, _, _):
    ...
  }

  // Правильно
  if case .done = result { ... }

  switch animal {
  case .dog:
    ...
  }
  ```

  </details>

### Функции

* <a id='omit-function-void-return'></a><a href='#omit-function-void-return'>#</a> **Опускайте возвращаемый тип `Void`.** [![SwiftLint: redundant_void_return](https://img.shields.io/badge/SwiftLint-redundant__void__return-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#redundant-void-return)

  <details>

  ```swift
  // Неправильно
  func doSomething() -> Void {
    ...
  }

  // Правильно
  func doSomething() {
    ...
  }
  ```

  </details>

### Замыкания

* <a id='favor-void-closure-return'></a><a href='#favor-void-closure-return'>#</a> **Используйте возвращаемый тип `Void` вместо `()` в определении замыкания.** [![SwiftLint: void_return](https://img.shields.io/badge/SwiftLint-void__return-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#void-return)

  <details>

  ```swift
  // Неправильно
  func method(completion: () -> ()) {
    ...
  }

  // Правильно
  func method(completion: () -> Void) {
    ...
  }
  ```

  </details>

* <a id='unused-closure-parameter-naming'></a><a href='#unused-closure-parameter-naming'>#</a> **Именуйте неиспользуемые параметры замыкания как нижние подчеркивания (`_`).** [![SwiftLint: unused_closure_parameter](https://img.shields.io/badge/SwiftLint-unused__closure__parameter-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#unused-closure-parameter)

    <details>

    #### Почему?
    Это упрощает чтение, так становится очевидно какие параметры используются, а какие не используются.

    ```swift
    // Неправильно
    someAsyncThing() { argument1, argument2, argument3 in
      print(argument3)
    }

    // Правильно
    someAsyncThing() { _, _, argument3 in
      print(argument3)
    }
    ```

    </details>

* <a id='closure-brace-spacing'></a><a href='#closure-brace-spacing'>#</a> **Однострочные замыкания должны содержать по одному пробелу до и после каждой скобки, за исключением пробела между закрывающей скобкой и следующим оператором.** [![SwiftLint: closure_spacing](https://img.shields.io/badge/SwiftLint-closure__spacing-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#closure-spacing)

  <details>

  ```swift
  // Неправильно
  let evenSquares = numbers.filter {$0 % 2 == 0}.map {  $0 * $0  }

  // Правильно
  let evenSquares = numbers.filter { $0 % 2 == 0 }.map { $0 * $0 }
  ```

  </details>

### Операторы

* <a id='infix-operator-spacing'></a><a href='#infix-operator-spacing'>#</a> **Инфиксные операторы должны отделятся одним пробелом с каждой стороны.** Предпочитайте скобки, чтобы визуально группировать выражения с большим количеством операторов, а не изменять ширину пробелов. Это правило не относится к операторам диапазона (например, `1...3`) и к префиксным или постфиксным операторам (например, `guest?` или `-1`). [![SwiftLint: operator_usage_whitespace](https://img.shields.io/badge/SwiftLint-operator__usage__whitespace-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#operator-usage-whitespace)

  <details>

  ```swift
  // Неправильно
  let capacity = 1+2
  let capacity = currentCapacity   ?? 0
  let mask = (UIAccessibilityTraitButton|UIAccessibilityTraitSelected)
  let capacity=newCapacity
  let latitude = region.center.latitude - region.span.latitudeDelta/2.0

  // Правильно
  let capacity = 1 + 2
  let capacity = currentCapacity ?? 0
  let mask = (UIAccessibilityTraitButton | UIAccessibilityTraitSelected)
  let capacity = newCapacity
  let latitude = region.center.latitude - (region.span.latitudeDelta / 2.0)
  ```

  </details>

## Паттерны

* <a id='implicitly-unwrapped-optionals'></a><a href='#implicitly-unwrapped-optionals'>#</a> **Инициализируйте свойства в `init` где это возможно, а не используйте форс-анвраппинг.**  Заметным исключением является UIViewController и его `view` свойство. [![SwiftLint: implicitly_unwrapped_optional](https://img.shields.io/badge/SwiftLint-implicitly__unwrapped__optional-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#implicitly-unwrapped-optional)

  <details>

  ```swift
  // Неправильно
  class MyClass: NSObject {

    var someValue: Int!

    init() {
      super.init()
      someValue = 5
    }

  }

  // Правильно
  class MyClass: NSObject {

    var someValue: Int

    init() {
      someValue = 0
      super.init()
    }

  }
  ```

  </details>

* <a id='time-intensive-init'></a><a href='#time-intensive-init'>#</a> **Избегайте выполнение любой значимой или времязатратной работы в `init()`.** Избегайте таких действий, как открытие соединения с базой данных, выполнение запросов в сеть, чтение большого объема данных с диска и т.п. Создайте метод вроде `start()` если вам нужно чтобы эти действия были выполнены до того как объект будет готов к использованию.

* <a id='complex-property-observers'></a><a href='#complex-property-observers'>#</a> **Выносите сложные наблюдатели свойств в методы.** Это уменьшает вложенность, отделяет сайд-эффекты от объявления и делает явным использование неявно передаваемых параметров, таких как `oldValue`.

  <details>

  ```swift
  // Неправильно
  class TextField {
    var text: String? {
      didSet {
        guard oldValue != text else {
          return
        }

        // Куча побочных эффектов связанных с текстом
      }
    }
  }

  // Правильно
  class TextField {
    var text: String? {
      didSet { updateText(from: oldValue) }
    }

    private func updateText(from oldValue: String?) {
      guard oldValue != text else {
        return
      }

        // Куча побочных эффектов связанных с текстом
    }
  }
  ```

  </details>

* <a id='complex-callback-block'></a><a href='#complex-callback-block'>#</a> **Выносите сложные определения замыканий в методы**.  Это уменьшает вложенность и сложность использования weak-self в блоках. Если необходимо сослаться на self в вызове замыкания, используйте `guard`, чтобы развернуть self на время вызова.

  <details>

  ```swift
  // Неправильно
  class MyClass {

    func request(completion: () -> Void) {
      API.request { [weak self] response in
        if let strongSelf = self {
          // Processing and side effects
        }
        completion()
      }
    }
  }

  // Правильно
  class MyClass {

    func request(completion: () -> Void) {
      API.request { [weak self] response in
        guard let strongSelf = self else { return }
        strongSelf.doSomething(strongSelf.property)
        completion()
      }
    }

    func doSomething(nonOptionalParameter: SomeClass) {
      // Processing and side effects
    }
  }
  ```

  </details>

* <a id='guards-at-top'></a><a href='#guards-at-top'>#</a> **Используйте `guard` в начале скоупа.**

  <details>

  #### Почему?
  Проще рассуждать о блоке кода, когда все операторы `guard` сгруппированы вверху, а не смешаны с бизнес-логикой.

  </details>

* <a id='limit-access-control'></a><a href='#limit-access-control'>#</a> **Контроль доступа должен быть максимально строгим.** Предпочитайте использование `public` вместо `open` и `private` вместо `fileprivate` пока вам не понадобится это поведение.

* <a id='avoid-global-functions'></a><a href='#avoid-global-functions'>#</a> **Избегайте глобальных функций где это возможно.** Предпочитайте методы в определениях типов.

  <details>

  ```swift
  // Неправильно
  func age(of person, bornAt timeInterval) -> Int {
    // ...
  }

  func jump(person: Person) {
    // ...
  }

  // Правильно
  class Person {
    var bornAt: TimeInterval

    var age: Int {
      // ...
    }

    func jump() {
      // ...
    }
  }
  ```

  </details>

* <a id='private-constants'></a><a href='#private-constants'>#</a> **Предпочитайте выделять константы в закрытый enum.** Если константы должны быть открыты, сделайте их статичными внутри определения класса.

  <details>

  ```swift
  public class MyClass {

    private enum Constants {
      static let privateValue = "private"
    }

    public static let publicValue = "public"

    func doSomething() {
      print(Constants.privateValue)
      print(MyClass.publicValue)
    }
  }
  ```

  </details>

* <a id='namespace-using-enums'></a><a href='#namespace-using-enums'>#</a> **Используйте `enum` без case для организации `public` или `internal` констант и функций в пространства имен.** Избегайте создания глобальных констант или функций. Не стесняйтесь вкладывать пространства имен, где это добавляет ясности.

  <details>

  #### Почему?
  `enum`-ы без case хорошо работают как пространства имен так как они не могут быть созданы, что соответствует их назначению.

  ```swift
  enum Environment {

    enum Earth {
      static let gravity = 9.8
    }

    enum Moon {
      static let gravity = 1.6
    }
  }
  ```

  </details>

* <a id='prefer-immutable-values'></a><a href='#prefer-immutable-values'>#</a> **Используйте неизменяемые значения где это возможно.** Используйте `map` и `compactMap` вместо добавления в новую коллекцию. Используйте `filter` вмеcто удаления элементов из изменяемой коллекции.

  <details>

  #### Почему?
  Изменяемые свойства увеличивают сложность, поэтому старайтесь держать их в максимально узкой области.

  ```swift
  // Неправильно
  var results = [SomeType]()
  for element in input {
    let result = transform(element)
    results.append(result)
  }

  // Правильно
  let results = input.map { transform($0) }
  ```

  ```swift
  // Неправильно
  var results = [SomeType]()
  for element in input {
    if let result = transformThatReturnsAnOptional(element) {
      results.append(result)
    }
  }

  // Правильно
  let results = input.compactMap { transformThatReturnsAnOptional($0) }
  ```

  </details>

* <a id='final-classes-by-default'></a><a href='#final-classes-by-default'>#</a> **Классы должны быть `final`, если другого не требует логика.**

  <details>

  #### Почему?
  Если класс должен быть переопределен, автор должен указать эту функциональность, опуская ключевое слово `final`.

  ```swift
  // Неправильно
  class SettingsRepository {
    // ...
  }

  // Правильно
  final class SettingsRepository {
    // ...
  }
  ```

  </details>

* <a id='switch-never-default'></a><a href='#switch-never-default'>#</a> **Никогда не используйте `default` case в `switch`.**

  <details>

  #### Почему?
  Перечисление каждого case требует, чтобы разработчики и ревьюеры учитывали правильность каждого оператора switch при добавлении новых case.

  ```swift
  // Неправильно
  switch anEnum {
  case .a:
    // Do something
  default:
    // Do something else.
  }

  // Правильно
  switch anEnum {
  case .a:
    // Do something
  case .b, .c:
    // Do something else.
  }
  ```

  </details>

* <a id='optional-nil-check'></a><a href='#optional-nil-check'>#</a> **Проверьте значение nil вместо использования разворачивания, если значение не требуется.** [![SwiftLint: unused_optional_binding](https://img.shields.io/badge/SwiftLint-unused_optional_binding-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#unused-optional-binding)

## Организация файлов

* <a id='alphabetize-imports'></a><a href='#alphabetize-imports'>#</a> **Сортируйте импорты по алфавиту и ставьте их после комментариев в заголовке файла.** Если одна из библиотек уже импортирует какие-то библиотеки необходимые в вашем модуле - их импорты можно опустить. [![SwiftFormat: sortedImports](https://img.shields.io/badge/SwiftFormat-sortedImports-7B0051.svg)](https://github.com/nicklockwood/SwiftFormat/blob/master/Rules.md#sortedImports)

  <details>

  #### Почему?
  Стандартный метод организации помогает инженерам быстрее определить, от каких модулей зависит файл.

  ```swift
  // Неправильно

  //  Copyright © 2018 Surf. All rights reserved.
  //
  import DLSPrimitives
  import Constellation
  import Epoxy  

  import UIKit
  import Foundation

  // Правильно

  //  Copyright © 2018 Surf. All rights reserved.
  //

  import Constellation
  import DLSPrimitives
  import Epoxy
  import UIKit
  ```

  </details>

  _Исключение: `@testable import` должны быть сгурппированы после обычных import и разделены пустой строкой._

  <details>

  ```swift
  // Неправильно

  //  Copyright © 2018 Surf. All rights reserved.
  //

  import DLSPrimitives
  @testable import Epoxy
  import Foundation
  import Nimble
  import Quick

  // Правильно

  //  Copyright © 2018 Surf. All rights reserved.
  //

  import DLSPrimitives
  import Foundation
  import Nimble
  import Quick

  @testable import Epoxy
  ```

  </details>

* <a id='limit-vertical-whitespace'></a><a href='#limit-vertical-whitespace'>#</a> **Ограничьте пустые вертикальные пробелы одной строкой.** [![SwiftLint: vertical_whitespace](https://img.shields.io/badge/SwiftLint-vertical__whitespace-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#vertical-whitespace)

* <a id='newline-at-eof'></a><a href='#newline-at-eof'>#</a> **Файлы должны заканчиваться новой строкой.** [![SwiftLint: trailing_newline](https://img.shields.io/badge/SwiftLint-trailing__newline-007A87.svg)](https://github.com/realm/SwiftLint/blob/master/Rules.md#trailing-newline)

## Совместимость с Objective-C

* <a id='prefer-pure-swift-classes'></a><a href='#prefer-pure-swift-classes'>#</a> **Старайтесь избегать наследования от NSObject.** Если ваш код должен быть использован каким-нибудь Objective-C кодом оберните его чтобы предоставить необходимую функциональность. Используйте `@objc` для отдельных функций и переменных вместо предоставления всего API класса при помощи `@objcMembers`.

  <details>

  ```swift
  class PriceBreakdownViewController {

    private let acceptButton = UIButton()

    private func setUpAcceptButton() {
      acceptButton.addTarget(
        self,
        action: #selector(didTapAcceptButton),
        forControlEvents: .TouchUpInside)
    }

    @objc
    private func didTapAcceptButton() {
      // ...
    }
  }
  ```

  </details>
