# protected 접근 지정
파생 클래스에게 접근을 허용한다.

---
# 상속과 생성자, 소멸자
+ 파생 클래스의 객체가 생성될 때, **기본 클래스 생성자, 파생 클래스 생성자 둘다 실행된다.**
+ 기본 클래스 생성자가 먼저 실행된다.
+ by compiler

## 소멸자의 실행 순서
+ 파생클래스 소멸자 먼저 실행 -> 기본 클래스 소멸자 실행
+ 생성자 실행 순서와 반대로 실행

## 파생클래스에서 기본 생성자 호출
원래는 파생클래스의 생성자와 함께 실행할 기본 생성자를 호출 하여야한다.

하지만, 기본 생성자를 지정하지 않는다면 -> 묵시적으로 기본클래스의 기본 생성자를 실행한다.

+ 만약, 기본클래스에 기본생성자가 정의되어있지 않으면, 파생클래스에서 매개변수를 이용한 파생생성자를 호출할 때, 오류가 남

### 명시적인 기본 클래스의 생성자 선택
```cpp
B(int x) : A(x+3) {
 ~~~
}
```
