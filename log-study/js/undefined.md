# 스코프, 클로저, 실행 컨텍스트

1. 프로그래밍은 변수, 함수에 이름을 부여해 의미를 갖게함. \(이름과 값의 대응표 만들어 사용\)
2. 변수, 함수를 프로그램 전체에서 한번에 관리해 충돌
3. 충돌을 피하기위해 **스코프**라는 규칙을 만듬

## **ES6 스코프 규칙**

함수 레벨, 블록 레벨의 렉시컬 스코프 \(ES6 이전엔 블록 레벨 스코프를 지원안함\)

### 함수 레벨 스코프\(function-level scope\)

```javascript
function x(){
    // 함수 레벨 스코프
    var functionScope = 'ok';
    if(){
        var functionScope_2 = 'ok';
        console.log(functionScope)
        console.log(functionScope_2)
    }
    console.log('함수 내부' + functionScope);
    console.log('함수 내부' + functionScope_2);
}
x()
console.log('함수 외부' + functionScope); // is not defined
console.log('함수 외부' + functionScope_2); // is not defined
```

* 함수 선언식
* var로 선언된 변수

### 블록 레벨 스코프\(block-level scope\)

```javascript
function x(){
    // 함수 레벨 스코프
    let blockScope = 'ok';
    if(){
        // 블록 레벨 스코프
        let blockScope_2 = 'ok';
        console.log(blockScope)
        console.log(blockScope_2)
    }
    console.log('함수 내부' + blockScope);
    console.log('함수 내부' + blockScope_2); // is not defined
}
x()
console.log('함수 외부' + blockScope); // is not defined
console.log('함수 외부' + blockScope_2); // is not defined
```

* const, let으로 선언된 변수

## **전역변수, 지역변수와 스코프**

### **전역변수**

```javascript
var x = 'gloval';
function ex(){
    // 함수 레벨 스코프
    var x = 'local';
    x = 'change';
    console.log(x); // 지역변수 x 출력 'change'
}
ex();
console.log(x); // 전역변수 x 출력 'gloval'
```

### **지역변수**

```javascript
var x = 'gloval';
function ex(){
    // 스코프 미생성
    x = 'change'; // 전역변수 x 에 접근
    console.log(x); // 전역변수 x 출력 'change'
}
ex();
console.log(x); // 전역변수 x 출력 'change'
```

## **스코프 체인**

내부 함수 &gt; 외부 함수 접근 가능하게 함 = 범위를 넓혀가며 값을 찾는 관계

```javascript
var name = '철수';
function outer(){
    console.log('외부 함수 내부' + name); // outer not defined > window name ="철수";
    function inner(){
        var name = "영희";
        var nickname = '호빵맨';
        console.log('내부 함수 내부' + name); // inner, outer not defined > window name ="철수";
    }
    inner();
}
outer();
console.log(nickname); // is not defined : inner 함수의 지역변수 접근 불가
```

## 동적 스코프와 렉시컬 스코프

### 동적 스코프\(Dynamic scope\)

* 함수를 어디서 호출하였는지에 따라 상위 스코프를 결정

### 렉시컬 스코프\(Lexical scope, Static scope\)

* 함수를 어디서 선언하였는지에 따라 상위 스코프를 결정
* 대부분의 프로그래밍 언어는 렉시컬 스코프를 따름

```javascript
// type1)
// ex1의 상위 스코프는 전역입니다.
var x = '전역';
function ex1(){
    console.log(x);
}
function ex2(){
    var x = '지역';
    console.log(x);
}
ex1();
ex2();

//type2)
// ex1의 상위 스코프는 ex2입니다.
var x = '전역';
function ex1(){
    console.log(x); // '지역'
}
function ex2(){
    var x = '지역';
    console.log(x); // '지역'
    ex1();
}
ex2();
```

## 전역변수의 충돌을 피하는 방법

### **네임 스페이스 패턴 \(namespace pattern\)**

#### ✔️ 공개

```javascript
var yj = {
    x : 'local',
    y : function(){
        console.log(this.x);
    }
}


yj.x = 'gloval'; // 사용자가 접근하여 변경 가능
yj.y() // y : function(){console.log(this.x)} 실행 > 'gloval'
```

#### ✔️ 비공개

```javascript
var yj = function(){
    var x = 'local';
    function y(){
        console.log(x);
    }
    return { y : y } // y 제외 나머지 비공개
}

var func = yj();
func.x = 'gloval'; // 사용자 변경 불가
func.y() // y : function(){console.log(this.x)} 실행 > 'local'
```

## **IIFE 기법**

```javascript
( function () {
    // code
} ) ();
```

* 즉시 호출 함수 표현식, 모듈 패턴의 기반
* 선언과 동시에 실행되므로 플러그인, 라이브러리 만들때 많이 사용.
* 비공개 변수가 없는 자바스크립트에 비공개 변수 기능을 만들어준다.
* **메모리 낭비** &gt; 한번 실행되고 **재사용되지 않으나 메모리를 차지**하고있음.

