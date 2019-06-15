[TOC]

# Automatic Reference Counting

Swift uses *Automatic Reference Counting* (ARC) to track and manage your app’s memory usage. In most cases, this means that memory management “just works” in Swift, and you do not need to think about memory management yourself. ARC automatically frees up the memory used by class instances when those instances are no longer needed.

However, in a few cases ARC requires more information about the relationships between parts of your code in order to manage memory for you. This chapter describes those situations and shows how you enable ARC to manage all of your app’s memory. Using ARC in Swift is very similar to the approach described in [Transitioning to ARC Release Notes](https://developer.apple.com/library/content/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html) for using ARC with Objective-C.

Reference counting applies only to instances of classes. Structures and enumerations are value types, not reference types, and are not stored and passed by reference.

Swift는 ARC (Automatic Reference Counting)를 사용하여 앱의 메모리 사용을 추적하고 관리합니다. 대부분의 경우, 이는 Swift에서 메모리 관리가 "just works"하고 있다는 것을 의미하며 메모리 관리에 대해 직접 생각할 필요가 없습니다. ARC는 클래스 인스턴스가 더 이상 필요하지 않을 때 클래스 인스턴스가 사용하는 메모리를 자동으로 비웁니다.

그러나 몇 가지 경우 ARC는 메모리를 관리하기 위해 코드들 간의 관계에 대한 추가 정보가 필요합니다. 이 장에서는 이러한 상황에 대해 설명하고 ARC가 모든 앱의 메모리를 관리하는 방법을 보여줍니다. Swift에서 ARC를 사용하는 것은 Objective-C와 함께 ARC를 사용하기 위해 ARC 릴리스 노트로 전환에서 설명한 방법과 매우 유사합니다.

참조 카운팅은 클래스의 인스턴스에만 적용됩니다. 구조체와 열거형은 참조 타입이 아닌 값 타입이며 참조로 저장되고 전달되지 않습니다.

<br />

## How ARC Works

Every time you create a new instance of a class, ARC allocates a chunk of memory to store information about that instance. This memory holds information about the type of the instance, together with the values of any stored properties associated with that instance.

Additionally, when an instance is no longer needed, ARC frees up the memory used by that instance so that the memory can be used for other purposes instead. This ensures that class instances do not take up space in memory when they are no longer needed.

However, if ARC were to deallocate an instance that was still in use, it would no longer be possible to access that instance’s properties, or call that instance’s methods. Indeed, if you tried to access the instance, your app would most likely crash.

To make sure that instances don’t disappear while they are still needed, ARC tracks how many properties, constants, and variables are currently referring to each class instance. ARC will not deallocate an instance as long as at least one active reference to that instance still exists.

To make this possible, whenever you assign a class instance to a property, constant, or variable, that property, constant, or variable makes a *strong reference* to the instance. The reference is called a “strong” reference because it keeps a firm hold on that instance, and does not allow it to be deallocated for as long as that strong reference remains.

클래스의 새 인스턴스를 만들 때마다 ARC는 해당 인스턴스에 대한 정보를 저장하기 위해 메모리 덩어리를 할당합니다. 이 메모리에는 해당 인스턴스와 관련된 저장된 속성의 값과 함께 인스턴스 타입에 대한 정보가 저장됩니다.

또한 인스턴스가 더 이상 필요하지 않을 때 ARC는 해당 인스턴스가 사용하는 메모리를 비우므로 다른 목적으로 메모리를 사용할 수 있습니다. 이렇게하면 클래스 인스턴스가 더 이상 필요하지 않을 때 메모리에서 공간을 차지하지 않도록 할 수 있습니다.

그러나 ARC가 여전히 사용중인 인스턴스의 할당을 해제하는 경우 더 이상 해당 인스턴스의 속성에 액세스하거나 해당 인스턴스의 메서드를 호출 할 수 없습니다. 실제로 인스턴스에 액세스하려고하면 응용 프로그램이 중단 될 가능성이 큽니다.

여전히 인스턴스가 필요하지 않을 때 사라지지 않도록 ARC는 현재 각 클래스 인스턴스를 참조하는 속성, 상수 및 변수의 수를 추적합니다. ARC는 해당 인스턴스에 대한 활성 참조가 하나 이상 존재하는 한 인스턴스의 할당을 해제하지 않습니다.

이를 가능하게하려면 클래스 인스턴스를 속성, 상수 또는 변수에 할당 할 때마다 해당 속성, 상수 또는 변수가 인스턴스에 대한 강한 참조를 만듭니다. 이 참조는 해당 인스턴스를 확고하게 유지하기 때문에 "강한"참조라고하며 강한 참조가 남아있는 한 해당 인스턴스의 할당을 해제 할 수 없습니다.

<br />

## ARC in Action

Here’s an example of how Automatic Reference Counting works. This example starts with a simple class called `Person`, which defines a stored constant property called `name`:

자동 참조 카운트가 어떻게 동작되는지에 대한 예제입니다. 이 예제는 상수 프로퍼티  `name` 을 정의하는  `Person` 클래스로 시작합니다.

```swift
class Person {
    let name: String
    init(name: String) {
        self.name = name
        print("\(name) is being initialized")
    }
    deinit {
        print("\(name) is being deinitialized")
    }
}
```



The `Person` class has an initializer that sets the instance’s `name` property and prints a message to indicate that initialization is underway. The `Person` class also has a deinitializer that prints a message when an instance of the class is deallocated.

`Person` 클래스는 인스턴스의 `name` 을 설정하고 초기화가 진행 중임을 나타내는 메시지를 인쇄하는 이니셜라이저가 있습니다. `Person` 클래스에는 클래스의 인스턴스가 할당 해제 될 때 메시지를 인쇄하는 deinitializer도 있습니다.



The next code snippet defines three variables of type `Person?`, which are used to set up multiple references to a new `Person` instance in subsequent code snippets. Because these variables are of an optional type (`Person?`, not `Person`), they are automatically initialized with a value of `nil`, and do not currently reference a `Person` instance.

다음 코드들은 `Person?` 유형의 세 가지 변수를 정의하며, 그 다음 코드들에서 새 `Person` 인스턴스에 대한 다중 참조를 설정하는 데 사용됩니다. 이러한 변수는 옵셔널 타입 (`Person`이 아닌 `Person?`)이기 때문에 값은 자동으로  `nil`로 초기화되고 지금은  `Person` 인스턴스를 참조하지 않습니다.

```swift
var reference1: Person? 
var reference2: Person? 
var reference3: Person? 
```

You can now create a new `Person` instance and assign it to one of these three variables:

이제 새 `Person` 인스턴스를 만들고 이 세 가지 변수 중 하나에 할당 할 수 있습니다.

```swift
reference1 = Person(name: "John Appleseed")
// Prints "John Appleseed is being initialized"
```

Note that the message `"John Appleseed is being initialized"` is printed at the point that you call the `Person` class’s initializer. This confirms that initialization has taken place.

Because the new `Person` instance has been assigned to the `reference1` variable, there is now a strong reference from `reference1` to the new `Person` instance. Because there is at least one strong reference, ARC makes sure that this `Person` is kept in memory and is not deallocated.

``"John Appleseed is initialized"``라는 메시지는 `Person` 클래스의 이니셜라이저를 호출하는 시점에 프린트됩니다. 이것은 초기화가 수행되었음을 확인해줍니다.

새로운 `Person` 인스턴스가 `reference1` 변수에 할당 되었기 때문에 `reference1`에서 새로운 `Person` 인스턴스로의 강한 참조가 있습니다. 적어도 하나의 강한 참조가 있으므로 ARC는 이 `Person`이 메모리에 남아 있고 할당이 해제되지 않았는지 확인합니다.



If you assign the same `Person` instance to two more variables, two more strong references to that instance are established:

동일한 `Person` 인스턴스를 두 개의 다른 변수에 할당하면 해당 인스턴스에 대한 강한 참조가 두 개 더 설정됩니다.

```swift
reference2 = reference1
reference3 = reference1
```

There are now *three* strong references to this single `Person` instance.

If you break two of these strong references (including the original reference) by assigning `nil` to two of the variables, a single strong reference remains, and the `Person` instance is not deallocated:

이제 이 한 `Person` 인스턴스에 대해 세 개의 강한 참조가 있습니다.

두 개의 변수에 `nil`을 할당하여 이러한 강한 참조 중 두 개 (원래 참조 포함)를 끊으면 하나의 강한 참조가 유지되고 `Person` 인스턴스가 할당 해제되지 않습니다.

```swift
reference1 = nil
reference2 = nil
```

ARC does not deallocate the `Person` instance until the third and final strong reference is broken, at which point it’s clear that you are no longer using the `Person` instance:

ARC는 세 번째이자 가장 강한 참조가 깨질 때까지 `Person` 인스턴스를 할당 해제하지 않습니다. 이 때, 더 이상 `Person` 인스턴스를 사용하지 않는다는 것은 확실합니다.

```swift
reference3 = nil
// Prints "John Appleseed is being deinitialized"
```

<br />

## Strong Reference Cycles Between Class Instances

In the examples above, ARC is able to track the number of references to the new `Person`instance you create and to deallocate that `Person` instance when it’s no longer needed.

However, it’s possible to write code in which an instance of a class *never* gets to a point where it has zero strong references. This can happen if two class instances hold a strong reference to each other, such that each instance keeps the other alive. This is known as a *strong reference cycle*.

You resolve strong reference cycles by defining some of the relationships between classes as weak or unowned references instead of as strong references. This process is described in [Resolving Strong Reference Cycles Between Class Instances](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html#ID52). However, before you learn how to resolve a strong reference cycle, it’s useful to understand how such a cycle is caused.

Here’s an example of how a strong reference cycle can be created by accident. This example defines two classes called `Person` and `Apartment`, which model a block of apartments and its residents:

위의 예에서 ARC는 사용자가 작성한 새 `Person` 인스턴스에 대한 참조 수를 추적하고 더 이상 필요없는 `Person` 인스턴스를 할당 해제 할 수 있습니다.

그러나 클래스의 인스턴스가 강한 참조가 0개가 될 수 없는 (즉, 강한 참조가 있을 수 밖에 없는) 코드를 작성할 수도 있습니다. 이것은 두 개의 클래스 인스턴스가 서로에 대한 강한 참조를 보유하여 각 인스턴스가 다른 인스턴스를 활성 상태로 유지하는 경우에 발생할 수 있습니다. 이를 강한 참조 싸이클이라고합니다.

클래스 사이의 관계 중 일부를 강한 참조가 아닌 weak 또는 unowned 참조로 정의하여 강한 참조주기를 해결할 수 있습니다. 이 프로세스는 [클래스 인스턴스 간 강한 참조 싸이클 해결](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html#ID52)에서 설명합니다. 그러나 강한 참조 싸이클을 해결하는 방법을 배우기 전에 그러한 주기가 어떻게 발생 하는지를 이해하는 것이 좋습니다.

우연하게 강한 참조 싸이클이 만들어지는 예제입니다. 이 예제는 아파트 블록과 그 거주자를 모델링하는 `Person` 클래스와 `Apartment `클래스를 정의합니다.

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
}
```

Every `Person` instance has a `name` property of type `String` and an optional `apartment`property that is initially `nil`. The `apartment` property is optional, because a person may not always have an apartment.

Similarly, every `Apartment` instance has a `unit` property of type `String` and has an optional `tenant` property that is initially `nil`. The tenant property is optional because an apartment may not always have a tenant.

Both of these classes also define a deinitializer, which prints the fact that an instance of that class is being deinitialized. This enables you to see whether instances of `Person` and `Apartment` are being deallocated as expected.

This next code snippet defines two variables of optional type called `john` and `unit4A`, which will be set to a specific `Apartment` and `Person` instance below. Both of these variables have an initial value of `nil`, by virtue of being optional:

모든 `Person` 인스턴스에는 `String` 유형의 `name` 프로퍼티와 초기값은 `nil` 인 옵셔널 `apartment` 프로퍼티가 있습니다. 사람이 항상 아파트가 있는 것은 아니기 때문에 아파트 속성은 옵셔널입니다.

마찬가지로 모든 `Apartment` 인스턴스에는 `String` 유형의 `unit` 속성이 있으며 초기에는 `nill` 인 선택적 `tenant`(세입자) 속성이 있습니다. 아파트에 항상 세입자가 있는 것은 아니기 때문에 세입자 프로퍼티는 옵셔널입니다.

이 두 클래스 모두 해당 클래스의 인스턴스가 할당 해제된다는 사실을 인쇄하는 deinitializer를 정의합니다.이렇게하면 `Person` 및 `Apartment의` 인스턴스가 예상대로 할당 해제되는지 여부를 확인할 수 있습니다.

다음 코드들은 `john` 및 `unit4A` 라는 두 가지 옵셔널 타입의 변수를 정의합니다. 이 변수는 아래의 특정 `Apartment` 및 `Person` 인스턴스로 설정될 것입니다. 두 변수의 초기값은 옵셔널의 초기값인  `nil` 입니다.

```swift
var john: Person?
var unit4A: Apartment?
```

You can now create a specific `Person` instance and `Apartment` instance and assign these new instances to the `john` and `unit4A` variables:

이제 특정 `Person` 인스턴스 및 `Apartment` 인스턴스를 생성하고 이러한 새 인스턴스를` john` 및 `unit4A` 변수에 할당할 수 있습니다.

```swift
john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")
```

Here’s how the strong references look after creating and assigning these two instances. The `john` variable now has a strong reference to the new `Person` instance, and the `unit4A`variable has a strong reference to the new `Apartment` instance:

이 두 인스턴스를 만들고 할당하면 강한 참조가 다음과 같이 표시됩니다. 이제 `john` 변수는 새 `Person` 인스턴스를 강하게 참조합니다. `unit4A` 변수는 새 `Apartment` 인스턴스를 강하게 참조합니다.

![](https://docs.swift.org/swift-book/_images/referenceCycle01_2x.png)

You can now link the two instances together so that the person has an apartment, and the apartment has a tenant. Note that an exclamation mark (`!`) is used to unwrap and access the instances stored inside the `john` and `unit4A` optional variables, so that the properties of those instances can be set:

이제 두 인스턴스를 서로 연결하여 사람이 아파트를 가지고 있고 아파트가 입주자를 갖도록 할 수 있습니다. 느낌표(`!`)를 사용하여 `john` 및 `unit4A`  옵셔널 변수 내에 저장된 인스턴스의 포장을 풀고 액세스함으로써 해당 인스턴스의 속성을 설정할 수 있습니다.

```swift
john!.apartment = unit4A
unit4A!.tenant = john
```

Here’s how the strong references look after you link the two instances together:

두 인스턴스를 서로 연결하고 나서 강한 참조는 아래 그림과 같이 됩니다.

![](https://docs.swift.org/swift-book/_images/referenceCycle02_2x.png)

Unfortunately, linking these two instances creates a strong reference cycle between them. The `Person` instance now has a strong reference to the `Apartment` instance, and the `Apartment` instance has a strong reference to the `Person` instance. Therefore, when you break the strong references held by the `john` and `unit4A` variables, the reference counts do not drop to zero, and the instances are not deallocated by ARC:

불행하게도 이 두 인스턴스를 연결하면 두 인스턴스 간에 강한 참조 싸이클이 생성됩니다.  `Person` 인스턴스는 `Apartment` 인스턴스를 강하게 참조하고, `Apartment` 인스턴스는 `Person` 인스턴스를 강하게 참조합니다. 따라서 `john` 및 `unit4A` 변수에 의해 유지되는 강한 참조를 해제해도 참조 수가 0으로 감소하지 않으며 인스턴스는 ARC에 의해 할당 취소되지 않습니다.

```swift
john = nil
unit4A = nil
```



Note that neither deinitializer was called when you set these two variables to `nil`. The strong reference cycle prevents the `Person` and `Apartment` instances from ever being deallocated, causing a memory leak in your app.

Here’s how the strong references look after you set the `john` and `unit4A` variables to `nil`:

이 두 변수를 `nil` 로 설정하면 deinitializer도 호출되지 않습니다. 강한 참조 싸이클은 `Person` 및 `Apartment` 인스턴스가 할당 해제되는 것을 방지하여 앱에서 메모리 누수를 유발합니다.

다음은 `john` 및 `unit4A` 변수를 `nil로` 설정 한 후 강한 참조는 아래 그림과 같이 됩니다.

![](https://docs.swift.org/swift-book/_images/referenceCycle03_2x.png)



The strong references between the `Person` instance and the `Apartment` instance remain and cannot be broken.

`Person` 인스턴스와 `Apartment` 인스턴스 사이의 강한 참조는 유지되며 깨뜨릴 수가 없습니다.



## Resolving Strong Reference Cycles Between Class Instances

Swift provides two ways to resolve strong reference cycles when you work with properties of class type: weak references and unowned references.

Weak and unowned references enable one instance in a reference cycle to refer to the other instance *without* keeping a strong hold on it. The instances can then refer to each other without creating a strong reference cycle.

Use a weak reference when the other instance has a shorter lifetime—that is, when the other instance can be deallocated first. In the `Apartment` example above, it’s appropriate for an apartment to be able to have no tenant at some point in its lifetime, and so a weak reference is an appropriate way to break the reference cycle in this case. In contrast, use an unowned reference when the other instance has the same lifetime or a longer lifetime.

Swift는 클래스 형식의 프로퍼티로 작업 할 때 강한 참조 싸이클을 해결하는 두 가지 방법, weak 참조 및 unowned 참조를 제공합니다.

weak와 unowned 참조는 참조 싸이클의 한 인스턴스가 다른 인스턴스를 강하게 잡고있지 않고 참조할 수 있게합니다. 그런 다음 인스턴스는 강한 참조 싸이클을 만들지 않고도 서로를 참조할 수 있습니다.

다른 인스턴스의 수명이 짧을 때 (즉, 다른 인스턴스를 먼저 할당 해제 할 수 있을 때) 약한 참조를 사용하십시오. 위의 `Apartment` 예제에서는 아파트가 평생동안 세입자를 가질 수는 없기 때문에 이 경우에는 약한 참조가 참조 싸이클을 끊는 적절한 방법입니다. 반대로 다른 인스턴스의 수명이 같거나 수명이 긴 경우에는 unowned 참조를 사용하십시오.

### Weak References

A *weak reference* is a reference that does not keep a strong hold on the instance it refers to, and so does not stop ARC from disposing of the referenced instance. This behavior prevents the reference from becoming part of a strong reference cycle. You indicate a weak reference by placing the `weak` keyword before a property or variable declaration.

Because a weak reference does not keep a strong hold on the instance it refers to, it’s possible for that instance to be deallocated while the weak reference is still referring to it. Therefore, ARC automatically sets a weak reference to `nil` when the instance that it refers to is deallocated. And, because weak references need to allow their value to be changed to `nil`at runtime, they are always declared as variables, rather than constants, of an optional type.

You can check for the existence of a value in the weak reference, just like any other optional value, and you will never end up with a reference to an invalid instance that no longer exists.

약한 참조는 참조하는 인스턴스를 강하게 갖지 않는 참조이며, ARC가 참조된 인스턴스를 삭제하는 것을 막지 않습니다. 이 동작은 참조가 강한 참조 싸이클의 일부가 되지 않도록 합니다. weak 키워드는 프로퍼티 또는 변수 선언 앞에 사용해서 약한 참조를 나타냅니다.

약한 참조가 참조하는 인스턴스를 강하게 잡지 않으므로 약한 참조가 계속 참조하는 동안에도 해당 인스턴스가 할당 해제 될 수 있습니다.  참조하는 인스턴스가 할당 해제되면 ARC는 약한 참조를 nil로 자동 설정합니다. 그리고 약한 참조는 그 값을 런타임시 변경되도록 허용해야하기 때문에 항상 옵셔널 유형의 변수로 선언됩니다.(상수가 아닙니다.)

 다른 옵셔널 값과 마찬가지로 약한 참조에서도 값이 있는지 없는지 확인할 수 있으며 더 이상 존재하지 않는 잘못된 인스턴스에 대한 참조로 끝나는 일은 없습니다.

>  NOTE
>
> Property observers aren’t called when ARC sets a weak reference to `nil`.

> NOTE
>
> 프로퍼티 옵저버는 ARC가 약한 참조를 `nil` 로 설정하면 호출되지 않습니다.

The example below is identical to the `Person` and `Apartment` example from above, with one important difference. This time around, the `Apartment` type’s `tenant` property is declared as a weak reference:

아래의 예제는 위의 `Person` 및 `Apartment` 예제와 동일하지만 중요한 차이점이 하나 있습니다. 이번에는 `Apartment` 유형의 `tenant` 속성이 약한 참조로 선언됩니다.

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    weak var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
```

The strong references from the two variables (`john` and `unit4A`) and the links between the two instances are created as before:

두 변수 (`john` 및 `unit4A`)의 강한 참조와 두 인스턴스 간의 연결은 이전과 같이 만들어집니다.

 ```swift
var john: Person?
var unit4A: Apartment?

john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")

john!.apartment = unit4A
unit4A!.tenant = john
 ```



Here’s how the references look now that you’ve linked the two instances together:

다음은 두 인스턴스를 함께 연결했을 때 참조가 어떻게 되는지 보여줍니다.

![ ](https://docs.swift.org/swift-book/_images/weakReference01_2x.png)

The `Person` instance still has a strong reference to the `Apartment` instance, but the `Apartment` instance now has a *weak* reference to the `Person` instance. This means that when you break the strong reference held by the `john` variable by setting it to `nil`, there are no more strong references to the `Person` instance:

이전과 같이 `Person` 인스턴스는 `Apartment` 인스턴스와 강한 참조를 갖고 있습니다. 그러나 `Apartment` 인스턴스는 `Person` 인스턴스와 약한 참조를 갖고 있습니다. 이것은 `john` 변수를 `nil`로 설정하면서 갖고 있던 강한 참조를 끊을 수 있다는 것을 뜻합니다. `Person` 인스턴스에는 더 이상 강한 참조가 없게 됩니다.

```swift
john = nil 
// Prints "John Appleseed is being deinitialized" 
```

Because there are no more strong references to the `Person` instance, it’s deallocated and the `tenant` property is set to `nil`:

왜냐하면 `Person` 인스턴스에는 더이상 강한 참조가 남아있지 않기 때문에 할당이 해제되고  `tenant` 프로퍼티를 `nil` 로 설정합니다.



![weakReference02_2x](https://docs.swift.org/swift-book/_images/weakReference02_2x.png)

The only remaining strong reference to the `Apartment` instance is from the `unit4A` variable. If you break *that* strong reference, there are no more strong references to the `Apartment`instance:

`Apartment` 인스턴스에 남아있는 유일한 강한 참조는 `unit4A` 변수로 생긴 강한 참조입니다. 만약 이 강한 참조를 깨면, `Apartment` 인스턴스에는 더이상 강한 참조가 존재하지 않게 됩니다. 

```swift
unit4A = nil 
// Prints "Apartment 4A is being deinitialized" 
```



Because there are no more strong references to the `Apartment` instance, it too is deallocated:

`Apartment` 인스턴스에는 더 이상 남아있는 강한 참조가 없기 때문에 이 인스턴스 또한 할당 해제됩니다.

![weakReference03_2x](https://docs.swift.org/swift-book/_images/weakReference03_2x.png)

> NOTE
>
> In systems that use garbage collection, weak pointers are sometimes used to implement a simple caching mechanism because objects with no strong references are deallocated only when memory pressure triggers garbage collection. However, with ARC, values are deallocated as soon as their last strong reference is removed, making weak references unsuitable for such a purpose.

> NOTE
>
> 가비지 콜렉션을 사용하는 시스템에서는 약한 포인터가 간단한 캐싱 메커니즘을 구현하기 위해 사용되기도 합니다. 메모리 참조가 가비지 수집을 발생시킬 때만 강한 참조가 없는 객체의 할당이 해제되기 때문입니다. 그러나 ARC를 사용하면 마지막 강한 참조가 제거 되 자마자 값이 할당 해제되므로 약한 참조를 만드는 것은 이런 목적에 적합하지 않습니다.



### Unowned References

Like a weak reference, an *unowned reference* does not keep a strong hold on the instance it refers to. Unlike a weak reference, however, an unowned reference is used when the other instance has the same lifetime or a longer lifetime. You indicate an unowned reference by placing the `unowned` keyword before a property or variable declaration.

An unowned reference is expected to always have a value. As a result, ARC never sets an unowned reference’s value to `nil`, which means that unowned references are defined using non-optional types.

>  IMPORTANT
>
> Use an unowned reference only when you are sure that the reference *always* refers to an instance that has not been deallocated.
>
> If you try to access the value of an unowned reference after that instance has been deallocated, you’ll get a runtime error.

The following example defines two classes, `Customer` and `CreditCard`, which model a bank customer and a possible credit card for that customer. These two classes each store an instance of the other class as a property. This relationship has the potential to create a strong reference cycle.

The relationship between `Customer` and `CreditCard` is slightly different from the relationship between `Apartment` and `Person` seen in the weak reference example above. In this data model, a customer may or may not have a credit card, but a credit card will *always* be associated with a customer. A `CreditCard` instance never outlives the `Customer` that it refers to. To represent this, the `Customer` class has an optional `card` property, but the `CreditCard`class has an unowned (and non-optional) `customer` property.

Furthermore, a new `CreditCard` instance can *only* be created by passing a `number` value and a `customer` instance to a custom `CreditCard` initializer. This ensures that a `CreditCard`instance always has a `customer` instance associated with it when the `CreditCard` instance is created.

Because a credit card will always have a customer, you define its `customer` property as an unowned reference, to avoid a strong reference cycle:

```swift
class Customer {
    let name: String
    var card: CreditCard?
    init(name: String) {
        self.name = name
    }
    deinit { print("\(name) is being deinitialized") }
}

class CreditCard {
    let number: UInt64
    unowned let customer: Customer
    init(number: UInt64, customer: Customer) {
        self.number = number
        self.customer = customer
    }
    deinit { print("Card #\(number) is being deinitialized") }
}
```



>  NOTE
>
> The `number` property of the `CreditCard` class is defined with a type of `UInt64` rather than `Int`, to ensure that the `number` property’s capacity is large enough to store a 16-digit card number on both 32-bit and 64-bit systems.

This next code snippet defines an optional `Customer` variable called `john`, which will be used to store a reference to a specific customer. This variable has an initial value of nil, by virtue of being optional:

```swift
var john: Customer? 
```

You can now create a `Customer` instance, and use it to initialize and assign a new `CreditCard` instance as that customer’s `card` property:

```swift
john = Customer(name: "John Appleseed") 
john!.card = CreditCard(number: 1234_5678_9012_3456, customer: john!) 
```

Here’s how the references look, now that you’ve linked the two instances:

![unownedReference01_2x](https://docs.swift.org/swift-book/_images/unownedReference01_2x.png)

The `Customer` instance now has a strong reference to the `CreditCard` instance, and the `CreditCard` instance has an unowned reference to the `Customer` instance.

Because of the unowned `customer` reference, when you break the strong reference held by the `john` variable, there are no more strong references to the `Customer` instance:

![unownedReference02_2x](https://docs.swift.org/swift-book/_images/unownedReference02_2x.png)

Because there are no more strong references to the `Customer` instance, it’s deallocated. After this happens, there are no more strong references to the `CreditCard` instance, and it too is deallocated:
```swift
john = nil 
// Prints "John Appleseed is being deinitialized" 
// Prints "Card #1234567890123456 is being deinitialized" 
```

The final code snippet above shows that the deinitializers for the `Customer` instance and `CreditCard` instance both print their “deinitialized” messages after the `john` variable is set to `nil`.

>  NOTE
>
> The examples above show how to use *safe* unowned references. Swift also provides *unsafe*unowned references for cases where you need to disable runtime safety checks—for example, for performance reasons. As with all unsafe operations, you take on the responsibility for checking that code for safety.
>
> You indicate an unsafe unowned reference by writing `unowned(unsafe)`. If you try to access an unsafe unowned reference after the instance that it refers to is deallocated, your program will try to access the memory location where the instance used to be, which is an unsafe operation.