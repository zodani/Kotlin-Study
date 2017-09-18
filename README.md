<img src="https://cdn.worldvectorlogo.com/logos/kotlin-2.svg">

### Kotlin 컴파일 속도
- 대개 Java보다 조금 빠르다!
- 클린 빌드 시에는 Java보다 조금 느리지만, 일반적인 개발 시나리오의 증분 빌드 시 성능이 좋다.
- [Kotlin vs Java: Compilation speed](https://medium.com/keepsafe-engineering/kotlin-vs-java-compilation-speed-e6c174b39b5d)

### 세미콜론(;) 안녕
- 더 이상 세미콜론은 필요가 없다.
```kotlin
fun main(args: Array<String>) {
    val age = 30
    val name: String = "Seo Jaeryong"
    println(name.length)
}
```

### 변수는 모두 val 아니면 var
- val : 값 변경 불가능 (read-only)
```kotlin
fun main(args: Array<String>) {
    val age: Int = 30
    a = 20 // 실패
}
```
- var : 값 변경 가능 (read-write)
```kotlin
fun main(args: Array<String>) {
    var age: Int = 30
    a = 20 // 성공
}
```
### 접근자
- `public (default)` : 전역 프로젝트에 공개
- `private` : 같은 파일내에 공개
- `protected` : Subclasses에 공개
- `internal` : 같은 Module내에 공개
```
Module이란?
- IntelliJ an IntelliJ IDEA module
- a Maven project
- a Gradle source set
- a set of files compiled with one invocation of the Ant task.
```
### 자동 형변환 (Smart Casts)
- is 체크 후 (Java의 instanceof)
```kotlin
fun demo(x: Any) {
    if (x is String) {
        print(x.length) // x가 자동으로 String으로 형변환 된다.
    }
}
```
- null 체크 후
```kotlin
fun demo1(x: String?) {
    if (x != null) {
        demo2(x) // x가 자동으로 NonNull String으로 형변환 된다.
    }
}

fun demo2(x: String) {
    print(x.length)
}
```
### 함수 값 리턴의 간략화
- 일반적인 함수 정의
```kotlin
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}
```
- 간략하게 표현
```kotlin
fun maxOf(a: Int, b: Int): Int = if (a > b) a else b
```
- 더 간략하게 표현 (리턴 타입을 생략해도 추론 가능)
```kotlin
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```
### For-Loop
- List
```kotlin
val items = listOf("apple", "banana", "kiwi")
for (index in items.indices) {
    println("item at $index is ${items[index]}")
}

결과
item at 0 is apple
item at 1 is banana
item at 2 is kiwi
```
- Range (a...b)
```kotlin
for (i in 0..10) { 
    print(i)
}

결과
012345678910
```
### While-Loop
```kotlin
val items = listOf("apple", "banana", "kiwi")
var index = 0
while (index < items.size) {
    println("item at $index is ${items[index]}")
    index++
}

결과
item at 0 is apple
item at 1 is banana
item at 2 is kiwi
```
### When
- 다양한 타입 비교
```kotlin
when (obj) {
    1          -> "One"
    "Hello"    -> "Greeting"
    is Long    -> "Long"
    !is String -> "Not a string"
    else       -> "Unknown"
}
```
- {} 블록을 지정해서 작성
```kotlin
when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> { // Note the block
        print("x is neither 1 nor 2")
    }
}
```
- 한 조건에 여러 값을 비교 (0, 1)
```kotlin
when (x) {
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}
```
- 범위 비교
```kotlin
when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}
```
### infix notation(중위 표기법)
- 함수앞에 infix를 붙인다.
- 멤버함수 혹은 확장 함수([extension funtions](https://kotlinlang.org/docs/reference/extensions.html))에 사용
- 하나의 파라미터를 받는 함수에서 사용
```kotlin
// 정의 방법
infix fun Int.shl(x: Int): Int {
    ...
}

1.shl(2)
1 shl 2 // can also be called like this
```
### Extensions
- 클래스에 함수 확장
```kotlin
// ViewExt.kt
fun View.show() {
    visibility = View.VISIBLE
}

fun View.hide() {
    visibility = View.GONE
}

// SearchActivity.kt
var textView = findViewById(R.id.textView) as TextView
textView.show() // ok
textView.hide() // ok
```
## 클래스
### 클래스 표현
```kotlin
class Invoice {

}
```
- 바디가 없을 때 {} 생략가능
```kotlin
class Empty
```
- 생성자 표현 (constructor 키워드)
```kotlin
class Person constructor(firstName: String) {

}
```
- 생성자 표현에 constructor 키워드 생략 가능
```kotlin
class Person(firstName: String) {

}
```
- 생성자의 초기화 블록 지정 (init 키워드)
```kotlin
class Customer(name: String) {
    init {
        logger.info("Customer initialized with value ${name}")
    }
}
```
- 위 표현을 아래와 같이 표현 가능 (동일)
```kotlin
class Customer {
    constructor(name: String) {
		logger.info("Customer initialized with value ${name}")
    }
}
```
- @Inject 어노테이션이 필요하면 constructor 키워드가 필요하다.
```kotlin
class Customer public @Inject constructor(name: String) { ... }
```
### Data 클래스
- 모든 var, val 변수의 Getter 제공
- 모든 var 변수의 Setter를 제공
- equals() / hashCode() / toString() / copy() 구현을 아름답게 제공
```kotlin
data class Customer(val name: String, var email: String)
```
### Open 클래스
- 코틀린의 모든 클래스는 기본적으로 final이라 상속이 불가능하다.
- open 키워드를 class 앞에 붙여줌으로써 상속을 허용시킨다.
```kotlin
open class Base(p: Int)

class Derived(p: Int) : Base(p)
```
### Abstract 클래스
- Java와 비슷
```kotlin
abstract class Base {
    abstract fun f()
}

class Derived() : Base() {
    override fun f() {
        // ...
    }
}
```
### Nested 클래스
- Outer클래스 멤버 참조가 `불가능`
```kotlin
class Outer {
    private val bar: Int = 1
    class Nested {
        fun foo() = 2 // bar 참조 불가
    }
}

val demo = Outer.Nested().foo() // == 2
```
### Inner 클래스
- Outer클래스 멤버 참조 `가능`
```kotlin
class Outer {
    private val bar: Int = 1
    inner class Inner {
        fun foo() = bar // bar 참조 가능
    }
}

val demo = Outer().Inner().foo() // == 1
```
### 익명 Inner 클래스
`object`키워드를 사용하고 타입은 `interface`나 `abstract class`를 받는다.
- `interface` : 이름 뒤에 ()를 붙이지 않는다. `View.OnClickListener`
```kotiln
button.setOnClickListener(object: View.OnClickListener {
    override fun onClick(view: View) {
        // ...
    }
})
```
- `abstract class` : 이름 뒤에 ()를 붙인다. `SimpleOnQueryTextListener()`
```kotlin
searchView.setOnQueryTextListener(object : SimpleOnQueryTextListener() {
    override fun onQueryTextSubmit(query: String): Boolean {
        presenter.searchImage(query)
        return false
    }
})
```
