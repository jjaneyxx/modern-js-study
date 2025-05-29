# 13 - Scope

Scope(유효범위)는 모든 프로그래밍 언어의 기본적인 개념이며, JS에서는 다른 언어의 Scope와 구별되는 특징이 있다.

식별자(변수 이름, 함수 이름, 클래스 이름 등)의 유효 범위를 의미한다.
코드에서 특정 변수나 함수에 접근할 수 있는지를 결정 하는데, JS에서는 주로 전역스코프(Global Scope)와 지역 스코프(Local Scope) 그리고 ES부터 도입된 블럭스코프(Block Scope)로 구분 된다.

## 전역 스코프: 프로그램 전체에서 유효한 범위.

## 지역 스코프: 특정 블록이나 함수 내에서만 유효.

## 블록 스코프: {} 중괄호 안에서만 유효.

### 전역 스코프

프로그램 어디서나 접근 가능한 변수 범위이며
예를 들어, 전역 변수 GlobalVar는 모든 함수에서 접근이 가능함
하지만 남용하면 위험할수있음 이름이 중복되면 다른 코드와 충돌하거나, 값 변경이 의도치 않게 일어날수 있음.
팀 프로젝트나 라이브러리 사용때 특히 조심
var counter = 0;

function increment() {
counter++;
}

function resetCounter() {
counter = 0; // 다른 코드에 영향을 줄 수 있음
}

increment();
console.log(counter); // 1
resetCounter();
console.log(counter); // 0

### 지역 스코프

특정 함수나 블록 내부에서만 유효한 변수 범위이며
외부에서 변수에 접근하지 못하기 때문에 충돌 위험이 줄어든다.
예를 들어, localScope 함수 안의 localVar는 함수 외부에선 참조가 불가능함
코드의 안정성을 높이고 독립성을 보장해줌

function calculateSum(a, b) {
let sum = a + b; // 지역 변수
return sum;
}

console.log(calculateSum(3, 4)); // 7
// console.log(sum); // ReferenceError: sum is not defined

### 블록 스코프

ES6 이후 도입된 let,const 키워드를 통해 구현하는 {} 내부에서만 유효한 변수를 만들수 있어
보다 세밀한 변수 범위의 조정이 가능함
예를 들면 조건문이나 반복문 내부에서만 변수를 사용 할 때 더 유용함
{
let blockScoped = "Inside block";
console.log(blockScoped); // "Inside block"
}
// console.log(blockScoped); // ReferenceError

### 스코프 체인

변수를 탐색하는 과정을 의미하는데
변수를 찾을 때, 현재 스코프에서 먼저 찾고 변수가 없다면 상위 스코프로 올라가서 이 과정을 최상위 스코프, 즉 전역 스코프 까지 반복한다
책의 예제에서 inervar,outervar, 그리고 globalvar를 출력하는 순서를 보면 탐색 과정을 한눈에 볼수 있다

### 함수 스코프

var로 선언된 변수에만 적용되는 개념이며, 예제에서 보다시피 블록 레벨 스코프를 인정 하지 않아서 의도치 않게 변수의 값이 재할당되는 결과가 발생한다

function Ex() {
if (true) {
var x = 10; // if 블록 내부에서 선언
}
console.log(x); // 10 (블록 외부에서도 접근 가능)
}

### 렉시컬 스코프

JS는 렉시컬 스코프(정적 스코프)를 따르는데
함수의 선언 위치에 따라서 스코프가 결정되고
실행 시점이 아닌 작성 시점 기준으로 동작한다

### 호이스팅

var는 선언만 끌어올려지므로 초기화 전에 접근 시 undefined를 반환한다.
let과 const는 초기화 전에 접근하면 ReferenceError를 발생시킨다.

console.log(hoistedVar); // undefined
var hoistedVar = "hoisted";

console.log(notHoistedVar); // ReferenceError
let notHoistedVar = "not hoisted";
