---
pin: true
layout: post
title: "프로토타입/동적/정적"
categories:
  - TIL
tags:
  - 패스파인더
author: insidepixce
show_excerpts: true
date: 2024-01-17 15:30:00 +0900
disqus: true
paginate: true
disqus-count-scr: true
comments: true
toc: true
---

프로토타입/동적/정적, 그리고 프로그램은 어떻게 실행되는지 얕지만 깊게 파헤쳐보았다. 


# 20230117

# 🔥프로토타입

### 📖프로토타입이란

**from book** 

- 어떤 객체의 상위객체의 역할을 하는 객체
- 하위(자식) 객체에게 자신의 프로퍼티와 메서드를 상속하며,
- 프로토타입 객체의 프로퍼티나 메서드를 상속받은 하위 객체는 자신의 프로퍼티 또는 메서드인 것 처럼 자유롭게 사용할 수 있다 .

### 🌐프로토타입의 예시 코드를 둘러보자

- 전체 코드

```jsx
function Shape(color) {
  this.color = color;
}
Shape.prototype.getColor = function() {
  console.log("Color: " + this.color);
};
function Circle(color, radius) {
  Shape.call(this, color);
  this.radius = radius;
}
Circle.prototype = Object.create(Shape.prototype);
Circle.prototype.constructor = Circle;
Circle.prototype.getArea = function() {
  console.log("Area: " + Math.PI * this.radius ** 2);
};
var redCircle = new Circle("red", 5);
redCircle.getColor(); 
redCircle.getArea();  
```

1. 부모 객체 생성자 함수 

```jsx
function Shape(color) {
  this.color = color;
}
```

1. 부모 객체의 프로토타입에 메서드 추가 

```jsx
Shape.prototype.getColor = function() {
  console.log("Color: " + this.color);
};
```

1. 자식 객체 생성자 함수 

```jsx
function Circle(color, radius) {
  Shape.call(this, color);
  this.radius = radius;
}
```

1. 프로토타입 체인 설정

```jsx
Circle.prototype = Object.create(Shape.prototype);
Circle.prototype.constructor = Circle;
```

1. 자식 객체의 프로토입에 메서드 추가 

```jsx
Circle.prototype.getArea = function() {
  console.log("Area: " + Math.PI * this.radius ** 2);
};
```

1. 객체 생성 

```jsx
var redCircle = new Circle("red", 5);
```

1. 프로토타입 체인을 통한 상속을 확인하기 위해 호출해보기 

```jsx
redCircle.getColot();
redCircle.getArea();
```

---

`shape` 는 부모 객체의 생성자 함수이며, `circle`은 자식 객체의 생성자 함수이다 . 

`circle` 는 `shape` 를 상속받아 `getColor` 메서드를 사용하고 동시에 `getArea` 메서드를 추가로 정의한다. 

`redcircle` 객체는 `Circle` 의 인스턴스이면서 `Shape` 의 프로토타입을 따라간다 

이렇게 프로토타입 체인을 통해 상속된 메서드들을 호출할 수 있다 

---

<aside>
💡 자바스크립트는 프로토 타입 기반의 언어이지만 자바는 클래스 기반의 상속을 사용한다. 클래스로부터 객체를 생성하고 상속을 통해 클래스의 속성과 메서드를 자식 클래스에서 사용한다.

</aside>

<aside>
💡 자바에서 클래스의 상속은 `extends` 키워들르 사용하여 이루어진다. 자식 클래스에서 추가적인 속성이나 메서드를 정의할 수 있따.

</aside>

## 🚀클래스 기반 vs 프로토타입 객체 기반

### 📢1.선언 방식

프로토타입 : 생성자 함수와 프로토타입 객체를 사용하여 상속을 선언함

클래스:  클래스를 정의하고 `extends` 키워드를 사용하여 상속을 선언합니다 

### ⚒️2. 인스턴스 생성

프로토타입 : `Object.create()`나 생성자 함수와 `new` 키워드를 사용하여 인스턴스를 생성함

클래스 : 클래스 기반 언어에서는 클래스의 생성자를 호출하여 객체를 생성한다 

### 😳3. 동적성

프로토타입 : 프로토타입 상속은 동적 (런타임에 프로토타입 객체를 변경할 수 있다) 

클래스 : 클래스 기반 언어에서는 정적 (클래스 정의 이후에는 변경 어려움) 

## ⁉️문제 만들기

### 🖥️1. 이 코드의 실행결과는 무엇인가 ?

```jsx
function Animal(name) {
    this.name = name;
  }
  
  Animal.prototype.eat = function() {
    console.log(this.name + "가 먹고있다");
  };
  
  function Dog(name, breed) {
    Animal.call(this, name);
    this.breed = breed;
  }
  
  Dog.prototype = Object.create(Animal.prototype);
  Dog.prototype.constructor = Dog;
  Dog.prototype.bark = function() {
    console.log(this.name + "가 짖고있다");
  };
  
  var myDog = new Dog("콜라", "푸들");
  
  myDog.eat();
  myDog.bark();
```

**풀이 전 보고 가자**

---

프로토타입 체인은 js에서 객체 간 상속을 구현하는 매커니즘이다. 각 개체는 자체 프로퍼티와 메서드를 가지고 있다. 

객체의 `prototype` 프로퍼티는 해당 객체가 상속받은 프로토타입 객체를 가리키며, 이 프로토타입 객체 또한 자신의 `prototype` 를 가리키는 식으로 연결된다. 

이런 방식으로 객체는 지속적으로 프로토타입을 따라가며 속성과 메서드를 찾아나간다 

---

**풀이**

---

`mydog.eat()` 은 `myDog` 가 `Dog` 의 인스턴스이기 때문에 `Dog` 의 프로토타입 체인을 따라가면서 `Animal` 의 `eat`  메서드를 호출한다

출력 :“콜라가 먹고있다”

`myDog.bark();` 은 `Dog` 의 고유 메서드인 `bark` 를 호출한다

출력 : “콜라가 짖고있다”

---

### 🖥️2. 이 조건에 맞춰 코드 써봐.

1. 조건
- 두 개의 생성자 함수가 주어집니다  (person, student)
- person 생성자 함수는 name이라는 속성을 가지고 있습니다
- student 생성자 함수는 school이라는 속성을 가지고 있습니다
- student 객체는 person 객체를 상속받아야 함
1. 요구사항
- person 생성자 함수를 직접 수정하지 않고 student 생성자 함수에서 pesron 객체를 상속받아야 함
- student 생성자 함수로 생성된 객체는 name 과 school 속성을 가질 수 있어야 함
1. 예상되는 출력값
- student 객체를 생성하여 name과 school 속성을 출력하면 각각의 값이 나와야 함

# 🚀생성자 함수

### ⭐️ 객체 초기화

- `this` 키워드를 사용하여 새로운 객체를 초기화함
- 생성자 함수를 호출하면  객체가 생성되고 이 객체는 생성자 함수 내부의 코드에 따라 속성/메서드로 초기화됨

```jsx
function Person(name, age) {
	this.name = name ;
	this.age = age ;
}
var person1 = new Person("soojin", 21);

```

---

### ⭐️ `new` 키워드 사용

- 생성자 함수를 호출할 때 `new` 키워드를 사용하여 새로운 인스턴스를 생성함
- `new` 를 사용하면 새로운 객체가 생성되고 생성자 함수 내의 코드가 실행됨

```jsx
var person1 = new Person("soojin", 21);
```

---

### ⭐️ 프로토타입 활용

- 생성자 함수는 일반적으로 프로토타입을 사용하여 모든 인스턴스가 공유할 수 있는 메서드나 속성을 정의한다
- 이는 메모리를 효율적으로 사용하고 중복 코드를 방지한다

```jsx
Person.prototype.sayHello = function() {
	console.log("안녕하세요 제 이름은" + this.name);
};

var person1 = new Person("Soojin", 21);
person1.sayHello(); 
```

---

### ⭐️ `this` 키워드

- 생성자 함수 내에서 `this` 는 새로 생성되는 객체를 가리킨다
- 생성자 함수를 통해 속성이나 메서드를 할당할 때 `this` 를 사용하여 현재 생성되고 있는 객체에 대한 참조를 얻을 수 있다

```jsx
function Car (make, model) {
	this.make = make; 
	this.model = model; 
	this.start = function() {
		console.log("엔진이 시작됨" + this.make + "  " + this.model)'
	};
}
var myCar = new Car ("toyota", "Carmy:");
myCar.start();
```

# 🚀동적과 정적에 관하여

## 🏋️‍♂️ 1. 동적

- 프로그램 실행 중에 변수의 타입이나 속성이 결정됨
- 코드 실행 시간에 유연성을 제공하며, 타입이나 속성을 변경할 수 있음
- js, python 등은 동적 언어
- 웹개발, 스크립팅 언어 등에서 자주 쓰임
- 빠른 프로토타이핑과 개발 생산성이 요구되는 환경에서 유용

→ 변수의 타입이 런타임에 정해짐 

```jsx
let dynamicVariable;  // 변수 선언 (타입은 현재 undefined)

dynamicVariable = 42;  // 숫자 타입으로 할당
console.log(typeof dynamicVariable);  // 출력: number

dynamicVariable = "Hello, world!";  // 문자열 타입으로 할당
console.log(typeof dynamicVariable);  // 출력: string

dynamicVariable = true;  // 불리언 타입으로 할당
console.log(typeof dynamicVariable);  // 출력: boolean
```

→ 동일한 변수가 다양한 타입을 가질 수 있음

```jsx
let dynamicVariable = 42; // 숫자 타입
console.log(dynamicVariable); // 42

dynamicVariable = "Hello, world!"; // 문자열 타입
console.log(dynamicVariable); // Hello, world!
```

## 💤 2. 정적

- 컴파일 시간에 변수의 타입이나 속성이 결정됨
- 컴파일러는 타입 체크를 수행하고 오류를 런타임 이전에 찾을 수 있음
- java, c++ , c# 등은 정적 언어
- 대규모 시스템 및 안정성이 중요한 프로젝트에서 사용됨
- 컴파일러에 의한 타입 체크로 런타임 오류 방지하고 성능 최적화

→ 변수의 타입은 선언 시점에 명시됨 , 해당 타입에 맞는 값만 할당할 수 있음

```jsx
public class StaticExample {
    public static void main(String[] args) {
        int staticVariable = 42; // 정수 타입
        System.out.println(staticVariable);

        // 아래 주석을 해제하면 컴파일 오류 발생
        // staticVariable = "Hello, world!"; // 문자열 할당은 허용되지 않음
    }
}
```

위의 코드에서 `staticVariable` 은 정수 타입으로 선언됨 → 정수 값인 42를 할당할 수 있음 

but 나중에 문자열을 할당하려고 시도하면 컴파일 오류 발생 

`anotherVariable`을 선언할때는 명시적으로  `double` 타입을 지정하여 실수 값 할당

## 🤺3. 정적 페이지

### ⭐️정적 페이지의 특징

- 고정된 컨텐츠를 제공하는 웹 페이지
- 사용자의 요청에 따라 콘텐츠가 생성되거나 변경되지 않고 고정된 상태를 유지함
- 주로 HTML CSS 이미지 등의 정적 리소스로 이루어져 있음

### 🔥정적 페이지의 예시

- 블로그의 고정된 페이지
- 회사 소개 페이지

→ 서버에 미리 저장되어 있으며, 클라이언트 요청 시에는 서버에서 그대로 전달됨

## 😪4. 동적 페이지

### ⭐️ 동적 페이지의 특징

- 사용자의 요청에 따라 콘텐츠가 동적으로 생성되거나 변경되는 웹 페이지
- 서버 측 프로그램이나 스크립트를 사용하여 사용자에게 맞춤형 컨텐츠를 동적으로 생성
- 주로 데이터베이스와의 상호작용이나 사용자 입력에 따라 동적으로 변하는 콘텐츠 제공

### 🔥동적 페이지의 예시

- 소셜 미디어 피드, 온라인 쇼핑몰의 상품 목록 등이 동적 페이지의 예시
- 사용자의 요청에 따라 서버에서 데이터를 가공하여 동적으로 생성되므로 항상 최신 정보 제공
- 주로 백엔드 프로그래밍 언어(php python node java)와 데이터베이스를 사용하여 콘텐츠를 동적으로 생성하며, 서버에서 실행되고 클라이언트에 전달함

> 정적 페이지는 서버에 미리 생성되어 있어 서버 부하가 낮고 로딩 속도가 빠르나 동적 페이지는 서버에서 동적으로 생성되기 때문에 서버 부하가 있을 수 있고 로딩속도가 상대적으로 느림
> 

---

# 🚀프로그램이 어떻게 실행되냐고?

## 💬1. 선언

### ⭐️자바스크립트

- **`var`**, **`let`**, **`const`** 등의 키워드를 사용하여 변수를 선언함
- 함수 선언, 클래스 선언도 포함
    
    ```jsx
    javascriptCopy code
    let x;
    function myFunction() {
      // 함수 내용
    }
    ```
    

### ⭐️자바

- 클래스, 변수, 메서드 등을 선언
- 자료형이 명시적으로 필요한 경우가 많음
    
    ```java
    javaCopy code
    public class MyClass {
        int x;
        void myMethod() {
            // 메서드 내용
        }
    }
    ```
    

### ⭐️파이썬

- 변수를 선언하거나 함수를 정의함
- 자료형이 명시적으로 필요하지 않다
    
    ```python
    pythonCopy code
    x = 10
    def my_function():
        # 함수 내용
    ```
    

## 👩🏼‍💻2. 컴파일

---

### 🖥️인터프리터

1. **동적 타입 언어**
    
    주로 동적 타입 언어(예: 파이썬, 자바스크립트)에서 사용됩니다. 동적 타입 언어는 실행 시간에 타입을 확인하고 필요한 경우 동적으로 타입을 변환합니다.
    
2. **코드 실행 중 에러 확인**
    
    소스 코드를 실행 중에 한 줄씩 읽고 실행하므로, 코드 실행 중 에러가 발생하면 해당 부분이 바로 확인됩니다.
    
3. **간단한 디버깅**
    
    에러가 발생하는 코드를 발견하면 즉시 중단하고 해당 부분을 수정할 수 있습니다.
    
4. **코드 실행 속도가 느릴 수 있음**
    
    컴파일러에 비해 일반적으로 실행 속도가 느릴 수 있습니다. 매번 코드를 해석하고 실행하기 때문입니다.
    

**인터프리터의 동작 과정:**

1. **토큰화 및 구문 분석:** 소스 코드를 토큰으로 분해하고 구문 분석하여 AST 생성.
2. **실행 및 해석:** AST를 읽고, 각 문장을 순차적으로 실행하여 결과를 출력.

### 🖥️컴파일러

1. **정적 타입 언어**
    
    주로 정적 타입 언어(예: C, C++, Java)에서 사용됨 정적 타입 언어는 변수의 자료형을 선언하고, 컴파일 시간에 타입을 확인
    
2. **컴파일 타임 오류**
    
    컴파일러는 소스 코드를 분석하고, 타입 체크 등을 수행함→  컴파일 타임에 오류를 검출할 수 있다
    
3. **기계어 또는 중간 코드 생성**
    
    소스 코드를 목적 언어(기계어) 또는 중간 코드로 변환
    
4. **전체 소스 코드 컴파일**
    
    소스 코드의 전체를 컴파일하여 실행 파일을 생성
    

**컴파일러의 동작 과정:**

1. **토큰화 및 구문 분석:** 소스 코드를 토큰으로 분해하고 구문 분석하여 AST 생성.
2. **의미 분석:** 변수, 함수 호출 등의 의미를 확인.
3. **코드 생성:** 목적 언어(기계어)로 코드 생성.
4. **최적화:** 생성된 코드를 최적화.
5. **링킹:** 필요한 라이브러리와 연결하여 실행 파일 생성.

<aside>
💡 컴파일러는 소스 코드를 기계어로 변환하고, 변환된 코드를 실행 파일로 저장한다. 모든 소스코드를 미리 컴파일해야 함으로 실행 전에 시간이 소요된다. 실행 파일은 플랫폼에 종속적이다

</aside>

<aside>
💡 인터프리터는 소스 코드를 직접 실행하고 중간 단계의 실행 파일이 생성되지 않는다. 실행 시간에 코드를 해석하므로 빠르게 결과를 얻을 수 있다. 일반적으로 플랫폼 독립적이지만, 플랫폼에 따라 구현 방식이 좀 다르다

</aside>

추후 다뤄봐야 할 주제 : 플랫폼 종속적, 독립적

---

### ⭐️자바스크립트에서의 컴파일

- 일반적으로 브라우저에서 직접 실행되므로 명시적인 컴파일 단계가 없음
- 대신 인터프리터에 의해 해석됨 → 컴파일 안 함

### ⭐️자바에서의 컴파일

- 자바 코드는 컴파일러에 의해 바이트코드로 변환
- 바이트코드는 Java 가상 머신(JVM)에서 실행

### ⭐️파이썬에서의 컴파일

- 파이썬 코드는 바이트코드로 변환되고, 이는 파이썬 인터프리터에 의해 실행됩니다.
- 파이썬은 컴파일과 실행이 결합된 형태를 가지고 있습니다.

---

## 🏃🏻‍♀️3. 런타임

### ⭐️자바스크립트

- 브라우저나 Node.js🖥️같은 환경에서 코드가 실행

### ⭐️자바

- 자바 가상 머신 (JVM)에서 바이트코드가 실행

### ⭐️파이썬

- 파이썬 인터프리터에서 코드가 실행

---

# 🚀그래서 어떻게 실행되는지 하나씩 정리해줄게 !

### ⭐️**자바스크립트 (동적 타입 언어):**

1. **선언 (Declaration)**
    - 변수나 함수 등을 선언
2. **런타임 (Runtime)**
    - 코드가 실행되면서 변수의 타입이 동적으로 결정
    - 변수에 값을 할당하거나 함수를 호출하는 등의 동작이 런타임에 이루어짐

### ⭐️**자바 (정적 타입 언어)**

1. **선언 (Declaration)**
    - 클래스, 변수, 메서드 등을 선언하고, 변수의 타입이 명시
2. **컴파일 (Compilation)**
    - 소스 코드가 바이트코드로 변환되어 컴파일
3. **런타임 (Runtime)**
    - JVM(Java Virtual Machine)에서 바이트코드가 실행

### ⭐️**파이썬 (동적 타입 언어)**

1. **선언 (Declaration)**
    - 변수나 함수 등을 선언. 변수에 대한 타입 선언은 필요하지 않음.
2. **런타임 (Runtime)**
    - 코드가 실행되면서 변수의 타입이 동적으로 결정.
    - 변수에 값을 할당하거나 함수를 호출하는 등의 동작이 런타임에 이루어짐.



# 오늘을 마무리하며 
cs지식을 더 많이 쌓아야겠다.
기초부터 천천히 올려야 뭔가 된다 