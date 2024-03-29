# 복사 생성자

+ 깊은 복사
+ 얕은 복사

### 얕은 복사의 문제점

​	**공유**한다는 것에서, 문제가 발생한다. 되도록이면 얕은 복사가 일어나지 않도록 해야한다.

## 복사 생성 및 복사 생성자

​	**복사 생성**은 객체가 생성될 때 원본 객체를 복사하여 생성한다. 복사 생성 시에만 실행되는 **복사 생성자**가 있다.

```
class ClassName {
  ClassName(const ClassName &c); //복사 생성자
}
```

​	이렇게 선언한다.

​	복사 생성자의 **매개 변수는 오직 하나**이고, **자기 클래스에 대한 참조**로 선언된다. 그리고 **오직 한개**만 선언할 수 있다.

### 복사 생성자 실행

```cpp
Circle src(30); //보통 생성자 실행

Circle dest(src); //복사 생성자 실행해서 dest객체 생성
```

​	컴파일러는 dest객체가 생성될 때, 보통 생성자 대신, 복사 생성자 **Circle(Circle &c)**를 호출 하도록 컴파일한다. 

### 디폴트 복사 생성자

​	복사 생성자를 정의하지 않아도, 컴파일러는 **디폴트 복사 생성자**를 묵시적으로 삽입하고 이 생성자를 호출하도록 컴파일 한다. **디폴트 복사 생성자**는 **얕은 복사**를 실행하도록 만든다. 이것은 단순히 원본 객체의 모든 멤버를 일대일 대응시켜서 복사한다.

## 얕은 복사 생성자의 문제점

​	포인터 타입의 멤버 변수가 없을 땐, 얕은 복사를 해도 **공유의 문제**가 발생하지 않는다. 그러나, 만약 포인터 변수 멤버가 있다면 사본 객체에 복사된 포인터 멤버 변수또한 같은 메모리를 가리키게 되어 문제가 생긴다.

​	예를 들어, A라는 객체를 생성후, B라는 객체를 A를 이용해 복사 생성자를 사용해서 생성한다고 가정해보자, 일단 A가 가리키던 메모리를 B와 함께 가리키게 되고**(공유의 문제)**, 만약 A객체를 소멸자로 소멸시키고, B객체를 소멸자로 소멸시킬 때 문제가 발생한다!

​	**A객체가 가리키던 메모리는 이미 반환되었고, B가 소멸할 때 메모리 반환이 안된다! why? 이미 반환했잖아!**

### 사용자 복사 생성자 작성

```cpp
Person::Person(const Person &p) {
    this->id = p.id;
    this->name = new char[strlen(p.name) + 1]; //공간 할당
    strcpy(this->name, p.name); //할당한 공간에 이름 삽입(만약 이게 없다면?)
}
```

​	이런식으로 서로 각각 다른 메모리를 가리키도록 해야한다.

### 묵시적 복사 생성

​	왠만하면 깊은 복사 생성자를 구현해놓자. Why? 복사 생성자를 구현해놓지 않았다가, 개발자가 모르는 사이에 묵시적 복사 생성자가 실행되어 공유의 문제 가능성이 있다.

### 묵시적 복사 생성자가 실행되는 3가지 경우

+ 객체로 초기화하여 객체가 생성될 때

```cpp
Person son = father; // 이 문장은
Person son(father); //이것과 같다
```

+ '값에의한 호출'로 객체가 전달될 때

```cpp
void f(Person person) {
  ...
}
...
f(father); //이렇게 값에 의한 호출로 매개변수로 전달할 때
```

+ 함수가 객체를 리턴할 때

```cpp
Person g() {
  Person mother(2, "jane");
  ...
  return mother; //객체를 리턴할 때
}
```

​	함수가 객체를 리턴할 때, **return 문은 리턴 객체의 복사본을 생성하여 호출한 곳으로 전달.**

