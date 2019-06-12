# Automatic Reference Counting

Swift uses *Automatic Reference Counting* (ARC) to track and manage your app’s memory usage. In most cases, this means that memory management “just works” in Swift, and you do not need to think about memory management yourself. ARC automatically frees up the memory used by class instances when those instances are no longer needed.

However, in a few cases ARC requires more information about the relationships between parts of your code in order to manage memory for you. This chapter describes those situations and shows how you enable ARC to manage all of your app’s memory. Using ARC in Swift is very similar to the approach described in [Transitioning to ARC Release Notes](https://developer.apple.com/library/content/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html) for using ARC with Objective-C.

Reference counting applies only to instances of classes. Structures and enumerations are value types, not reference types, and are not stored and passed by reference.

Swift는 ARC (Automatic Reference Counting)를 사용하여 앱의 메모리 사용을 추적하고 관리합니다. 대부분의 경우, 이는 Swift에서 메모리 관리가 "just works"하고 있다는 것을 의미하며 메모리 관리에 대해 직접 생각할 필요가 없습니다. ARC는 클래스 인스턴스가 더 이상 필요하지 않을 때 클래스 인스턴스가 사용하는 메모리를 자동으로 비웁니다.

그러나 몇 가지 경우 ARC는 메모리를 관리하기 위해 코드들 간의 관계에 대한 추가 정보가 필요합니다. 이 장에서는 이러한 상황에 대해 설명하고 ARC가 모든 앱의 메모리를 관리하는 방법을 보여줍니다. Swift에서 ARC를 사용하는 것은 Objective-C와 함께 ARC를 사용하기 위해 ARC 릴리스 노트로 전환에서 설명한 방법과 매우 유사합니다.

참조 카운팅은 클래스의 인스턴스에만 적용됩니다. 구조체와 열거형은 참조 타입이 아닌 값 타입이며 참조로 저장되고 전달되지 않습니다.



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

