# Kotlin-Study

#### Kotlin 컴파일 속도
- 대개 Java보다 조금 빠르다!
- 클린 빌드 시에는 Java보다 조금 느리지만, 일반적인 개발 시나리오의 증분 빌드 시 성능이 좋다.
- [Kotlin vs Java: Compilation speed](https://medium.com/keepsafe-engineering/kotlin-vs-java-compilation-speed-e6c174b39b5d)

#### 세미콜론(;) 안녕
- 더 이상 세미콜론은 필요가 없다.

#### 변수는 모두 var 아니면 val
- var : 값이 바뀌는 변수
- val : 값이 불변하는 상수 변수

#### 자동 형변환 (Smart Casts)
- is 또는 null 체크 후에는 자동 형변환이 된다.
```java
fun demo(x: Any) {
    if (x is String) {
        print(x.length) // x가 자동으로 String으로 형변환 된다.
    }
}
```
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
