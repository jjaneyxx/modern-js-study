# 22장 this

## this 키워드

**this는 객체 자신 또는 객체로 생성될 인스턴스를 가리키는 변수이다.**

Java와 같은 클래스 기반 언어에서 this는 클래스가 생성하는 인스턴스를 가리키지만, 
Javascript의 this는 함수 호출 방식에 따라 this에 바인딩될 값이 동적으로 결정된다.

## 함수 호출 방식과 this 바인딩

⚠️this 바인딩 시점(함수 호출 시) ≠ 상위 스코프 결정 시점(함수 평가 시, 렉시컬 스코프)

### 일반 함수 호출

**전역 객체가 this에 바인딩됨.**

```jsx
function func() {
  console.log(this);
  function inner() {
    console.log(this);
  }
  inner();
};

func(); // window가 두번 출력된다.
```

**발생하는 문제**: 메서드 내에서 정의한 일반 함수의 this에 전역 객체가 바인딩된다.

```jsx
var value = 1; // var 키워드로 선언해서, 전역 변수의 프로퍼티가 된다. window.value = 1

const obj = {
  value: 100,
  foo() {
  console.log(this.value); // 100
    
    // 메서드 내부 중첩 함수를 일반 함수로 정의하면 this에 전역 변수가 바인딩된다.
    function bar() {
      console.log(this.value);
    }
    
    bar(); // 1
    
    // 메서드 내부 콜백 함수를 일반 함수로 정의하면 역시 this에 전역 변수가 바인딩된다.
    setTimeout(function () {
      console.log(this.value); // 1
    }, 1000);
  }
};

obj.foo();
```

**해결법**: `Function.prototype.apply` `Function.prototype.call` `Function.prototype.bind`를 사용하여 `this`를 명시적으로 바인딩한다. (구식) 또는 **화살표 함수를 사용한다.**

**화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.**

**화살표 함수 내부에서 this를 참조하면, 상위 스코프의 this를 그대로 참조한다. (lexical this, 렉시컬 스코프와 같이 함수가 정의된 위치에 의해 this가 결정됨)**

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log(this.value)
    
    const bar = () => {
      console.log(this.value);
    }
    
    bar();
    
    setTimeout(() => {
      console.log(this.value); // 1
    }, 1000);
  }
};

obj.foo(); // 모두 100이 출력된다.
```

### 메서드 호출

**메서드를 호출한 객체가 this에 바인딩됨.**

```jsx
// 객체의 메서드 --- 1️⃣
const person = {
  name: "Lee",
  getName() {
    return this.name;
  }
};
console.log(person.getName()) // Lee

// 또 다른 객체의 메서드 --- 2️⃣
const anotherPerson = {
  name: "Kim",
  getName: 	getName() {
    return this.name;
  }
  };
anotherPerson.getName = person.getName;
console.log(anotherPerson.getName()); // Kim

// 일반 함수로 호출 --- 3️⃣
const getName = getName() {
    return this.name;
  }
console.log(getName()); // ''.  window.name과 같다.

```

**동작 원리**: 메서드는 객체의 프로퍼티가 함수를 참조하는 형태로 구현되어 있다. 즉, 메서드는 독립된 함수 객체이다. 이 때문에 하나의 메서드(함수 객체)를 서로 다른 객체가 가리킬 수 있고(2️⃣), 독립적으로 실행할 수도 있다(3️⃣). 객체의 메서드로 함수를 호출하면,  this는 호출한 객체에 바인딩된다.

### 생성자 함수 호출

**인스턴스가 this에 바인딩됨.**

```jsx
function Circle() {
  this.radius = radius;
  this.getDiameter = function () {
    return this.radius * 2;
  };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

## React에서 this

초창기부터 클래스형과 함수형 컴포넌트가 존재했지만, 함수형 컴포넌트는 상태 및 라이프사이클 관리가 불가능했다(props만 사용 가능). 이후, 16.8 버전부터 리액트 훅이 등장했고, 본격적으로 함수형 컴포넌트 사용이 시작되었다.

16.8.0 (Feb. 6, 2019) Hooks 등장 -  [react/CHANGELOG.md at main · facebook/react](https://github.com/facebook/react/blob/main/CHANGELOG.md#1680-february-6-2019)

### React.Component Class - [React.Component – React](https://legacy.reactjs.org/docs/react-component.html)

1. 라이프사이클 관리 메서드
    - Mounting
        - [**`constructor()`**](https://legacy.reactjs.org/docs/react-component.html#constructor)
        - [`static getDerivedStateFromProps()`](https://legacy.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
        - [**`render()`**](https://legacy.reactjs.org/docs/react-component.html#render)
        - [**`componentDidMount()`**](https://legacy.reactjs.org/docs/react-component.html#componentdidmount)
    - Updating
        - [`static getDerivedStateFromProps()`](https://legacy.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
        - [`shouldComponentUpdate()`](https://legacy.reactjs.org/docs/react-component.html#shouldcomponentupdate)
        - [**`render()`**](https://legacy.reactjs.org/docs/react-component.html#render)
        - [`getSnapshotBeforeUpdate()`](https://legacy.reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)
        - [**`componentDidUpdate()`**](https://legacy.reactjs.org/docs/react-component.html#componentdidupdate)
    - Unmounting
        - [**`componentWillUnmount()`**](https://legacy.reactjs.org/docs/react-component.html#componentwillunmount)
2. 속성
    - [`props`](https://legacy.reactjs.org/docs/react-component.html#props)
    - [`state`](https://legacy.reactjs.org/docs/react-component.html#state)
3. 기타
    - [`setState()`](https://legacy.reactjs.org/docs/react-component.html#setstate)