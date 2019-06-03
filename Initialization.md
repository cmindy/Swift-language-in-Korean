# Initialization

*Initialization* is the process of preparing an instance of a class, structure, or enumeration for use. This process involves setting an initial value for each stored property on that instance and performing any other setup or initialization that is required before the new instance is ready for use.

*생성자* 는 클래스, 구조체, 열거형의 인스턴스를 사용하기 위해 준비하는 프로세스입니다. 이 프로세스에는 해당 인스턴스에서 저장된 각 프로퍼티에 대한 초기 값을 설정하고 새 인스턴스를 사용하기 전에 필요한 다른 설정이나 초기화를 수행해야합니다.



You implement this initialization process by defining *initializers*, which are like special methods that can be called to create a new instance of a particular type. Unlike Objective-C initializers, Swift initializers do not return a value. Their primary role is to ensure that new instances of a type are correctly initialized before they are used for the first time.

이 초기화 프로세스는 특정 타입의 새 인스턴스를 만들기 위해 호출 할 수 있는 특수 메서드인 초기화 메서드를 정의하여 구현합니다. Objective-C 이니셜라이저와 달리 Swift 이니셜 라이저는 값을 반환하지 않습니다. 그들의 주요 역할은 타입의 새 인스턴스가 처음 사용되기 전에 올바르게 초기화 되도록 하는 것입니다.



Instances of class types can also implement a *deinitializer*, which performs any custom cleanup just before an instance of that class is deallocated. For more information about deinitializers, see [Deinitialization](https://docs.swift.org/swift-book/LanguageGuide/Deinitialization.html).

클래스 유형의 인스턴스는 해당 클래스의 인스턴스가 deallocated 되기 직전에 custom cleanup을 수행하는 deinitializer를 구현할 수도 있습니다. deinitializers에 대한 자세한 내용은 [Deinitialization](https://docs.swift.org/swift-book/LanguageGuide/Deinitialization.html)을 참조하십시오.



## Setting Initial Values for Stored Properties

Classes and structures *must* set all of their stored properties to an appropriate initial value by the time an instance of that class or structure is created. Stored properties cannot be left in an indeterminate state.

클래스 및 구조체는 해당 클래스 또는 구조체의 인스턴스가 만들어 질 때까지 저장된 모든 프로퍼티를 적절한 초기 값으로 설정해야합니다. 저장 프로퍼티는 확정되지 않은 상태로 남겨 둘 수 없습니다.



You can set an initial value for a stored property within an initializer, or by assigning a default property value as part of the property’s definition. These actions are described in the following sections.

초기화 프로그램 내의 저장 프로퍼티에 대한 초기 값을 설정하거나 프로퍼티 정의의 일부로 defatault 값을 할당하여 초기 값을 설정할 수 있습니다. 이러한 작업은 다음 섹션에서 설명합니다.



> NOTE
>
> When you assign a default value to a stored property, or set its initial value within an initializer, the value of that property is set directly, without calling any property observers.

> NOTE
>
> 저장 프로퍼티에 기본값을 할당하거나 이니셜라이저 내에서 초기값을 설정하면 프로퍼티 옵저버를 호출하지 않고 해당 속성의 값이 직접 설정됩니다.