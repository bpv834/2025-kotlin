#  Kotlin `object` 키워드 완전 정리

`object` 키워드는 **클래스를 정의하면서 동시에 인스턴스(객체)를 생성**할 때 사용하는 Kotlin의 강력한 문법이다.  
Kotlin에서는 다음 **두 가지 형태**로 사용할 수 있다.

- **Object Declaration (선언식)** — 싱글톤(Singleton) 객체 생성  
- **Object Expression (표현식)** — 익명 객체(Anonymous Object) 생성

---

##  1. Object Declaration (선언식)

`object`를 선언식으로 사용하면 **싱글톤(Singleton)** 형태의 객체를 만든다.  
즉, 프로그램 전체에서 **단 하나의 인스턴스만 존재**하는 객체를 정의할 수 있다.

###  Java에서의 Singleton 구현

```java
public class Singleton {
    private static Singleton INSTANCE;

    private Singleton() { }

    public static Singleton getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }
}
```
### kotlin에서의 singleton 구현

```kotlin
코틀린에서 object를 선언식으로 사용하는 문법은 아래와 같다.

object Singleton{
	...
}

// 사용 방법
val singleton = Singleton
```

#### Object 내에 class와 같이 멤버 변수/ 메서드를 가질 수 있으며, class와 interface의 상속이 가능하다.
```kotlin
object Singleton: MyClass(), MyInterface { // class, interface 상속 가능
	
	private val name: String = "Molrhmbo" //  멤버 변수를 가질 수 있다.
	
	override fun MyClassFunc(){
		...
	}
}
```
---
##  2. Object Expression (표현식): Non-Singleton

`object`를 **표현식(Expression)** 으로 사용하면,  
**싱글톤이 아닌 익명 객체(Anonymous Object)** 를 생성할 수 있다.

> 즉, `object` 선언식이 전역적으로 단 하나의 객체를 만드는 것과 달리,  
> **표현식은 호출될 때마다 새로운 객체 인스턴스를 생성**한다.

---

###  익명 객체 (Anonymous Object)

익명 객체는 이름이 없는 클래스의 인스턴스를 **즉시 생성**할 때 사용한다.  
대표적인 예시는 **이벤트 리스너(Listener)** 나 **콜백(Callback)** 을 구현할 때이다.

---

###  예제 1: 익명 클래스의 객체를 바로 생성하는 경우

```kotlin
//  익명 클래스의 객체를 바로 생성하고자 하는 경우
fun main() {
    val user = object {
        val name = "Molrhmbo"
        val age = 28
    }

    println(user.name)
    println(user.age)
}
```
- 추상 클래스, 인터페이스의 구현체를 익명 클래스의 객체로 구현하고자 하는 경우.
```kotlin
//  추상 클래스, 인터페이스의 구현체를 익명 클래스의 객체로 구현하고자 하는 경우.
interface MyInterface{
    val name:String
    val age: Int
    fun greeting()
}

val myListener= object: MyInterface{
    override val name = "Molrhmbo"
    override val age = 28
   	override fun greeting(){
       	println("my name is ${name}")
       	println("and I'm ${age} years old!")
   	}
}

fun main() {
    myListener.greeting()
}
```
- 익명 객체로 리스너 구현
  
```kotlin
/*
* 익명 객체로 리스너 구현하기
*/
binding.countingButton.setOnClickListener(
	object: View.OnClickListener { //  익명 객체로 클릭 리스너 선언
		override fun OnClick(v: View?) {
			...
		}
	}
)
```
- TIP : object 익명 객체의 사용은 여러 메소드를 오버라이드 해야하는 경우에 훨씬 더 유용하다.

##  메서드가 여러 개인 경우 — 람다식 vs 익명 객체 비교

람다식은 **“하나의 추상 메서드(SAM)”** 를 가진 인터페이스에서만 사용할 수 있다.  
즉, **메서드가 여러 개인 인터페이스**에서는 **람다식으로 구현이 불가능하다.**  
이때는 **익명 객체(Object Expression)** 을 사용해야 한다.

---

###  예제 1: 메서드가 여러 개인 인터페이스

```kotlin
interface MyListener {
    fun onClick()
    fun onLongClick()
}

// 에러 발생 ❌
binding.button.setOnClickListener { 
    println("clicked!") 
}


binding.button.setListener(
    object : MyListener {
        override fun onClick() {
            println("short click!")
        }

        override fun onLongClick() {
            println("long click!")
        }
    }
)
```
