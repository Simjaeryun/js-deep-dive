
## 심벌이란?
ECMAScript로 표준화된 이래로 자바스크립트엔 6개의 타입이 있다 
1. 문자열
2. 숫자
3. 불리언
4. undefined
5. null
6. 객체 타입

심벌은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다. 
**심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다.** 따라서 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

프로퍼티 키로 사용할 수 있는 값은 빈 문자열을 포함한 모든 문자열 또는 심벌 값이다.

## 심벌 값의 생성.
### Symbol 함수
심벌 값은 Symbol 함수를 호출하여 생성한다. 다른 원시값의 값은 리터럴 표기법으로 값을 생성 할 수 있지만
심벌 값은 Symbol함수를 호출해서 생성해야한다. 이때 생성된 심벌 값은 외부로 노출되지 않아 확인 할 수 없ㅇ며, 다른 값과 절대 중복되지 않는 유일무이한 값이다.
```js
const mySymbol = Symbol();
console.log(typeof mySymbol) // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol) // Symbol()
```
언뜻 보면 생성자 함수로 객체를 생성하는 것처럼 보이지만 Symbol 함수는 생성자 함수와 달리 new 연산자와 함께 호출하지 않는다. new 연산자와 함께 생성자 함수 또는 클래스를 호출 하면 객체(인스턴스)가 생성되지만 심벌 값은 변경 불가능한 원시 값이다.

Symbol 함수에는 선택적으로 문자열을 인수로 전달할 수 있다. 이 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용되며, 심벌 값 생성에 어떠한 영향도 주지 않는다. 즉, 심벌 값에 대한 설명 이 같더라도 생성된 심벌 값은 유일무이한 값이다.
```js
const mySymbol1 = Symbol("hi"),
	  mySymbol2 = Symbol("hi");

mySymbole1 === mySymbol2  // false
```
심벌 값도 문자열,숫자 불리언 같이 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다. 다음 에제의 description 프로퍼티와 toString 메서드는 Symbole.prototype의 프로퍼티다.
```js
consle.log(mySymbol1.description) // hi
console.log(mySymbol1.toString()) // Symbol(hi)
```
심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다. 하지만 불리언 타입으로는 암묵적 타입 변환됨 이를 통해 if문 등에서 존재하는지 확인이 가능.
```js
const mySymbol = Symbol();

// 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.  
console.log(mySymbol + ''); 
// TypeError: Cannot convert a Symbol value to a string 
console.log(+mySymbol);
// TypeError: Cannot convert a Symbol value to a string


const mySymbol = Symbol();  
// 불리언 타입으로는 암묵적으로 타입 변환된다
console.log(!!mySymbol); // true

// if 문 등에서 존재 확인이 가능하다.  
if (mySymbol) console.log('mySymbol is not empty.');

```

### Symbol.for / Symbol.keyFor 메서드
#### symbol.for
메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다. 
1. 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환함.
2. 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장 후 생성된 심벌 값을 반환.

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성 
const s1 = Symbol.for('mySymbol');  
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환 
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2); // true
```

Symbol 함수는 호출될 때마다 유일무이한 심벌 값을 생성한다. 이때 자바스크립트 엔진이 관리하는 심벌 값 저장소인 전역 심벌 레지스트리에서 심벌 값을 검색할 수 있는 키를 지정할 수 없으므로 전역 심벌 레지스트리에 등록되어 관리되지 않는다. 하지만 Symbol.for 메서드를 사용하면 애플리케이션 전역에서 중복되지 않는유일무이한상수인 심벌값을 단 하나만 생성 하여 전역 심벌 레지스트리를 통해 공유 할 수 있다.

#### Symbol.keyFor
메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출 할 수 있다.
```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성 
const s1 = Symbol.for('mySymbol');  
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출  
Symbol.keyFor(s1); // mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');  
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출  
Symbol.keyFor(s2); // undefined
```

## 심벌과 상수
예를들어 4방향을 나타내는 상수를 정의한다고 생각해보자.
```js
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.  
// 이때 값 1, 2, 3, 4에는 특별한 의미가 없고 상수 이름에 의미가 있다. 
const Direction = {
	UP: 1, DOWN: 2, LEFT: 3, RIGHT: 4
};

// 변수에 상수를 할당  
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
	console.log('You are going UP.'); 
}
```
위 예제처럼 값에는 특별한 의미가 없고 이름 자체에 의미가 있는 경우가 있다. 이때 문제는 상수 값 1,2,3,4가 변경될 수 있으며, 다른 변수 값과 중복될 수도 있다는 것이다. 이러한 경우 변경/중복될 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심벌 값을 사용할 수 있다.

```js
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다. 
// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성한다. 
const Direction = {
	UP: Symbol('up'), 
	DOWN: Symbol('down'), 
	LEFT: Symbol('left'), 
	RIGHT: Symbol('right')
};

const myDirection = Direction.UP;
if (myDirection === Direction.UP) {
	console.log('You are going UP.'); 
}
```

#### enum
javascript에서 enum 사용하기. 
Enum은 enumerated type의 줄임말이다. 말그대로 열거하는 타입이라는 의미를 가진다.
무엇을 열거한다는 것일까?
상수 값 중 비슷한 종류들을 묶어두기 위한 용도로 자주 사용된다.
자바스크립트에서의 enum 타입은 고유한 타입이 아니다. 
하지만 이 자료형과 조금이나마 비슷하게 작동할 수 있는 데이터를 자바스크립트에서도 간단하게 만들 수 있다 아래에서 정리해 보겠다.

enum의 장점
1. 코드가 단순해지고 가독성이 좋다.
2. enum이란 이름 자체만으로 열거 의도를 분명히 알 수 있다.
3. 하나의 변수에 대해서 고정값으로 이용이 가능하다.
4. 안정적이며 읽기 쉽고 오류의 발생도 줄일 수 있다.
예시)
```js
if(season === "winter") console.log("겨울")
...

...
if(season === "Winter") console.log("겨울이 좋아")
```
다음과 같이 다른 위치에서, 동일한 문자열에 대해 작업 할 때 season값이 "Winter"일지 "winter"일지 혼동 되는것은 자주 일어나는 일입니다. 심지어, wimter처럼 오타를 발생시킬 가능성도 큽니다.
이 때, 다음과 같은 코드를 작성한다면 의도치 않은 실수가 발생할 걱정도 없을것입니다.
```js
const WINTER = "winter"

if(data === WINTER) console.log("겨울")
...

...
if(data === WINTER) console.log(:"겨울이 좋아.")
```

이렇게 상수값으로 사용할 문자열을 선언해두면 오류처리에 유리합니다.

위 예시와 같이 Season이라는 객체를 생성해 그 범주들을 상수로 묶고자 하면 enum을 사용 할 수 있을것입니다.
```js
const Season = {
	SPRING:"spring",
	SUMMER:"summer",
	WINTER:"winter",
}
```
하지만 문제가 있습니다. js에서 key:value 값을 가진 이 데이터 값은 변경될 수 있습니다.

-------------------------------여기까진 enum에 대해서 아래는 다시 책--------------------

자바스크립트에서 enum을 흉내 내어 사용하려면 다음과 같이 객체의 변경을 방지하기 위해 객체를 동결하는 Object.freeze 메서드2와 심벌 값을 사용한다.


```js
// JavaScript enum  
// Direction 객체는 불변 객체이며 프로퍼티 값은 유일무이한 값이다. 
const Direction = Object.freeze({
	UP: Symbol('up'),
	DOWN: Symbol('down'),
	LEFT: Symbol('left'), 
	RIGHT: Symbol('right')
});

const myDirection = Direction.UP;
if (myDirection === Direction.UP) {

console.log('You are going UP.'); }
```


## 심벌과 프로퍼티 키

객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있다고 했다. 동적으로도 생성이 가능하다. 심벌 값으로 프로퍼티 키를 동적 생성하여 프로퍼티를 만들어 보자. 심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다. 프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용 해야 한다.
```js
const obj = {
[Symbor.for("hi")]:1
};

obj[Symbol.for("hi")]; // 1
```
심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다. 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 미래에 추가될 어떤 프로퍼티 키와도 충돌할 위험이 없다.

## 심벌과 프로퍼티 은닉
심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for...in 문이나 Object.keys, Object. getOwnPropertyNames 메서드로 찾을 수 없다. 이처럼 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출 할 필요가 없는 프로퍼티를 은닉 할 수 있다.
**아니근데 이렇게 쓰면 나도 사용못하지않나?**
```js
const obj = {  
// 심벌 값으로 프로퍼티 키를 생성 
[Symbol('mySymbol')]: 1

};

for (const key in obj) {
console.log(key); // 아무것도 출력되지 않는다. 
}
console.log(Object.keys(obj)); // [] 
console.log(Object.getOwnPropertyNames(obj));// []
```
하지만 프로퍼티를 완전하게 숨길 수 있는 것은 아니다. ES6에서 도입된 Object.getOwnPropertySymbols 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.
```js
const obj = {  
// 심벌 값으로 프로퍼티 키를 생성
[Symbol('mySymbol')]: 1

};

// getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환한다. console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

// getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있다.  
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0]; console.log(obj[symbolKey1]); // 1
```
그렇다면 완벽 은닉은 아니구만.

## 심벌과 표준 빌트인 객체 확장
하지말자 표준 빌트인 객체는 그냥 읽기전용으로 사용하자.

## Well-known Symbol
자바스크립트가 기본 제공하는 빌트인 심벌값을 Well-known Symbols라고 부른다. 이는 자바스크립트 엔진의 내부 알고리즘에 사용된다.![[스크린샷 2023-11-21 오후 8.30.56.png]]
위 참조 결과에서 알 수 있듯이 Symbol 객체는 프로퍼티와 메소드를 가지고 있다. Symbol 객체의 프로퍼티 중에 length와 prototype을 제외한 프로퍼티를 `Well-Known Symbol`이라 부른다.

JavaScript의 Well-known Symbol은 언어 내장의 Symbol 값입니다. 이들은 전역 객체 `Symbol`의 속성으로, 개발자가 객체 동작을 정의하거나 수정하는 데 사용할 수 있습니다.

다음은 Well-known Symbol의 예입니다:

- `Symbol.iterator`: 객체가 어떻게 반복되어야 하는지를 정의합니다. 이 심볼을 키로 사용하는 메서드가 있으면, 그 객체는 반복 가능합니다.
- `Symbol.toStringTag`: `Object.prototype.toString` 메서드에 의해 사용되는 기본 설명을 정의합니다.
- `Symbol.species`: 생성자 함수가 파생 객체를 만드는 데 사용되어야 하는 함수를 정의합니다.

이 외에도 많은 Well-known Symbol이 있으며, 이들은 모두 특정 객체 동작을 정의하거나 수정하는 데 사용됩니다.