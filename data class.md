
# 🧩 Data Class

## 📘 Data Class (데이터 클래스) 개념
- 일반 클래스와 달리, 다양한 메서드를 **자동으로 생성**해주는 클래스이다.
- 데이터를 담기 위한 용도로 주로 사용된다.

---

### ⚙️ Data Class 생성 시 자동으로 만들어지는 메서드

| 메서드 | 설명 |
|--------|------|
| `hashCode()` | 객체의 해시코드를 자동으로 생성 |
| `copy()` | 일부 속성만 변경한 새 객체를 쉽게 복사 |
| `equals()` | 참조가 아닌 **값 기반 비교** 가능 |
| `toString()` | 객체 내용을 문자열로 반환 |
| `componentN()` | 구조 분해 선언에 사용 (`val (name, age) = user`) |

---

### 🧱 예시
```java
class People {
    String name;
    int age;

    @Override
    public String toString(){
        return "[People] name : " + name + ", age : " + Integer.toString(age);
    }

    public String getName(){
        return name;
    }

    public void setName(String name){
        this.name = name;
    }
}

```
위처럼 toString 오버라이딩해줘야 객체 상태를 볼 수 있었다.
하지만 data class를 사용한다면 바로 출력이 가능하다!

```kotlin
data class People(
    val name: String,
    val age: Int
)

fun main() {
    val p1 = People("홍길동", 20)
    val p2 = People.copy(age = 25)
    println(p1) // People(name=홍길동, age=20)
    println(p2) // People(name=홍길동, age=25)
}


```
## 또한 아래와 같은 여러 가지 다양한 특징을 갖고 있다.

- 기본 생성자에 1개 이상의 파라미터가 있어야 함
- 기본 생성자의 파라미터가 val 또는 var 로 선언해야 함
- 다른 클래스를 상속받을 수 없음 (슈퍼 클래스를 가질 수 없음)
- 단, sealed 클래스는 상속받을 수 있으며, 인터페이스는 구현할 수 있음 (v1.1 이후 기준)
- abstract, open, sealed, inner 등 키워드를 붙일 수 없음
- 자동으로 생성한 메소드를 오버라이딩할 경우, 오버라이드 된 메소드 사용

## copy() 메소드
copy() 메소드 역시 사용할 수 있다. 특정 필드값만 바꿔서 복사하기에 간편하다.

```
fun main() {
    val peopleA = People("H43RO", 23)
    val peopleB = People("LULU", 21)
}

val olderPeopleA = peopleA.copy(age = 33)
println(olderPeopleA)

// People(name=H43RO, age=33) -> 나이가 23 에서 33 으로 변경되었음!
```

## hashCode() 메소드

프로퍼티 값이 완전히 같은 두 Data Class 객체를 만들고, hashCode() 를 출력해보자.

```kotlin
val peopleA = People("H43RO", 23)
val peopleB = People("H43RO", 23)

println(peopleA.hashCode())
println(peopleB.hashCode())
2110922579
2110922579

두 객체는 서로 다른 인스턴스(참조 주소는 다름) 지만,
값이 완전히 동일하기 때문에 equals()는 true, hashCode()도 같게 나옵니다.


```

## equals() 메소드

```kotlin
println(peopleA == peopleB)

// true (두 객체의 프로퍼티가 완전히 같음)
```

## componentN() 메소드

```
data class People(
    val name: String,
    val age: Int
)
```

## 데이터 클래스 상속을 막는 이유

Kotlin의 data class는 값 비교와 복사를 위해 equals, hashCode, copy 등을 자동 생성하는데, 상속을 허용하면 이 메서드들의 일관성이 깨질 수 있습니다.
그래서 컴파일러는 데이터의 불변성과 일관성을 보장하기 위해 data class를 final로 만들어 상속을 금지합니다.

이 클래스는 프로퍼티가 2개이다. 이 때 선언 순서가 name 다음에 age 형태로 되어있기 때문에, component1() 메소드에 name 필드가 대응되고, component2() 메소드에 age 필드가 대응되게 된다. 

<img width="586" height="95" alt="image" src="https://github.com/user-attachments/assets/60d6d78f-0430-45c8-be19-d050ee394b88" />

출처 : https://velog.io/@haero_kim/Kotlin-%EA%B0%90%EB%8F%99-%EC%8B%A4%ED%99%94-Data-Class-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0

