# Kotlin-Study

### Kotlin 컴파일 속도
- 대개 Java보다 조금 빠르다!
- 클린 빌드 시에는 Java보다 조금 느리지만, 일반적인 개발 시나리오의 증분 빌드 시 성능이 좋다.
- [Kotlin vs Java: Compilation speed](https://medium.com/keepsafe-engineering/kotlin-vs-java-compilation-speed-e6c174b39b5d)

### 세미콜론(;) 안녕
- 더 이상 세미콜론은 필요가 없다.
```java
fun main(args: Array<String>) {
    val age = 30
    val name: String = "Seo Jaeryong"
    println(name.length)
}
```

### 변수는 모두 val 아니면 var
- val : 값 변경 불가능 (read-only)
```java
fun main(args: Array<String>) {
    val age: Int = 30
    a = 20 // 실패
}
```
- var : 값 변경 가능 (read-write)
```java
fun main(args: Array<String>) {
    var age: Int = 30
    a = 20 // 성공
}
```

### 자동 형변환 (Smart Casts)
- is 체크 후 (Java의 instanceof)
```java
fun demo(x: Any) {
    if (x is String) {
        print(x.length) // x가 자동으로 String으로 형변환 된다.
    }
}
```
- null 체크 후
```java
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
```java
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}
```
- 간략하게 표현
```java
fun maxOf(a: Int, b: Int): Int = if (a > b) a else b
```
- 더 간략하게 표현 (리턴 타입을 생략해도 추론 가능)
```java
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```
### For-Loop
- List
```java
val items = listOf("apple", "banana", "kiwi")
for (index in items.indices) {
    println("item at $index is ${items[index]}")
}

item at 0 is apple
item at 1 is banana
item at 2 is kiwi
```
- Range (a...b)
```java
for (i in 0..10) { 
    print(i)
}

012345678910
```
