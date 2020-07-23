# 스코프(Scope)

## 정의

스코프란 **JavaScript에서 변수, 객체, 함수가 접근할 수 있는 유효 범위이며 현재 실행되는 실행 문맥의 어휘적 환경(Lexical Environment)** 이다.

<br>

## 전역 스코프(Global Scope)
**함수의 안이든 밖이든, 어느 위치에서나 접근할 수 있는 스코프**이다.
ES5 문법의 유일한 변수 선언 키워드인 var는 함수 스코프를 가지며 함수 외부에 선언되면 전역 스코프, 내부에 선언되면 지역 스코프를 가진다.

<br>

```javascript
var global = 1;

function print () {
  console.log(global);
}

console.log(global); // 1
print(); // 1
```
print함수는 전역 스코프를 가지며 이 함수 내부에서 전역 스코프에 있는 global 변수에 접근이 가능하다.
이 과정에서 스코프 체인(중첩된 어휘 구조의 상위 탐색)이 이루어진다.

<br>

```javascript
for(var globalNumber=0; globalNumber < 1; globalNumber++) {
  var forValue = 1;
  console.log(forValue); // 1
}
console.log(globalNumber, forValue); // 1 1
```
for문은 함수가 아니기 때문에 내부에서 위 예시에서 선언된 변수들은 전부 전역 스코프이다.

<br>

## 암묵적 전역 스코프

만약 **선언 키워드가 없이 변수를 초기화하면 암묵적으로 전역 스코프**가 된다.

```javascript
function createGlobalVariable () {
  global = 1;
}
createGlobalVariable();

console.log(global); // 1
```

<br>

## 지역 스코프 (함수)
**함수 내부의 스코프**를 말하며 이 스코프에 선언된 변수를 지역 변수라고 한다.
```javascript
var value = 'global';

function print () {
  var value = 'local';
  console.log(value);
}
print(); // local
console.log(value); // global
```
print함수 내부에 선언된 value는 함수 스코프를 가지며 전역 스코프의 value변수와는 다른 값을 가지고 있다.

<br>

```javascript
function print () {
  var value = 'local';
  console.log(value);
}
print (); // local
console.log(value); // error: Uncaught ReferenceError: value is not defined
```
함수 외부에서 지역 스코프에 있는 변수에 접근하려 하면 에러가 발생한다.

<br>

물론 변수뿐만 아니라 함수 내부에 함수를 선언했을 때도 동일하다.
```javascript
function outerFunction () {
  console.log('This Function is hoisted!');


  function innerFunction () {
    console.log('innerFunction');
  }
  innerFunction(); // innerFunction
};

outerFunction(); // This Function is hoisted!
innerFunction(); // Uncaught ReferenceError: innerFunction is not defined
```
outerFunction 내부에 선언된 innerFunction은 함수 스코프를 가지기 때문에 외부에서 호출하려 하면 에러가 발생한다.

<br>

## 지역 스코프 (블록) (=중괄호, {})
**ES6 문법이 도입되면서 새로 추가된 키워드**인 **const**와 **let**이 있다.

**이 키워드들은 함수 스코프가 아닌 블록 스코프**를 가진다. 위 키워드를 가진 변수가 코드 블록(함수, if문, for문, while문, try/catch문 등) 내에 선언되면 해당 블록 외부에서는 접근할 수 없다. 물론 블록 외부에서 선언될 경우 전역 스코프를 가진다.

<br>

```javascript
{
  var varVariable = 1;
  console.log(varVariable); // 1
}

console.log(varVariable); // 1
```

```javascript
function print () {
  {
    var varVariable = 1;
    console.log(varVariable); // 1
  }
  console.log(varVariable); // 1
}
print();

console.log(varVariable); // Uncaught ReferenceError: varVariable is not defined
```
위 두 개의 예시를 보면 var 키워드로 선언된 변수는 함수 스코프를 가지기 때문에 코드 블록과 상관없는 모습을 볼 수 있다.

<br>

const, let의 예시를 보자. const는 Read Only(재할당이 불가능함)을 나타내는 키워드이며 let은 변수를 나타내는 키워드이다.
```javascript
{
  const constVariable = 1;
  let letVariable = 1;
  console.log(constVariable, letVariable); // 1 1
}
console.log(constVariable, letVariable); // Uncaught ReferenceError: varVariable is not defined
```
const, let키워드로 선언된 변수는 블록 스코프를 가지기 때문에 블록 외부에서 접근할 수 없다.

<br>

## 렉시컬 스코프(Lexical Scope)

**프로그래머가 코드를 짤 때, 변수 및 함수/블록 스코프를 어디에 작성하였는가에 따라 정해지는 스코프** 를 렉시컬 스코프라고 한다. "렉시컬(Lexical)" 이라는 명칭이 붙은 이유는 자바스크립트 컴파일러가 소스코드를 토큰(Token)으로 쪼개서 의미를 부여하는 렉싱(Lexing) 단계에 해당 스코프가 확정되기 때문이다. 다시 쉽게 말하면, 변수 혹은 함수/블록이 어디에 써있는가를 보고 그 스코프를 판단하면 된다.

<br>

## 스코프 체인(Scope Chain)

**현재 스코프에서 식별자를 검색할 때 상위 스코프를 연쇄적으로 찾아나가는 방식** 을 말한다. 실행 컨텍스트를 배웠다면 생성될 때마다 LexicalEnvironment가 만들어지고 그 안에 outer 참조 값이 있다는 것을 알 것이다. 바로 이 outer 참조 값이 상위 스코프의 LexicalEnvironment를 가리키기 때문에 이를 통해 체인처럼 연결되는 것이다.

즉, 다음과 같은 과정으로 스코프 체인을 검색한다.

1. 현재 실행 컨텍스트의 LexicalEnvironment의 EnvironmentRecord에서 식별자를 검색한다.
2. 없으면 outer 참조 값으로 스코프 체인을 타고 올라가 상위 스코프의 EnvironmentRecord에서 식별자를 검색한다.
3. 이를 outer 참조 값이 `null` 일 때까지 계속하고 찾지 못한다면 에러를 발생시킨다.

<br>

## 참조

* [You don't know JS, Scope and Closures](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/scope-closures/ch1.md)
* [PoiemaWeb, 스코프](https://poiemaweb.com/js-scope)