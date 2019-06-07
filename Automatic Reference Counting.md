# Automatic Reference Counting

Swift uses *Automatic Reference Counting* (ARC) to track and manage your app’s memory usage. In most cases, this means that memory management “just works” in Swift, and you do not need to think about memory management yourself. ARC automatically frees up the memory used by class instances when those instances are no longer needed.

However, in a few cases ARC requires more information about the relationships between parts of your code in order to manage memory for you. This chapter describes those situations and shows how you enable ARC to manage all of your app’s memory. Using ARC in Swift is very similar to the approach described in [Transitioning to ARC Release Notes](https://developer.apple.com/library/content/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html) for using ARC with Objective-C.

Reference counting applies only to instances of classes. Structures and enumerations are value types, not reference types, and are not stored and passed by reference.

Swift는 ARC (Automatic Reference Counting)를 사용하여 앱의 메모리 사용을 추적하고 관리합니다. 대부분의 경우, 이는 Swift에서 메모리 관리가 "단지 작동합니다"는 것을 의미하며 메모리 관리에 대해 직접 생각할 필요가 없습니다. ARC는 클래스 인스턴스가 더 이상 필요하지 않을 때 클래스 인스턴스가 사용하는 메모리를 자동으로 비 웁니다.

그러나 몇 가지 경우 ARC는 메모리를 관리하기 위해 코드 부분 간의 관계에 대한 추가 정보가 필요합니다. 이 장에서는 이러한 상황에 대해 설명하고 ARC가 모든 앱의 메모리를 관리하는 방법을 보여줍니다. Swift에서 ARC를 사용하는 것은 Objective-C와 함께 ARC를 사용하기 위해 ARC 릴리스 노트로 전환에서 설명한 방법과 매우 유사합니다.

참조 카운팅은 클래스의 인스턴스에만 적용됩니다. 구조와 열거 형은 참조 유형이 아닌 값 유형이며 참조로 저장되고 전달되지 않습니다.



## How ARC Works

Every time you create a new instance of a class, ARC allocates a chunk of memory to store information about that instance. This memory holds information about the type of the instance, together with the values of any stored properties associated with that instance.

Additionally, when an instance is no longer needed, ARC frees up the memory used by that instance so that the memory can be used for other purposes instead. This ensures that class instances do not take up space in memory when they are no longer needed.

However, if ARC were to deallocate an instance that was still in use, it would no longer be possible to access that instance’s properties, or call that instance’s methods. Indeed, if you tried to access the instance, your app would most likely crash.

To make sure that instances don’t disappear while they are still needed, ARC tracks how many properties, constants, and variables are currently referring to each class instance. ARC will not deallocate an instance as long as at least one active reference to that instance still exists.

To make this possible, whenever you assign a class instance to a property, constant, or variable, that property, constant, or variable makes a *strong reference* to the instance. The reference is called a “strong” reference because it keeps a firm hold on that instance, and does not allow it to be deallocated for as long as that strong reference remains.

클래스의 새 인스턴스를 만들 때마다 ARC는 해당 인스턴스에 대한 정보를 저장하기 위해 메모리 덩어리를 할당합니다. 이 메모리에는 해당 인스턴스와 관련된 저장된 속성의 값과 함께 인스턴스 유형에 대한 정보가 저장됩니다.

또한 인스턴스가 더 이상 필요하지 않을 때 ARC는 해당 인스턴스가 사용하는 메모리를 비우므로 다른 목적으로 메모리를 사용할 수 있습니다. 이렇게하면 클래스 인스턴스가 더 이상 필요하지 않을 때 메모리에서 공간을 차지하지 않도록 할 수 있습니다.

그러나 ARC가 여전히 사용중인 인스턴스의 할당을 해제하는 경우 더 이상 해당 인스턴스의 속성에 액세스하거나 해당 인스턴스의 메서드를 호출 할 수 없습니다. 실제로 인스턴스에 액세스하려고하면 응용 프로그램이 중단 될 가능성이 큽니다.

여전히 인스턴스가 필요하지 않을 때 사라지지 않도록 ARC는 현재 각 클래스 인스턴스를 참조하는 속성, 상수 및 변수의 수를 추적합니다. ARC는 해당 인스턴스에 대한 활성 참조가 하나 이상 존재하는 한 인스턴스의 할당을 해제하지 않습니다.

이를 가능하게하려면 클래스 인스턴스를 속성, 상수 또는 변수에 할당 할 때마다 해당 속성, 상수 또는 변수가 인스턴스에 대한 강력한 참조를 만듭니다. 이 참조는 해당 인스턴스를 확고하게 유지하기 때문에 "강력한"참조라고하며 강력한 참조가 남아있는 한 해당 인스턴스의 할당을 해제 할 수 없으므로 참조하십시오.