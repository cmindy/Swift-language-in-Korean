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