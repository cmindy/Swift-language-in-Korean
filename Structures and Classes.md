[TOC]

# Structures and Classes 구조체와 클래스

*Structures* and *classes* are general-purpose, flexible constructs that become the building blocks of your program’s code. You define properties and methods to add functionality to your structures and classes using the same syntax you use to define constants, variables, and functions.

구조체와 클래스는 프로그램 코드의 빌딩 블록(단위)이 되는 범용 목적의 유연한 구조입니다. 상수, 변수 및 함수를 정의하는 데 사용하는 것과 동일한 문법을 사용해 구조 및 클래스에 기능을 추가하는 속성 및 메서드를 정의합니다.



Unlike other programming languages, Swift doesn’t require you to create separate interface and implementation files for custom structures and classes. In Swift, you define a structure or class in a single file, and the external interface to that class or structure is automatically made available for other code to use.

다른 프로그래밍 언어와 달리 Swift는 커스텀 구조체들 및 클래스들에 대한 별도의 인터페이스 및 구현 파일을 만들 필요가 없습니다. Swift에서는 구조체 또는 클래스를 단일 파일로 정의하고 해당 클래스나 구조체에 대한 외부 인터페이스를 다른 코드에서 자동으로 사용할 수 있습니다.



>  NOTE
>
>  An instance of a class is traditionally known as an *object*. However, Swift structures and classes are much closer in functionality than in other languages, and much of this chapter describes functionality that applies to instances of *either* a class or a structure type. Because of this, the more general term *instance* is used.

> NOTE
>
> 클래스의 인스턴스는 전통적으로 객체로 알려져 있습니다. 그러나 Swift 구조와 클래스는 다른 언어보다 기능면에서 훨씬 가깝습니다.이 장의 많은 부분에서는 클래스 나 구조체 유형의 인스턴스에 적용되는 기능에 대해 설명합니다. 이 때문에 더 일반적인 용어 인스턴스가 사용됩니다.



## Comparing Structures and Classes 구조체와 클래스 비교

Structures and classes in Swift have many things in common. Both can:

- Define properties to store values
- Define methods to provide functionality
- Define subscripts to provide access to their values using subscript syntax
- Define initializers to set up their initial state
- Be extended to expand their functionality beyond a default implementation
- Conform to protocols to provide standard functionality of a certain kind

For more information, see [Properties](https://docs.swift.org/swift-book/LanguageGuide/Properties.html), [Methods](https://docs.swift.org/swift-book/LanguageGuide/Methods.html), [Subscripts](https://docs.swift.org/swift-book/LanguageGuide/Subscripts.html), [Initialization](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html), [Extensions](https://docs.swift.org/swift-book/LanguageGuide/Extensions.html), and [Protocols](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html).

Swift에서 구조체와 클래스는 공통적으로 많은 것을 할 수 있습니다. 

- 값을 저장할 프로퍼티 정의
- 기능을 제공하는 메소드 정의
- 서브스크립트 문법을 사용하여 값에 대한 액세스를 제공하는 서브스크립트 정의
- 초기 상태를 설정하는 이니셜라이저 정의
- 기본 구현 이상으로 기능을 확장하도록 확대
- 프로토콜을 준수하여 특정 종류의 표준 기능을 제공

자세한 내용은 [Properties](https://docs.swift.org/swift-book/LanguageGuide/Properties.html), [Methods](https://docs.swift.org/swift-book/LanguageGuide/Methods.html), [Subscripts](https://docs.swift.org/swift-book/LanguageGuide/Subscripts.html), [Initialization](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html), [Extensions](https://docs.swift.org/swift-book/LanguageGuide/Extensions.html), [Protocols](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html) 을 참고하세요.



Classes have additional capabilities that structures don’t have:

- Inheritance enables one class to inherit the characteristics of another.
- Type casting enables you to check and interpret the type of a class instance at runtime.
- Deinitializers enable an instance of a class to free up any resources it has assigned.
- Reference counting allows more than one reference to a class instance.

For more information, see [Inheritance](https://docs.swift.org/swift-book/LanguageGuide/Inheritance.html), [Type Casting](https://docs.swift.org/swift-book/LanguageGuide/TypeCasting.html), [Deinitialization](https://docs.swift.org/swift-book/LanguageGuide/Deinitialization.html), and [Automatic Reference Counting](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html).

클래스에는 구조체에 없는 추가 기능이 있습니다.

- 상속은 한 클래스가 다른 클래스의 특성을 상속 할 수 있게 합니다.
- 타입 캐스팅을 사용하면 런타임에 클래스 인스턴스의 타입을 확인하고 해석 할 수 있습니다.
- Deinitializers는 클래스 인스턴스가 할당한 모든 리소스를 해제 할 수 있게 합니다.
- 참조 카운팅은 클래스 인스턴스에 대한 둘 이상의 참조를 허용합니다.

자세한 내용은 [Inheritance](https://docs.swift.org/swift-book/LanguageGuide/Inheritance.html), [Type Casting](https://docs.swift.org/swift-book/LanguageGuide/TypeCasting.html), [Deinitialization](https://docs.swift.org/swift-book/LanguageGuide/Deinitialization.html),  [Automatic Reference Counting](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html) 을 참고하세요.



The additional capabilities that classes support come at the cost of increased complexity. As a general guideline, prefer structures because they’re easier to reason about, and use classes when they’re appropriate or necessary. In practice, this means most of the custom data types you define will be structures and enumerations. For a more detailed comparison, see [Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes).

클래스가 지원하는 추가 기능은 복잡성 증가때문입니다. 일반적인 가이드라인으로, 클래스보다 구조체가 더 쉬워서 구조체를 선호합니다.  구조체보다 클래스가 더 적절하거나 필요할 때에만 클래스를 사용합니다. 실제로 이는 사용자가 정의한 대부분의 데이터 형식이 구조체와 열거형임을 의미합니다. 보다 자세한 비교 방법은  [Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes) 를 참고하십시오.

### Definition Syntax 정의 구문

Structures and classes have a similar definition syntax. You introduce structures with the `struct` keyword and classes with the `class` keyword. Both place their entire definition within a pair of braces:

구조와 클래스는 비슷한 정의 구문을 사용합니다. `struct` 키워드 및 `class` 키워드를 사용하여 구조를 소개합니다. 두 가지 모두 전체 정의를 중괄호 안에 넣습니다.

```swift
struct SomeStructure {
    // structure definition goes here
}
class SomeClass {
    // class definition goes here
}
```

>  NOTE
>
>  Whenever you define a new structure or class, you define a new Swift type. Give types `UpperCamelCase` names (such as `SomeStructure` and `SomeClass` here) to match the capitalization of standard Swift types (such as `String`, `Int`, and `Bool`). Give properties and methods `lowerCamelCase` names (such as `frameRate` and `incrementCount`) to differentiate them from type names.

> NOTE
>
> 새로운 구조체나 클래스를 정의 할 때마다 새로운 Swift 타입을 정의하는 것입니다. 타입에 표준 Swift 타입 (`String`, `Int` 및 `Bool`과 같은)의 대소문자와 일치될 수 있도록 `UpperCamelCase`  이름을 지으세요.  (예시의  `SomeStructure` 및 `SomeClass`와 같이). 속성 및 메서드에는 `lowerCamelCase` 이름 (예 : `frameRate` 및 `incrementCount`)을 지정하여 타입 이름과 구분합니다.

Here’s an example of a structure definition and a class definition:

```swift
struct Resolution {
    var width = 0
    var height = 0
}
class VideoMode {
  var resolution = Resolution()
  var interlaced = false
  var frameRate = 0.0
  var name: String?
}
```

   

The example above defines a new structure called `Resolution`, to describe a pixel-based display resolution. This structure has two stored properties called `width` and `height`. Stored properties are constants or variables that are bundled up and stored as part of the structure or class. These two properties are inferred to be of type `Int` by setting them to an initial integer value of `0`.

위의 예제는 픽셀에 기반한 디스플레이 해상도를 설명하기 위해 `Resolution`이라는 새로운 구조체를 정의합니다. 이 구조체에는 `width` 및 `height`라는 두 개의 저장된 속성이 있습니다. 저장 프로퍼티는 번들로 묶여 구조체 또는 클래스의 일부로 저장되는 상수 또는 변수입니다. 이 두 속성은 초기 정수 값인 `0`으로 설정하여 `Int` 유형으로 유추됩니다.

The example above also defines a new class called `VideoMode`, to describe a specific video mode for video display. This class has four variable stored properties. The first, `resolution`, is initialized with a new `Resolution` structure instance, which infers a property type of `Resolution`. For the other three properties, new `VideoMode` instances will be initialized with an `interlaced` setting of `false` (meaning “noninterlaced video”), a playback frame rate of `0.0`, and an optional `String` value called `name`. The `name` property is automatically given a default value of `nil`, or “no `name` value”, because it’s of an optional type.

위의 예는 `VideoMode`라는 새로운 클래스를 정의하여 비디오 디스플레이를 위한 특정 비디오 모드를 설명합니다. 이 클래스에는 4 개의  저장변수 특성이 있습니다.  `resolution` 은, `Resolution`의 property 형으로 추측되는 새로운 `Resolution` 구조 인스턴스로 초기화됩니다. 다른 세 가지 속성의 경우 새 `VideoMode` 인스턴스는 `interlaced` 세팅은  `false` ( "인터레이스 되지 않은 비디오"를 의미), 재생 프레임 속도는  `0.0`, `name`이라는 옵셔널 `String` 값으로 초기화됩니다. `name` 속성은 옵셔널 타입이기 때문에 기본값인 `nil` 또는 "no `name` value"가 자동으로 주어집니다.

### Structure and Class Instances

The `Resolution` structure definition and the `VideoMode` class definition only describe what a `Resolution` or `VideoMode` will look like. They themselves don’t describe a specific resolution or video mode. To do that, you need to create an instance of the structure or class.

The syntax for creating instances is very similar for both structures and classes:

```swift
let someResolution = Resolution() 
let someVideoMode = VideoMode() 
```

Structures and classes both use initializer syntax for new instances. The simplest form of initializer syntax uses the type name of the class or structure followed by empty parentheses, such as `Resolution()` or `VideoMode()`. This creates a new instance of the class or structure, with any properties initialized to their default values. Class and structure initialization is described in more detail in [Initialization](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html).

### Accessing Properties

You can access the properties of an instance using *dot syntax*. In dot syntax, you write the property name immediately after the instance name, separated by a period (`.`), without any spaces:

```swift
print("The width of someResolution is \(someResolution.width)") 

// Prints "The width of someResolution is 0" 
```

In this example, `someResolution.width` refers to the `width` property of `someResolution`, and returns its default initial value of `0`.

You can drill down into subproperties, such as the `width` property in the `resolution`property of a `VideoMode`:

```swift
print("The width of someVideoMode is \(someVideoMode.resolution.width)") 

// Prints "The width of someVideoMode is 0" 
```

You can also use dot syntax to assign a new value to a variable property:

```swift
someVideoMode.resolution.width = 1280 

print("The width of someVideoMode is now \(someVideoMode.resolution.width)") 

// Prints "The width of someVideoMode is now 1280" 
```



### Memberwise Initializers for Structure Types

All structures have an automatically generated *memberwise initializer*, which you can use to initialize the member properties of new structure instances. Initial values for the properties of the new instance can be passed to the memberwise initializer by name:

```swift
let vga = Resolution(width: 640, height: 480) 
```

Unlike structures, class instances don’t receive a default memberwise initializer. Initializers are described in more detail in [Initialization](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html).

## Structures and Enumerations Are Value Types

A *value type* is a type whose value is *copied* when it’s assigned to a variable or constant, or when it’s passed to a function.

You’ve actually been using value types extensively throughout the previous chapters. In fact, all of the basic types in Swift—integers, floating-point numbers, Booleans, strings, arrays and dictionaries—are value types, and are implemented as structures behind the scenes.

All structures and enumerations are value types in Swift. This means that any structure and enumeration instances you create—and any value types they have as properties—are always copied when they are passed around in your code.

NOTE

Collections defined by the standard library like arrays, dictionaries, and strings use an optimization to reduce the performance cost of copying. Instead of making a copy immediately, these collections share the memory where the elements are stored between the original instance and any copies. If one of the copies of the collection is modified, the elements are copied just before the modification. The behavior you see in your code is always as if a copy took place immediately.

Consider this example, which uses the `Resolution` structure from the previous example:

```swift
let hd = Resolution(width: 1920, height: 1080) 
var cinema = hd 
```
This example declares a constant called `hd` and sets it to a `Resolution` instance initialized with the width and height of full HD video (1920 pixels wide by 1080 pixels high).

It then declares a variable called `cinema` and sets it to the current value of `hd`. Because `Resolution` is a structure, a *copy* of the existing instance is made, and this new copy is assigned to `cinema`. Even though `hd` and `cinema` now have the same width and height, they are two completely different instances behind the scenes.

Next, the `width` property of `cinema` is amended to be the width of the slightly wider 2K standard used for digital cinema projection (2048 pixels wide and 1080 pixels high):

```swift
cinema.width = 2048 
```

   

Checking the `width` property of `cinema` shows that it has indeed changed to be `2048`:

```swift
print("cinema is now \(cinema.width) pixels wide") 
   // Prints "cinema is now 2048 pixels wide" 
```

   

However, the `width` property of the original `hd` instance still has the old value of `1920`:

```swift
print("hd is still \(hd.width) pixels wide") 

// Prints "hd is still 1920 pixels wide" 
```

When `cinema` was given the current value of `hd`, the *values* stored in `hd` were copied into the new `cinema` instance. The end result was two completely separate instances that contained the same numeric values. However, because they are separate instances, setting the width of `cinema` to `2048` doesn’t affect the width stored in `hd`, as shown in the figure below:



The same behavior applies to enumerations:

```swift
enum CompassPoint {
    case north, south, east, west
    mutating func turnNorth() {
        self = .north
    }
}
var currentDirection = CompassPoint.west
let rememberedDirection = currentDirection
currentDirection.turnNorth()

print("The current direction is \(currentDirection)")
print("The remembered direction is \(rememberedDirection)")
// Prints "The current direction is north"
// Prints "The remembered direction is west"
```

When `rememberedDirection` is assigned the value of `currentDirection`, it’s actually set to a copy of that value. Changing the value of `currentDirection` thereafter doesn’t affect the copy of the original value that was stored in `rememberedDirection`.

## Classes Are Reference Types

Unlike value types, *reference types* are *not* copied when they are assigned to a variable or constant, or when they are passed to a function. Rather than a copy, a reference to the same existing instance is used.

Here’s an example, using the `VideoMode` class defined above:

```swift
let tenEighty = VideoMode() 
tenEighty.resolution = hd 
tenEighty.interlaced = true 
tenEighty.name = "1080i" 
tenEighty.frameRate = 25.0 
```

This example declares a new constant called `tenEighty` and sets it to refer to a new instance of the `VideoMode` class. The video mode is assigned a copy of the HD resolution of `1920` by `1080` from before. It’s set to be interlaced, its name is set to `"1080i"`, and its frame rate is set to `25.0` frames per second.

Next, `tenEighty` is assigned to a new constant, called `alsoTenEighty`, and the frame rate of `alsoTenEighty` is modified:

```swift
let alsoTenEighty = tenEighty 
alsoTenEighty.frameRate = 30.0 
```

Because classes are reference types, `tenEighty` and `alsoTenEighty` actually both refer to the *same* `VideoMode` instance. Effectively, they are just two different names for the same single instance, as shown in the figure below:



Checking the `frameRate` property of `tenEighty` shows that it correctly reports the new frame rate of `30.0` from the underlying `VideoMode` instance:

```swift
   print("The frameRate property of tenEighty is now \(tenEighty.frameRate)") 
   // Prints "The frameRate property of tenEighty is now 30.0" 
```

This example also shows how reference types can be harder to reason about. If `tenEighty`and `alsoTenEighty` were far apart in your program’s code, it could be difficult to find all the ways that the video mode is changed. Wherever you use `tenEighty`, you also have to think about the code that uses `alsoTenEighty`, and vice versa. In contrast, value types are easier to reason about because all of the code that interacts with the same value is close together in your source files.

Note that `tenEighty` and `alsoTenEighty` are declared as *constants*, rather than variables. However, you can still change `tenEighty.frameRate` and `alsoTenEighty.frameRate`because the values of the `tenEighty` and `alsoTenEighty` constants themselves don’t actually change. `tenEighty` and `alsoTenEighty` themselves don’t “store” the `VideoMode`instance—instead, they both *refer* to a `VideoMode` instance behind the scenes. It’s the `frameRate` property of the underlying `VideoMode` that is changed, not the values of the constant references to that `VideoMode`.

### Identity Operators

Because classes are reference types, it’s possible for multiple constants and variables to refer to the same single instance of a class behind the scenes. (The same isn’t true for structures and enumerations, because they are always copied when they are assigned to a constant or variable, or passed to a function.)

It can sometimes be useful to find out whether two constants or variables refer to exactly the same instance of a class. To enable this, Swift provides two identity operators:

- Identical to (`===`)
- Not identical to (`!==`)

Use these operators to check whether two constants or variables refer to the same single instance:

```swift
if tenEighty === alsoTenEighty {
    print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
}
// Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
```

Note that *identical to* (represented by three equals signs, or `===`) doesn’t mean the same thing as *equal to* (represented by two equals signs, or `==`). *Identical to* means that two constants or variables of class type refer to exactly the same class instance. *Equal to* means that two instances are considered equal or equivalent in value, for some appropriate meaning of *equal*, as defined by the type’s designer.

When you define your own custom structures and classes, it’s your responsibility to decide what qualifies as two instances being equal. The process of defining your own implementations of the `==` and `!=` operators is described in [Equivalence Operators](https://docs.swift.org/swift-book/LanguageGuide/AdvancedOperators.html#ID45).

### Pointers

If you have experience with C, C++, or Objective-C, you may know that these languages use *pointers* to refer to addresses in memory. A Swift constant or variable that refers to an instance of some reference type is similar to a pointer in C, but isn’t a direct pointer to an address in memory, and doesn’t require you to write an asterisk (`*`) to indicate that you are creating a reference. Instead, these references are defined like any other constant or variable in Swift. The standard library provides pointer and buffer types that you can use if you need to interact with pointers directly—see [Manual Memory Management](https://developer.apple.com/documentation/swift/swift_standard_library/manual_memory_management).