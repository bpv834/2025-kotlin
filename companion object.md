#  Companion Object vs Static

## Companion Object

```kotlin
class Student {
    var id: String? = null
    var name: String? = null

    companion object {
        val national = "korea"

        fun printNational() {
            println(national)
        }
    }
}

fun main() {
    println(Student.Companion.national)
    Student.Companion.printNational()

    val comp = Student.Companion
    println(comp.national)
    comp.printNational()

    val comp2 = Student
    println(comp2.national)
    comp2.printNational()
}
```
- Kotlin은 static 키워드가 없으며, 대신 Companion Object로 정적 멤버를 정의한다.

- Companion Object는 클래스가 로딩될 때 생성되는 객체이다.

- 클래스 내부에서 하나만 선언 가능하다.

- Companion Object 내부 변수는 클래스 멤버에는 직접 접근 불가하다.

- Companion Object는 객체이므로 변수에 할당 가능하며 참조형으로 사용할 수 있다.

## JAVA Decompile

```java
public final class Student {
   @Nullable
   private String id;
   @Nullable
   private String name;
   @NotNull
   private static final String national = "korea";
   public static final Student.Companion Companion = new Student.Companion((DefaultConstructorMarker)null);

   @Nullable
   public final String getId() {
      return this.id;
   }

   public final void setId(@Nullable String var1) {
      this.id = var1;
   }

   @Nullable
   public final String getName() {
      return this.name;
   }

   public final void setName(@Nullable String var1) {
      this.name = var1;
   }

   public static final class Companion {
      @NotNull
      public final String getNational() {
         return Student.national;
      }

      public final void printnational() {
         String var1 = ((Student.Companion)this).getNational();
         boolean var2 = false;
         System.out.println(var1);
      }

      private Companion() {
      }

      // $FF: synthetic method
      public Companion(DefaultConstructorMarker $constructor_marker) {
         this();
      }
   }
}
```
- Student 클래스 내부에 static으로 Companion 클래스를 생성하는 것을 확인할 수 있습니다.
- 이를 통해 Companion 객체는 클래스가 생성될 때 메모리에 적재되면서 동시에 생성하는 객체라는 것을 알 수 있습니다.

### static과 companion object 비교
| 항목         | Java `static`            | Kotlin `companion object`                          |
| ---------- | ------------------------ | -------------------------------------------------- |
| 선언 방식      | `static` 키워드 사용          | `companion object { ... }`                         |
| 접근         | `ClassName.staticMember` | `ClassName.member` 또는 `ClassName.Companion.member` |
| 객체 형태      | 클래스 변수/메서드               | 객체 인스턴스로 존재                                        |
| 생성 시점      | 클래스 로딩 시점                | 클래스 로딩 시 Companion 객체 생성                           |
| 인스턴스 멤버 접근 | 불가능                      | 불가능                                                |
| 클래스 내 개수   | 여러 static 가능             | 클래스당 하나의 companion object                          |
| 테스트/DI     | 전역 접근 → 유연성 낮음           | 객체이므로 DI, 모킹 가능                                    |
| 사용 목적      | 공통 데이터 관리, 상수 정의         | 정적 멤버 관리 + 객체 기반 확장 가능                             |

출처 : https://math-coding.tistory.com/214
