표준 빌트인 객체 Math는 수학적인 상수와 함수를 위한 프로퍼티와 메서드를 제공한다. Math는 생성자 함수가 아니다. 따라서 Math는 정적 프로퍼티와 정적 메서드만 제공한다.

## Math 프로퍼티
Math.PI : 원주율 PI 값을 반환한다.
```js 
Math.PI; // 3.1415...
```

## Math 메서드

### Math.abs 
: 절대값을 반환한다.
```js
Math.abs(-1); //1
Math.abs('-1');  //1
Math.abs(''); //0
Math.abs([]);  //0
Math.abs(null);  //0
Math.abs(undefined); //NAN 
Math.abs({}); //NaN
Math.abs('string'); //NaN 
Math.abs(); //NaN
```
### Math.round 
: 인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환한다.
```js
Math.round(1.4) //1
Math.round(1.6) //2
Math.round(-1.4) // -1
Math.round(-1.6) // -2
Math.round() //NaN
```
### Math.ceil 
: Math.ceil 메서드는 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환한다.
```js
Math.ceil(1.4) //2
Math.ceil(1.6) //2
Math.ceil(-1.4) //-1
Math.ceil(-1.6) //-1
```
### Math.floor
: Math.ceil과 반대되는 개념

### Math.sqrt 
인수로 전달된 숫자의 제곱근을 반환한다.
```js
Math.sqrt(9) //3
Math.sqrt(-9) // NaN
```

### Math.random 
임의의 난수(랜덤 숫자)를 반환한다. Math.random 메서드가 반환한 난수는 0에서 1 미만의 실수다. 즉, 0은 포함되지만 1은 포함되지 않는다.
```js
Math.random(); // 0에서 1 미만의 랜덤 실수(0.8208720231391746)

/*  
1에서 10 범위의 랜덤 정수 취득  
1) Math.random으로 0에서 1 미만의 랜덤 실수를 구한 다음, 10을 곱해 0에서 10 미만의 랜덤 실수를 구한다. 2) 0에서 10 미만의 랜덤 실수에 1을 더해 1에서 10 범위의 랜덤 실수를 구한다.  
3) Math.floor로 1에서 10 범위의 랜덤 실수의 소수점 이하를 떼어 버린 다음 정수를 반환한다.  
*/  
const random = Math.floor((Math.random() * 10) + 1);  
console.log(random); // 1에서 10 범위의 정수
```

### Math.pow 
: 첫번째 인수를 밑으로, 두 번째 인수를 지수로 거듭제곱한 결과를 반환한다.
```js
Math.pow(2,8); // 256
Math.pow(2,-1); // 0.5
Math.pow(2); //NaN
```
Math.pow 대체제로 ES7에서 도입된 지수 연산자를 사용하면 가독성이 더 좋다.
```js
// ES7 지수 연산자  
2 ** 2 ** 2; // 16 
Math.pow(Math.pow(2, 2), 2); // 16
```

### Math.max 
: 전달받은 인수 중 가장 큰 수를 반환한다.
```js
Math.max(1) //1
Math.max(1,2) // 2
Math.max(3,2,1) //3 
```
배열을 인수로 전달받아 배열의 요소중 최대값을 구하려면 스프레드 문법을 사용해야한다.
```js
Math.max(...[1,2,3]) //3
```

### Math.min 
: Math.max와 반대되는 개념 사용법은 동일함.







## 알아두면 쓸데가 있을만한 것 ?
#### Math.random()은 정말 랜덤일까?
자바스크립트는 어떻게 Math.random 함수에서 난수를 생성하는가?
JS는 사실 아무것도 하지 않는다. 브라우저에 따라 달라진다.
현재 대부분의 브라우저는 xorshift128+라는 알고리즘을 사용한다고한다.
xorshift128+에 의해 생성된 숫자는 난수를 생성하는 것이 아닌, 특정 값에 공식을 대입한 수학식이다.

사실 이런 특징으로 Math.random() 함수는 실제로 무작위 값을 반환하지 않는다.
특정 공식을 사용해 무작위로 보일 수 있지만, 결국 난수를 계속해서 생성하면 반복되어 어떠한 패턴을 나타낸다.

그렇기 때문에 Math.random()은 위험 합니다.
실제 crypto-js에서 난수 생성을 위해 Math.random() 함수를 사용할 때 시드가 동일한 취약점이 발견된 적이 있다.

Math.random 함수는 시드 값을 설정할 수 없다. 즉 내부 시드값과 Math.random 함수에서 사용하는 알고리즘이 밝혀진다면 매우 치명적일 것이다.

위 사건으로 인해 시간을 들인다면 내부 Math.random 함수의 시드 값을 확인하는 것이 가능할 것이라고 유추되어서 Math.random함수는 현재 crypto-js에서 사용되지 않는다.

그렇다면 위문제를 어떻게 해결이 가능한가.

[Browser]
window.crypto.getRandomValues
[Node.js]
require('crypto').randomBytes
#### Math.floor VS parseInt
positive decimal number를 내림할 때 Math.floor과 parseInt를 사용할 수 있는데 
둘중에 하나를 택한다면 Math.floor를 사용하자 근데 Math.floor보다 빠른게 
`~~라는 연산자를 사용하면 더 빠름` 

### tilde(~) 연산자

tilde 연산자는 비트연산자로 NOT의 기능을 한다고 생각하면 된다.  
아래와 같이 2진수일 때 0과 1만 뒤바꾸면 된다.

```javascript
const a = 5;     // 0000000000000101
console.log(~a); // 1111111111111010
// expected output: -6

const b = -3;    // 1111111111111101
console.log(~b); // 0000000000000010
// expected output: 2
```

하지만 저것을 콘솔에 실행시켜 보면 expected output마냥 -6,2만 나오게 된다. 이를 이진수로 직접 눈으로 확인 하는 법은 없을까?

##### toString를 이용해 간단하게 2진수로 변환하기

`toString`메소드를 활용하면 10진수를 간단하게 2진수로 변환할 수 있다.

```javascript
const a = 8
console.log(a.toString(2)) // 1000
```

하지만 이렇게 했을 때 한가지 문제점이 발생한다. 아래의 코드가 어떻게 실행되는지 예상해보자.

```javascript
const b = -8
console.log(b.toString(2))
```

1111111111111111111111111111000 이렇게 1000의 보수가 나와야 할것만 같지만 -1000으로 나와버린다. 이렇게되면 not연산자를 제대로 확인할 수 없다.

이를 해결하기 위해 비트 시프트 연산자 중 `>>>`를 활용할 수 있다.

#### `>>>` 연산자

`>>>` 연산자는 `>>` 연산자와 비슷하지만 둘의 차이는 값이 음수 일 때 차이가 있다.

mdn의 예시로 확인해보자.

- `>>` 연산자 예시

```javascript
-9 (10진수): 11111111111111111111111111110111 (2진수)
                   --------------------------------
-9 >> 2 (10진수): 11111111111111111111111111111101 (2진수) = -3 (10진수)
```

- `>>>` 연산자 예시

```javascript
-9 (10진수): 11111111111111111111111111110111 (2진수)
                    --------------------------------
-9 >>> 2 (10진수): 00111111111111111111111111111101 (2진수) = 1073741821 (10진수)
```

차이는 `>>`연산자는 1이라면 1로 채우면서 왼쪽으로 밀지만 `>>>`연산자는 0으로 채우면서 민다.

이 특징을 활용하여 음수 역시 2진수로 변환시킬 수 있다.

이제 다시 위로 돌아가자.

아래 코드와 같이 `b>>>0`를 활용해서 음수역시 2진수로 볼 수 있다.

```javascript
const b = -8
console.log((b>>>0).toString(2)) 
//11111111111111111111111111111000
```

이제 tilde(~)연산자가 어떻게 되는지 확인했다.

---

### double tilde(`~~`)

사실 별거 없다. tilde를 2번 반복해주는 것이다.

`let k = 8` 이라고 했을 때 `~k`는 무엇일까? `-9` 이다. (why? `~`은 보수가 아닌 not이기 때문) 그럼 `~~k`는?? 그렇다 원래대로 8이다.

이 **double tilde**연산자가 어떻게 유용하게 사용되는지 알아보자.

##### 1. Math.floor() 대신 사용될 수 있다.

숫자에 `~`연산을 하게되면서 소수점들은 버려지게된다. 즉, `~~`를 활용하면 `Math.floor()` 처럼 활용할 수 있다.

`Math.floor()` 대신에 활용하는데 장점과 단점이 있다.

#### 장점

속도 측면에서 `~~`가 빠르다고 한다. [The Mysterious Double Tilde (`~~`) Operation](https://dev.to/asadm/the-mysterious-double-tilde-operation-mih) 이 블로그에 보면 크롬, Safari,iPhone XS 각각`Math.floor`, `~~`, `parseInt` 의 속도를 비교한 결과가 있다.

결과는 `~~` , `Math.floor`, `parseInt` 순으로 `~~`가 가장 빠른 퍼포먼스를 보여줬다.

#### 단점

코드에 `~~`가 덕지덕지 있다고 상상해보자. 이해가 쉽지 않을 것이다. 복잡한 코드 또는 협업하는 과정에서는 가독성이 좋지 않기 때문에 사용하지 않는편이 좋을 것 같다.

##### 2. undefined 또는 null을 0으로 변환할 때 사용될 수 있다.

위의 경우가 언제쓰일까 싶지만 `undefined+3 => NaN` 이런 상황이 자주 있었을 것이다.

예시 하나만 작성해 보자.  
배열[1,1,1,2,2,3,3,3,3]을 각 숫자가 몇개인지 객체에 저장해보자.

```javascript
const arr = [1,1,1,2,2,3,3,3,3]
const obj1 = {}

arr.forEach(v=>{
  if(obj1[v]) obj1[v]+=1
  else obj1[v]=1
})
//obj1 {1: 3, 2: 2, 3: 4}
```

위와같이 obj에 key값이 있는지 확인해주는 작업이 필요하다. 그렇지 않으면 NaN이 나올 것이다.

그럼 `~~`를 활용해 다시 코드를 작성해보자.

```javascript
const obj2 = {}
arr.forEach(v=>obj2[v]= ~~obj2[v]+1)
//obj2 {1: 3, 2: 2, 3: 4}
```

`undefined`가 나올 수 있는 상황을 `~~`연산자를 이용해서 간단하게 해결할 수 있다. 알고리즘 답안을 보면서 배운 연산자인 만큼 알고리즘에서 유용하게 사용할 수 있을 것 같다.

#### Math.min과 max를 대체하는것

JavaScript를 쓰다보면 사용자에게 통계를 보여줘야 하거나 슬라이더의 경계를 설정할 경우 배열에서 최소 값과 최대 값을 알아내야 할 때가 있습니다. 이 글에서는 JavaScript 배열에서 최소/최대 값을 알아내는 방법을 알아볼 것이고 이들 사이의 속도를 비교해 보겠습니다.

##### **Math 객체를 이용해 Min, Max 얻기**

Math는 수학적 계산을 위해 필요할만한 많은 유용한 메소드와 상수를 가지고있는 JavaScript의 내장 객체입니다. 이 글에서는 두개의 메소드를 살펴볼 것인데 Math.min()과 Math.max() 입니다. 두 메소드 다 매개변수로 숫자의 목록을 받고 메소드 이름에서 알 수 있듯이 하나는 목록의 가장 작은 값을 반환하고 다른 하나는 목록의 가장 큰 값을 반환하는 기능을 합니다.

```
console.log(Math.min(20, 23, 27)); // 20
console.log(Math.max(20, 23, 27)); // 27

console.log(Math.min(-20, -23, -27)); // -27
console.log(Math.max(-20, -23, -27)); // -20
```

만일 매개변수 중 하나라도 숫자가 아니거나 숫자로 변환할 수 없다면 Math.min()과 Math.max()는 NaN을 반환합니다.

```
console.log(Math.min('-20', -23, -27)); // -27
console.log(Math.max('number', -23, -27)); // NaN
```

![](https://blog.kakaocdn.net/dn/dN2eS8/btrGx4fUDo8/FOFK8mK8p3PnOWUHYPDOSk/img.png)

비슷하게 Math.min() 메소드에 매개변수로 숫자의 배열인 객체를 전달하면 NaN을 반환합니다. 왜냐하면 이는 배열 단일 항목으로 취급되고 이 객체를 숫자 값으로 변환할 수 없기 때문입니다.

```
const myArray = [2, 3, 1];
console.log(Math.min(myArray)); // NaN
```

그러나 ... 연산자를 이용하여 배열 객체를 풀어서 전달하면 올바른 결과를 얻을 수 있습니다.

```
const myArray = [2, 3, 1];
console.log(Math.min(...myArray)); // 1
```

##### **reduce()를 활용해 Min, Max 얻기**

reduction 연산은 함수 프로그래밍에서 다양한 분야에서 쓸 수있는 가장 강력한 연산일 것 입니다. reduce() 함수는 각 배열 요소에 대해 리듀서 함수(콜백으로 정의)를 실행하고 마지막에 단일 값을 반환합니다. 이 방법은 여러 부분에 보편적으로 적용할 수 있기 때문에 다루어 볼 가치가 있습니다.

```
const myArray = [20, 23, 27];

let minElement = myArray.reduce((a, b) => {
    return Math.min(a, b);
});

console.log(minElement); // 20
```

![](https://blog.kakaocdn.net/dn/ctFfFf/btrGthIkKFT/AxKY5bv4QTxKP4a3ZdzyHk/img.png)

##### **apply()로 Min, Max 찾기**

apply() 메소드는 매개변수로 this 값과 배열을 받아 함수를 실행하는데 사용합니다. 이를 통해 Math.min() 정적 함수에 배열을 입력할 수 있습니다.

```
const myArray = [20, 23, 27];

let minElement = Math.min.apply(Math, myArray);
console.log(minElement); // 20
// Or
let minElement = Math.min.apply(null, myArray);
console.log(minElement); // 20
```

##### **기본 for loop 사용**

루프는 JavaScript에서 조건에 따라 반복되는 작업을 수행하기 위해 사용되고 이는 true 또는 false를 반환합니다. 정의된 조건이 false를 반환할 때 까지 루프는 계속 실행됩니다. 우리의 경우 코드를 반복적으로 사용하기 위해 for loop를 사용하겠습니다.

##### **최소값 구하기:**

먼저 최소 요소로 배열의 첫 번째 값으로 설정하고 전체 배열을 반복하여 다른 요소의 값이 현재 최소 값보다 작은지 확인합니다. 다른 요소의 값이 최소 값보다 작은 경우 최소 값을 요소의 값으로 변경합니다.

```
const myArray = [20, 23, 27];

let minElement = myArray[0];
for (let i = 1; i < arrayLength; ++i) {
    if (myArray[i] < minElement) {
        minElement = myArray[i];
    }
}

console.log(minElement); // 20
```

##### **최대값 구하기:**

최대 값을 배열의 첫 번째 값으로 초기화합니다. 그 다음 전체 배열을 반복하여 초기화된 값 보다 큰 요소가 있는지 확인합니다.

```
const myArray = [20, 23, 27];

let maxElement = myArray[0];
for (let i = 1; i < arrayLength; ++i) {
    if (myArray[i] > maxElement) {
        maxElement = myArray[i];
    }
}

console.log(maxElement); // 27
```

##### **속도 비교**

벤치마크 사이트를 이용해 각 코드에 100개의 항목에서 1000000개 항목의 배열을 입력했습니다. 성능은 배열의 길이에 따라 상대적입니다.

- 작은 크기의 배열(100): reduce() 메소드가 가장 나았습니다. 그다음 표준 for loop, [스프레드 연산](https://sisiblog.tistory.com/370), apply() 메소드 순서입니다. 이들의 시간차 성능은거의 비슷비슷합니다.
- 중간 배열(1000): 표준 for loop가 최고입니다. 다음으로 reduce() 메소드, [스프레드 연산](https://sisiblog.tistory.com/370), apply() 메소드 순 입니다. 이 부분에서는 표준 for loop가 reduce() 보다 월등히 빨랐습니다.
- 아주 큰 배열(1000000): 표준 루프가 다른 모든 방법보다 월등히 뛰어났습니다.

표준 루프는 확장성이 뛰어나고 소형 배열에 적용할 때만 조금 밀렸습니다. 작은 항목이나 더 작은 배열을 다룰 경우 모든 방법이 원활히 작동합니다. 배열이 커질 수록 표준 for loop를 사용에 이점이 커집니다.

##### **정리**

이 가이드에서는 JavaScript 배열에서 최소/최대 값을 찾는 방법을 알아봤습니다. 구체적으로 Math.min()과 Math.max() 메소드, reduce() 메소드, apply() 메소드, for loop를 알아봤습니다. 마지막으로 벤치마크를 해봤는데 작은 배열에서는 별다른 속도 차이가 없었는데 큰 배열에서는 기본 for loop가 가장 나은 결과를 보였습니다.
