<img src="https://cdn.worldvectorlogo.com/logos/kotlin-2.svg">

### Kotlin 컴파일 속도
- 대개 Java보다 조금 빠르다!
- 클린 빌드 시에는 Java보다 조금 느리지만, 일반적인 개발 시나리오의 증분 빌드 시 성능이 좋다.
- [Kotlin vs Java: Compilation speed](https://medium.com/keepsafe-engineering/kotlin-vs-java-compilation-speed-e6c174b39b5d)

### 세미콜론 제거
```kotlin
fun main(args: Array<String>) {
    val age = 30
    val name: String = "Seo Jaeryong"
    println(name.length)
}
```

### 변수는 모두 val 아니면 var
- val : 변경 불가능 (read-only)
```kotlin
fun main(args: Array<String>) {
    val age: Int = 30
    a = 20 // error
}
```
- var : 변경 가능 (read-write)
```kotlin
fun main(args: Array<String>) {
    var age: Int = 30
    a = 20 // ok
}
```
### 함수 값 리턴의 간략화
- 일반적인 함수
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
### Nullsafe
- Kotlin은 널 참조의 위험([The Billion Dollar Mistake](https://en.wikipedia.org/wiki/Tony_Hoare#Apologies_and_retractions))을 제거하기 위해 노력했다.
- 아래와 같은 상황에서만 NPE(NullPointerException)를 만들 수 있다.
  - 명시적인 `throw NullPointerException()` 호출
  - `!!` 오퍼레이터 사용
  - NPE를 발생시키는 외부 Java 코드 호출
- Non-null
```kotlin
var a: String = "abc"
a = null // error
```
- Nullable(?)
```kotlin
var b: String? = "abc"
b = null // ok
```
- '?.' operator (for nullable)
```kotlin
var b: String? = "abc"
b.length // error
b?.length // ok
```
- 여러 체인의 객체를 호출할 때 유용하다.
```kotlin
// Java
String name;
if (bob != null) {
    if (department != null) {
        if (head != null) {
	    name = bob.department.head.name;
	}
    }
}

// Kotlin
var name: String = bob?.department?.head?.name ?: "" // ok
```
- `!!` operator (for nullable)
  - for `NPE-lovers`.
```kotlin
var b: String? = null
val l = b!!.length // ok, but throw an NPE if b is null
```
- Safe casts
```kotlin
val aInt: Int? = a as? Int // return `null` if the attempt was not successful
```
- Collections of Nullable Type
```kotlin
val nullableList: List<Int?> = listOf(1, 2, null, 4)
val intList: List<Int> = nullableList.filterNotNull()
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
### 클래스
- 기본
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
- Data 클래스
  - 모든 var, val 변수의 Getter 제공
  - 모든 var 변수의 Setter를 제공
  - equals() / hashCode() / toString() / copy() 구현을 아름답게 제공
```kotlin
data class Customer(val name: String, var email: String)
```
- Open 클래스
  - 코틀린의 모든 클래스는 기본적으로 final이라 상속이 불가능하다.
  - open 키워드를 class 앞에 붙여줌으로써 상속을 허용시킨다.
```kotlin
open class Base(p: Int)

class Derived(p: Int) : Base(p)
```
- Abstract 클래스 (open 붙일 필요 없음)
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
- Nested 클래스
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
- Inner 클래스
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
- 익명 Inner 클래스
  - `object`키워드를 사용하고 타입은 `interface`나 `abstract class`를 받는다.
  - `interface` : 이름 뒤에 ()를 붙이지 않는다. `View.OnClickListener`
  - `abstract class` : 이름 뒤에 ()를 붙인다. `SimpleOnQueryTextListener()`
```kotiln
// interface
button.setOnClickListener(object : View.OnClickListener {
    override fun onClick(view: View) {
        // ...
    }
})

// abstract class
searchView.setOnQueryTextListener(object : SimpleOnQueryTextListener() {
    override fun onQueryTextSubmit(query: String): Boolean {
        presenter.searchImage(query)
        return false
    }
})
```
- Enum 클래스
```kotlin
enum class Direction {
    NORTH, SOUTH, WEST, EAST
}
enum class Color(val rgb: Int) {
    RED(0xFF0000),
    GREEN(0x00FF00),
    BLUE(0x0000FF)
}
```
- 클래스 위임 (Class Delegation)
  - 해당 클래스안에 by절 `뒤`에 오는 참조가 `private으로 저장`된다.
  - 해당 클래스안에 by절 `앞`에 오는 인터페이스의 `메소드를 자동 생성`한다.
```kotlin
interface Base {
    fun print()
}

class BaseImpl(val x: Int) : Base {
    override fun print() { print(x) }
}

// b가 Derived내에 private으로 저장 됨
// Base의 메소드를 Derived내에 자동 생성한다.
// 그 메소드들은 b를 참조하여 실행한다.
class Derived(b: Base) : Base by b 

fun main(args: Array<String>) {
    val b = BaseImpl(10)
    Derived(b).print() // prints 10
}
```
### Destructuring Declarations
```kotlin
// class
data class Person(val name: String, val age: Int)
fun main(args: Array<String>) {
	val (name, age) = Person("Jee-ryong", 30)
	print("My name is $name and I am $age years old.")
}

// map
for ((key, value) in map) {
    print("key is $key")
    print("value is $value")
}
```
### List
- [mutableListOf](https://github.com/seojr/kotlin/blob/master/libraries/stdlib/src/kotlin/collections/Collections.kt#L96), [arrayListOf](https://github.com/JetBrains/kotlin/blob/master/libraries/stdlib/src/kotlin/collections/Collections.kt#L101) 둘 다 ArrayList를 만들어 리턴
- ArrayList보다 kotlin스타일로 리스트를 다루는 인터페이스가 구현되어 있는 MutableList를 사용하는 편이 더 나아보임.
```kotlin
val lists: List<Int> = listOf(1, 2, 3) // read only
val lists: MutableList<Int> = mutableListOf(1, 2, 3) // read/write
val lists: ArrayList<Int> = arrayListOf(1, 2, 3) // read/write
```
### Map
```kotlin
// new instance
val map = mapOf("Korea" to 1, "Japan" to 2) // read only
val map = mutableMapOf("Korea" to 1, "Japan" to 2) // read/write
val map = linkedMapOf("Korea" to 1, "Japan" to 2)
val map = hashMapOf("Korea" to 1, "Japan" to 2) 
val map = sortedMapOf("Korea" to 1, "Japan" to 2)

// use
map.put("London", 3)
map.get("Korea")
map["Korea"]
map.containsKey("Japan")
map.toList()
map.toMap()
map.toMutableMap()
map.toSortedMap()
```
### Set
```kotlin
// new instance
val set = setOf("Korea", "Japan")
val set = mutableSetOf("Korea", "Japan")
val set = hashSetOf("Korea", "Japan")
val set = linkedSetOf("Korea", "Japan")
val set = sortedSetOf("Korea", "Japan")

// use
set.add("London")
set.remove("London")
set.contains("London")
set.size
set.toList()
set.toMutableList()
set.toSet()
set.toHashSet()
set.toMutableSet()
set.toSortedSet()
```
### Ranges
```kotlin
for (i in 1..4) print(i) // prints "1234"
for (i in 1..4 step 2) print(i) // prints "13"
for (i in 4 downTo 1) print(i) // prints "4321"
for (i in 4 downTo 1 step 2) print(i) // prints "42"
for (i in 1 until 10) println(i) // prints "123456789"
(1..12 step 2).last // 11
```
### Equality
- Referential equality (`===`, `!==`)
```kotlin
val a = Integer(10)
val b = a
a === b // true
a !== b // false

val a = Integer(10)
val b = Integer(10)
a === b // false
a !== b // true
```
- Structural equality (`==`, `!=`)
```kotlin
data class Person(val name: String, val age: Int)
val person = Person("Jae-ryong", 20)
val person2 = Person("Jae-ryong", 20)
person == person2 // true
person != person2 // false
```
- Arrays Equality (using infix funtions)
  - [contentEquals]()
```kotlin
val hobbies = arrayOf("Hiking", "Chess")
val hobbies2 = arrayOf("Hiking", "Chess")
assertTrue(hobbies contentEquals hobbies2) // passed

// 참고 - contentEquals는 미리 정의 된 infix함수이다.
public infix inline fun <T> kotlin.Array<out T>.contentEquals(other: kotlin.Array<out T>): kotlin.Boolean
```
### Lambdas
```kotlin
// example
fun <T, R> List<T>.map(transform: (T) -> R): List<R> {
    val result = arrayListOf<R>()
    for (item in this)
        result.add(transform(item))
    return result
}
```
- input 파라미터 네이밍은 자유
```kotlin
var ints = listOf(1,2,3,4,5)
val doubled = ints.map { value -> value * 2 }
```
- `it`을 사용하면 input 파라미터 생략가능
```kotlin
ints.map { it * 2 }
```
- 사용하지 않는 파라미터는 `_`로 선언 가능
```kotlin
var map = mapOf("Korea" to "Seoul", "Japan" to "Tokyo")
map.forEach { _, value -> println("$value!") }
```
