# Part 4. JavaScript 마스터

**.DS_Stpre 삭제, 생기지 않게 하는 방법 :**
> Finder에서 .DS_Store 생성 자체를 막기
```javascript
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
defaults write com.apple.desktopservices DSDontWriteUSBStores -bool true

// Finder 재시작
killall Finder
```

> 개발 프로젝트에서는 Git에서 무시하기 (필수)
```javascript
// .gitignore에 추가
.DS_Store

// 이미 생긴 파일이 있다면 한 번 제거
find . -name ".DS_Store" -delete
```

<br />
<br />

**Homebrew는 macOS의 패키지 관리 툴 :**
> 맥북 사용자라면 필수적으로 설치해야할 프로그램 중 하나이다.<br />
> 간단한 명령어로 다양한 소프트웨어를 설치, 관리, 제거할 수 있어 매우 유용하다!<br />
> (macOS, Homebrew로 `npm`, `nvm`, `git` 설치)<br />
> [Homebrew 공홈 바로가기](https://brew.sh/ko/)

<br />
<hr />
<br />

## 목차
<details>
    <summary>Ch 01. Node JS</summary>

    - [1-1. Node.js 다운로드](#1-1-nodejs-다운로드)
    - [1-2. npm](#1-2-npm)
    - [1-3. CDN vs npm](#1-3-cdn-vs-npm)
    - [1-4. npm 설치](#1-4-npm-설치)
    - [1-5. Parcel, 개발 서버 실행과 빌드](#1-5-parcel-개발-서버-실행과-빌드)
    - [1-6. 유의적 버전(Semver)](#1-6-유의적-버전semver)
</details>

<details>
    <summary>Ch 02. JS 데이터</summary>

    - [2-1. 원시형 - String, Number](#2-1-원시형---string-number)
    - [2-2. 원시형 - Boolean, null, undefined](#2-2-원시형---boolean-null-undefined)
    - [2-3. 참조형 - Array](#2-3-참조형---array)
    - [2-4. 참조형 - Object](#2-4-참조형---object)
    - [2-5. 참조형 - Function](#2-5-참조형---function)
    - [2-6. 형 변환(Type Conversion)](#2-6-형-변환type-conversion)
    - [2-7. 참과 거짓(Truthy & Falsy)](#2-7-참과-거짓truthy--falsy)
    - [2-8. 데이터 타입 확인](#2-8-데이터-타입-확인)
</details>

<details>
    <summary>Ch 03. 연산자와 구문</summary>

    - [3-1. 산술, 할당, 증감 연산자](#3-1-산술-할당-증감-연산자)
    - [3-2. 부정, 비교 연산자](#3-2-부정-비교-연산자)
    - [3-3. 논리 연산자](#3-3-논리-연산자)
    - [3-4. Nullish 병합, 삼항 연산자](#3-4-nullish-병합-삼항-연산자)
    - [3-5. 전개 연산자](#3-5-전개-연산자)
    - [3-6. 구조 분해 할당(Destructuring assignment)](#3-6-구조-분해-할당destructuring-assignment)
    - [3-7. 선택적 체이닝](#3-7-선택적-체이닝)
    - [3-8. if, Switch 조건문](#3-8-if-switch-조건문)
    - [3-9. For, For of, For in 반복문](#3-9-for-for-of-for-in-반복문)
    - [3-10. While, Do while 반복문](#3-10-while-do-while-반복문)
</details>

<br />
<hr />
<br />

## Ch 01. Node JS
`Node.js`는 Chrome V8 JavaScript 엔진으로 빌드된 **JavaScript 런타임**(프로그래밍 언어가 동작하는 환경).  

<br />

### 1-1. Node.js 다운로드
- [node 공식 - 다운로드](https://nodejs.org/ko/download)
- 환경에 맞게 다운로드 설치.

<br />

### 1-2. npm
- `npm`(node package manager)은 전 세계의 개발자들이 만든 다양한 기능(패키지, 모듈)들을 관리.
- `npm install ???` 으로 설치하여 사용 할 수 있다.

#### npm을 사용하는 이유
`node.js` 환경에서는 `npm`을 통해 필요한 패키지를 직접 설치하고 버전을 관리하며 사용한다.<br />
이 방식은 초기에는 설정과 개념을 이해해야 해서 다소 복잡하지만,<br />
의존성 관리와 확장성이 뛰어나 프로젝트를 체계적으로 관리할 수 있다.<br />
그 결과, 비교적 적은 시간으로도 복잡한 기능을 안정적으로 추가하고 고도화할 수 있다.<br />
이처럼 초기 복잡함을 감수하고 장기적인 효율을 얻는 선택을 트레이드 오프라고 한다.

<br />

### 1-3. CDN vs npm
<details> 
    <summary>CDN 방식</summary>

- 라이브러리를 외부 서버에서 바로 불러와 사용
- 설정이 거의 없고 빠르게 시작 가능
- 프로젝트가 커지면 파일 여러 곳에 링크가 흩어져 버전/의존성 관리가 어려움
</details>

<details> 
    <summary>npm 방식</summary>

- 라이브러리를 프로젝트 안에 설치해서 사용
- 설정과 빌드 과정이 필요함
- `package.json`(및 lock 파일)에 사용 패키지/버전이 기록되어 **버전 고정·의존성 관리·환경 재현에 유리**함
- 팀/대규모 프로젝트에 특히 적합
- [npm 공홈 바로가기](https://www.npmjs.com/)
</details>

<br />

### 1-4. npm 설치
<details> 
    <summary>npm으로 패키지 설치하기</summary>

```javascript
// 1. 기본적인 질문 생략하고 package.json 생성
// 폴더명에 특수문자, 한글 등으로 error일 경우, npm init 으로 설치하면서 name(폴더 이름) 직접 지정.
npm init -y

// 2. lodash 패키지 설치
npm install lodash

// 3. package.json 파일 - "dependencies" 영역에서 설치 확인 가능
// 패키지 설치되면 node_modules > lodash 생성
"dependencies": {
    "lodash": "^1.0.2"
}

// 4. parcel 패키지 설치
// parcel 이라는 번들러(bundler) 설치
// parcel(파셀) : 웹사이트에서 동작할 내용들을 하나로 묶어주는 역할
// parcel(파셀) 패키지는 실제 웹사이트에서 동작하는 것이 아니기 때문에 --save-dev 를 붙여서 설치.
// --save-dev : 개발할 때만 동작하는 패키지.
npm install parcel --save-dev

npm install --save-dev parcel

npm install -D parcel

/**
 * 프로젝트를 진행할 떄,
 * 실제 웹사이트 동작에 필요한 것인지? 개발할 떄만 필요한 것인지?
 * 구분할 줄 알아야 한다.!!
 */ 

// 5. package.json 파일 - "devDependencies" 영역에서 설치 확인 가능
"devDependencies": {
    "parcel": "^2.16.3"
}

// 프로젝트가 직접적으로 의존하고 있는 패키지들을 관리하는 package.json 파일
// 설치된 패키지들이 또 추가적으로 의존하고 있는 다른 패키지들의 관계 정보를 가지고 있는 package-rock.json 파일

// 6. node_modules 삭제 또는 폴더가 없을 때
// package.json 파일(관련 패키지 파일 정보)이 있기 떄문에 npm install 또는 npm i 설치
npm istall

// 7. .gitignore 파일 생성
// package.json 파일에서 버전 관리를 하기 때문에 github 같은 저장소에 업로드 할 필요가 없다.
// 그렇기 때문에 .gitignore 파일에 적용하여 저장소에 업로드되지 않게 한다.
node_modules
```
</details>

<br />

### 1-5. Parcel, 개발 서버 실행과 빌드
- [Parcel(파셀) 공홈, 바로가기](https://parceljs.org/)

<details> 
    <summary>Parcel 설정 순서 (파일 구성 → scripts → dev/build)</summary>

1. `index.html` 파일 생성
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>

        <!-- main.js 파일 생성 -->
        <!-- type="module" defer 추가 -->
        <!-- defer : HTML 파싱을 멈추지 않고 JS를 내려받고, DOM이 모두 만들어진 뒤에 실행 -->
        <script type="module" defer src="./main.js"></script>
    </head>
    <body>
        
    </body>
    </html>
    ```

2. `main.js` 파일 생성
    ```javascript
    import _ from "lodash"

    console.log(_.upperCase("hello-world"))
    ```

3. `package.json` : dev, build 명령어 만들기
    ```javascript
    "scripts": {
        "dev": "parcel ./index.html",
        "build": "parcel build ./index.html"
    },

    // npm run dev 로 로컬 서버 실행하기.
    // dev 또는 build로 실행했을 때, main: index.js 같은 에러 메세지가 보일 때 삭제.
    // parcel은 dev 또는 build 과정에서 main 옵션 영향을 받고 있다는 것을 알 수 있고,
    // 현재 상황에선 삭제(제거)해도 무방하다.
    ```

4. `main.ts` 파일 생성 : `Parcel(파슬)`은 ts(타입스크립트)도 지원해 준다.
    - `dist` 폴더 안에 `javascript`로 변환하여 `index.html`에 적용해 준 것을 확인할 수 있다.
    ```typescript
    import _ from "lodash"

    console.log(_.upperCase("hello-world"))

    interface User {
        name: string,
        age: number
    }

    const user: User = {
        name: 'Heropy',
        age: 85
    }

    console.log(user)
    ```
</details>

<br />

### 1-6. 유의적 버전(Semver)
<details>
    <summary>Major.Minor.Patch</summary>

```text
Major.Minor.Patch
4.17.21 

Major : 기존 버전과 호환되지 않는 새로운 버전.
Minor : 기존 버전과 호환되는 기능이 추가된 버전.
Patch : 기존 버전과 호환되는 버그 및 오타 등이 수정된 버전.
```
</details>

<details>
    <summary>^Major.Minor.Patch</summary>

```text
^Major.Minor.Patch
^4.17.21 

^Major : Major 버전 안에서 가장 최신 버전으로 업데이트 가능.
    ㄴ 4버전 안에서 Minor, Patch 최신 버전 업데이트 가능

npm info 패키지이름
    ㄴ ex. lodash => npm info lodash
    ㄴ dist-tags: latest: 4.17.21 
    ㄴ 가장 최신 버전을 알 수 있다.
    ㄴ 업데이트 필요 시, npm update lodash
    ㄴ ^Major(^4)가 아닌 Major(4)로 표기되어 있을 경우, npm update를 해도 업데이트 되지 않음.
```
</details>

<details>
    <summary>~Major.Minor.Patch</summary>

```text
~Major.Minor.Patch
~4.17.21 

Minor 버전 안에서 가장 최신 버전으로 업데이트 가능
즉, Patch 버전만 가장 최신 버전으로 업데이트 된다는 개념.
```
</details>

<br />
<hr />
<br />

## Ch 02. JS 데이터

<details>
    <summary>원시형 - 원시 타입 (Primitive Types)</summary>

- 값 자체가 저장됨 (참조가 아님)
- 불변(immutable)
- 복사 시 값이 그대로 복사됨
- 메서드를 호출하면 일시적으로 객체로 감싸졌다가 바로 해제됨

```text
string
number
bigint
boolean
undefined
symbol
null
```
</details>

<details>
  <summary>원시 타입 vs 객체 타입</summary>

| 구분    | 원시 타입              | 객체 타입                         |
| ----- | ------------------ | ----------------------------- |
| 저장 방식 | 값 자체               | 참조                            |
| 변경 가능 | 불변                 | 가변                            |
| 비교    | 값 비교               | 참조 비교                         |
| 예시    | `number`, `string` | `object`, `array`, `function` |

</details>

<br />

### 2-1. 원시형 - String, Number
<details>
  <summary>string (문자열)</summary>

```javascript
const string1 = "Hello"
const string2 = 'hello'
const string3 = `hello ${string1} ?!` // 템플릿 리터럴 방식

console.log(string1, string2, string3)
```
</details>

<details>
  <summary>number (숫자)</summary>

```javascript
const number = 123
const pi = 3.14

console.log(number + 1)
console.log(pi)

/**
 *  typeof (number + underfined)
 *    NaN은 typeof가 'number'이지만, 유효한 숫자 결과가 아니라 “계산 불가/변환 실패”를 의미한다.
 *    따라서 피연산자 중 하나가 숫자로 정상 변환되지 않았을 가능성이 있으므로 값/타입 검증이 필요하다.
 *    일반적으로 정상 케이스가 아니라 예외(오류 신호)에 가까운 값으로 해석한다.
 */
console.log(number + undefined)

/**
 * 사람은 10진수로 생각하여 0.1 + 0.2 = 0.3을 기대한다.
 * 자바스크립트는 숫자를 2진수(부동소수점)로 저장하며,
 * 0.1, 0.2는 2진수로 정확히 표현되지 않아 근사값으로 저장된다.
 * 이 근사값으로 연산하면서 미세한 오차가 결과에 나타난다.
 */
const a = 0.1;
const b = 0.2;

console.log(a + b); // 기대값: 0.3

/**
 *  toFixed(1)
 *    toFixed는 반올림 + 자리수 고정, 반환값은 문자열
 *    화면 출력(표시용)에는 적합하지만 숫자 계산용으로는 부적함.
 */
const result = (0.1 + 0.2).toFixed(1);
console.log(result); // "0.3"

// 숫자 타입을 유지하면서 0.3으로 만들기
const result = Number((0.1 + 0.2).toFixed(1));
console.log(result); // 0.3

/**
 *  오차 보정 방식 (계산 중심)
 *    소수점을 정수로 바꿔 계산 → 다시 나눔
 *    금융, 수량 계산 쪽에서 자주 사용
 *    자리수가 명확할 때만 안전
 */ 
const result = Math.round((0.1 + 0.2) * 10) / 10;
console.log(result); // 0.3
```
</details>

<br />

### 2-2. 원시형 - Boolean, null, undefined
<details>
  <summary>Boolean</summary>

```javascript
const a = true
const b = false
```
</details>

<details>
  <summary>null</summary>

```javascript
/**
 *  명시적
 *  개발자가 의도적으로 '값이 없음'을 할당한 상태
 *  값이 비어있음을 표현하기 위한 값
 */
let age = null
```
</details>

<details>
  <summary>undefined</summary>

```javascript
/**
 *  암시적
 *  변수에 값이 할당되지 않은 상태
 *  선언만 되었을 때 자바스크립트가 자동으로 부여하는 값
 */
let age;
console.log(age); // undefined
```
</details>

<br />

### 2-3. 참조형 - Array
<details>
  <summary>Array (배열)</summary>

```javascript
// 생성자 함수로 배열 생성
const fruits1 = new Array('Apple', 'Banana','Cherru');

// 대괄호 기호로 배열 생성 - Array Literal(배열 리터럴) 방식
const fruits2 = ['Apple', 'Banana','Cherru'];

// 인덱싱 대괄호 표기법
console.log(fruits1[1]); // 아이템 또는 요소(배열의 앨리먼트) 콘솔로그로 읽기.

// length 로 아이템 개수를 반환
console.log(fruits1.length);     // 총 3개
console.log(fruits1.length - 1); // 배열의 마지막 인덱스 값을 추출할 떄
```
</details>

<br />

### 2-4. 참조형 - Object
<details>
  <summary>생성자 함수로 객체 데이터 생성</summary>

```javascript
//생성자 함수로 객체 데이터 생성
const user = new Object();
user.name = 'HEROPY'
user.age = 85

/**
 *  {name: "HEROPY", age: 85}
 *    ㄴ 객체는 key: value 형태
 *    ㄴ key   => 속성(property, 프로퍼티)
 *    ㄴ value => 값 
 */
console.log(user);
```
</details>

<details>
  <summary>함수로 객체 데이터 생성</summary>

```javascript
/**
 *  함수 내부에서 this 라는 키워드를 통해
 *  각각의 속성의 값을 추가한 방식으로 객체 데이터 생성 
 */
function User() {
    this.name = "HEROPY"
    this.age = 85
}

const user = new User();

console.log(user); // User{name: "HEROPY", age: 85}
```
</details>

<details>
  <summary>기호를 통해 객체 데이터 생성 - 리터럴 방식</summary>

```javascript
const user = {
    name: "HEROPY",
    age: 85
}

console.log(user); // {name: "HEROPY", age: 85}
console.log(user.name);    // 점표기법 - "HEROPY"
console.log(user["name"]); // 대괄호 표기법 - "HEROPY"

// key를 변수에 담아 이용할 수도 있다
const key = "name"
console.log(user[key]);    // 대괄호 표기법 - "HEROPY"

/**
 *  갹체 데이터 안에 들어있는 각각의 속성들의 이름은 고유하기 떄문에
 *  순서라는 개념이 없다. 
 *  먼저 만들었다고 해서 콘솔로그에 동일한 순서로 나오지 않는다는 의미.
 *  age 라는 속성을 한 개 더 만들어서 30 으로 지정했다면
 *  마지막에 작성된 age의 값이 적용 된다.
 */
const user = {
    name: "HEROPY",
    age: 85,
    age: 30,
}
```
</details>

<br />

### 2-5. 참조형 - Function
<details>
  <summary>함수 선언문</summary>

```javascript
// 함수 선언문
function hello() {
    console.log("hello")
}

// call (실행)
hello();

console.log(hello);           // 하나의 데이터 hello { console.log("hello") } 가 출력
console.log(typeof hello);    // function
console.log(hello());         // hello() 함수가 실행되어 "hello" 가 출력
console.log(typeof hello());  // string
```
</details>

<details>
  <summary>함수 표현식</summary>

```javascript
const getNumber = function() {
    return 123;
}

// 예제
const a = function () {
    console.log("A")
}

const b = function (c) {
    console.log(c);
    c();
}

b(a);
/**
 * console.log(c)
 *   ㄴ function () { console.log("A") }
 * c();
 *   ㄴ "A"
 */
```
</details>

<br />

### 2-6. 형 변환(Type Conversion)
<details>
  <summary>동등 연산자 ==</summary>

```javascript
const a = 1;   // number
const b = "1"; // string

// 동등 연산자 ==
console.log(a == b)  // true
```
</details>

<details>
  <summary>일치 연산자 ===</summary>

```javascript
const a = 1;   // number
const b = "1"; // string

// 일치 연산자 ===
console.log(a === b) // false
```
</details>

<details>
  <summary>타입 변환 없이 값과 타입을 함께 비교하려면 === 사용</summary>

```javascript
/**
 *  서로 정확하게! 같은 타입의 데이터인지 비교하기 위해선
 *  동등 연산자(==) 보단 일치 연산자(===) 을 사용해야 한다!
 */

// 다른 예시 1
const a = 0;
const b = false;

console.log(a == b);  // true
console.log(a === b); // false

// 다른 예시 2
const a = 1;
const b = true;

console.log(a == b);  // true
console.log(a === b); // false
```
</details>

<br />

### 2-7. 참과 거짓(Truthy & Falsy)
<details> 
    <summary>거짓에 해당하는 데이터만 알고 있기</summary>

```javascript
// 거짓에 해당하는 데이터만 알고 있기
false
0
null
undefined
NaN
''  // 빈 문자열
0n. // 빅 인트
```
</details>

<br />

### 2-8. 데이터 타입 확인
<details> 
    <summary>데이터 타입 확인 (typeof / constructor / toString)</summary>

```javascript
console.log(typeof 'Hello' === 'string');          // true
console.log(typeof 123 === 'number');              // true
console.log(typeof false === 'boolean');           // true
console.log(typeof undefined === 'undefined');     // true
console.log(typeof function () {} === 'function'); // true

// typeof null, []. {} 모두 object로 인식.
console.log(typeof null === 'object'); // true
console.log(typeof [] === 'object');   // true
console.log(typeof {} === 'object');   // true

console.log([].constructor === Array)  // true
console.log({}.constructor === Object) // true

// null 데이터에는 constructor 없다.
console.log(Object.prototype.toString.call(null).slice(8, -1) === 'Null') // true
```
</details>

<details> 
    <summary>데이터 타입 체크 함수</summary>

```javascript
// 타입 이름 대문자로 출력할 떄
function checkType(data) {
    return Object.prototype.toString.call(data).slice(8, -1)
}

console.log(checkType("Hello") === 'String'); // true
console.log(checkType(null) === 'Null');      // true

// 타입 이름 소문자로 출력할 떄
function checkType(data) {
    return Object.prototype.toString.call(data).slice(8, -1).toLowerCase();
}

console.log(checkType("Hello") === 'string'); // true
console.log(checkType(null) === 'null');      // true
```
</details>

<br />
<hr />
<br />

## Ch 03. 연산자와 구문
### 3-1. 산술, 할당, 증감 연산자
<details>
    <summary>산술(Arithmetic) 연산자</summary>

```javascript
// console.log(1(피연산자) +(연산자) 2(피연산자));
console.log(1 + 2);  // 2
console.log(5 - 7);  // -2
console.log(3 * 4);  // 12
console.log(10 / 2); // 5
console.log(7 % 5);  // 2 || 나머지 연산자

/**
 *  짝수 홀수 구분하기
 *    2로 나눴을 때, 0 이면 true, 0 이 아니면 false
 */
function isEven(num) {
    return num % 2 === 0
}
```
</details>

<details>
    <summary>할당(Assignment) 연산자</summary>

```javascript
// =(equal, 이퀄) 기호를 통해서 할당해서 집어 넣겠다는 의미
const a = 3;

let b = 3;

// b = b + 2;
b += 2;       // 축약해서 사용 가능
b -= 2;
b *= 2;
b /= 2;
b %= 2;
```
</details>

<details>
    <summary>증감(Increment 증가 & Decrement 감소) 연산자</summary>

```javascript
// 증가 연산자
let a = 3;

console.log(a++); // 3
console.log(a);   // 4
console.log(++a); // 5

// 감소 연산자
let b = 3;
console.log(b--); // 3
console.log(b);   // 2
console.log(--b);  // 1
```

> 증가 혹은 감소 연산자는<br /> 
> 그 기호를 변수의 앞이나 뒤에 붙이는 것에 따라서<br />
> 전혀 다른 결과가 나올 수 있다.
</details>

<br />

### 3-2. 부정, 비교 연산자
<details>
    <summary>부정(Negation) 연산자</summary>

```javascript
/**
 *  부정 연산자 => !(느낌표)
 *    부정 연산자는 항상 결과값을 boolean(블린) 데이터로 출력
 */
console.log(true);       // true
console.log(!true);      // false
console.log(!!true);     // true

console.log(0);          // false
console.log(!0);         // true
console.log(!null);      // true
console.log(!undefined); // true
```
</details>

<details>
    <summary>비교(Comparison) 연산자</summary>

```javascript
const a = 1;
const b = 3;

// 동등 (형 변환!)
console.log(a == b);  // false

// 부등
console.log(a != b);  // true

// 일치
console.log(a === b); // false

// 뷸일치
console.log(a !== b); // true

// 크다
console.log(a > b);   // false

// 크거나 같음
console.log(a >= b);  // false

// 작다
console.log(a < b);   // true

// 크거나 같음
console.log(a <= b);  // true
```
</details>

<br />

### 3-3. 논리 연산자
<details>
    <summary>조건식에서 사용하는 논리 연산자(AND/OR)</summary>

```javascript
const a = true
const b = false

// AND(그리고) 연산자
if (a && b) {
    console.log('모두가 참!');
}

// OR(또는) 연산자
if (a || b) {
    console.log('하나 이상이 참!')
}
```
</details>

<br />

#### 단락 평가(Short-circuit Evaluation)의 의미
> 논리 연산자(&&, ||)는 왼쪽부터 순서대로 평가한다<br />
> 평가 도중에 전체 결과가 확정되면,<br />
> 뒤에 남은 표현식은 실행·평가하지 않는다.<br />
> 이 “중간에 끊고 나가는 평가 방식”을 단락 평가라고 한다.

<details>
    <summary>AND(&&) 연산자의 단락 평가와 반환값</summary>

```javascript
/**
 *  &&는 참/거짓을 반환하는 연산자가 아니다
 *  왼쪽부터 평가한다
 */
console.log(true && false); // false
console.log(1 && 0);        // 0
console.log(1 && 2 && 0);   // 0

// 처음 만나는 거짓 값을 그대로 반환하고 끝낸다
console.log(1 && 0 && 2);     // 0
console.log(0 && 1 && 2);     // 0
console.log(0 && false && 2); // 0
console.log(false && 0 && 2); // false

// 거짓이 하나도 없으면 마지막 값을 반환한다 
console.log('A' && 'B' && 'C'); // C
```
</details>

<details>
    <summary>OR(||) 연산자의 단락 평가와 반환값</summary>

```javascript
/**
 *  ||는 참/거짓을 반환하는 연산자가 아니다
 *  왼쪽부터 평가한다
 */
console.log(false || true); // true
console.log(0 || 1);        // 1

// 처음 만나는 참 값을 그대로 반환하고 끝낸다
console.log(false || 0 || 1);                   // 1
console.log(false || 0 || {});                  // {}
console.log(false || [] || null);               // []
console.log(function () {} || undefined || ''); // function () {}

// 참이 하나도 없으면 마지막 값을 반환한다
console.log(false || 0 || NaN); // NaN
```
</details>

<details>
  <summary>[], {} 의 truthy / falsy</summary>

```javascript
/**
 *  [] 와 {} 는 비어 있어도 falsy가 아니다
 *  배열과 객체는 항상 truthy로 평가된다.
 * 
 *  falsy로 평가되는 값은 정해진 것만 있다
 *  false, 0, "", null, undefined, NaN
 */
console.log(Boolean([])); // true
console.log(Boolean({})); // true

/**
 *  "비어 있음"을 판단하려면 값 자체([] / {})를 평가하지 않는다
 *  숫자 값을 꺼내서 평가한다
 *
 *  배열: length
 *  객체: Object.keys(obj).length
 */
console.log([].length);                   // 0
console.log([1, 2].length);               // 2
console.log(Object.keys({}).length);      // 0
console.log(Object.keys({ a: 1 }).length); // 1
```
</details>

<br />

### 3-4. Nullish 병합, 삼항 연산자
<details>
    <summary>Nullish 병합(Coalescing) - ?? 표기</summary>

```javascript
const n = 0;

// OR 연산자를 사용한 경우
// 왼쪽부터 검사, 거짓 X 참 O
const num1 = n || 7
console.log(num1) // 7

// Nullish 병합 연산자를 사용한 경우
// null, undefined가 아니면 return
const num2 = n ?? 7
console.log(num2) // 0

console.lgg(null ?? 1);       // 1
console.log(undefined ?? 2);  // 2

// null·undefined가 아닌 첫 값을 반환
console.log(null ?? 1 ?? 2);  // 1
console.log(false ?? 1 ?? 2); //false

// null·undefined가 아닌 첫 값을 반환하고
// 없으면 마지막 값 null·undefined 반환
console.log(null ?? undefined); // undefined
console.log(undefined ?? null); // null
```
</details>

<details>
    <summary>삼항(Ternary) 연산자</summary>

```javascript
const a = 1;

if (a < 2) {
    console.log('참!')
} else {
    console.log('거짓')
}

/**
 *  삼항 연산자
 *    조건 ? 참 : 거짓
 */
console.log(a < 2 ? '참' : '거짓');

// 예시
function getAlert(message) {
    // return message ? message : '메세지가 존재하지 않습니다!'
    return message 
            ? message 
            : '메세지가 존재하지 않습니다!'
}

console.log(getAlert('안녕하세요'));  // 안녕하세요
console.log(getAlert(''));         // 메세지가 존재하지 않습니다!
```
</details>

<br />

### 3-5. 전개 연산자
<details>
    <summary>전개 연산자(Spread Operator)</summary>

```javascript
const a = [1, 2, 3];
/**
 *  전개 연산자(...)를 배열에 쓰면
 *    배열을 그대로 넘기는 게 아니라, 배열 안의 요소들을 하나씩 꺼내서 펼쳐 넣는다.
 *    예) console.log(...a)  // console.log(a[0], a[1], a[2]) 와 같은 의미
 */
console.log(...a); // 1,2,3
```
</details>

<details>
    <summary>배열 병합: concat() vs 전개 연산자(...)</summary>

```javascript
const a = [1, 2, 3];
const b = [4, 5, 6];

// 배열 병합: concat() 메소드
const c = a.concat(b);
console.log(c); // [1, 2, 3, 4, 5, 6]

// concat() 메서드가 아닌 전개 연산자(...)로 병합
const d = [...a, ...b];
console.log(d); // [1, 2, 3, 4, 5, 6]
```
</details>

<details>
    <summary>객체 병합: Object.assign() vs 전개 연산자(...)</summary>

```javascript
const a = { x: 1, y: 2 };
const b = { y: 3, z: 4 };

/**
 *  객체 병합: Object.assign()
 *    {}를 target으로 주면 새 객체에 병합 결과를 만든다(원본 a, b 유지).
 *    같은 키(y)는 뒤에 오는 b 값으로 덮어쓴다.
 *    결과: { x: 1, y: 3, z: 4 }
 */
const c = Object.assign({}, a, b);
console.log(c); // { x: 1, y: 3, z: 4}

// 전개 연산자(...)
const d = {...a, ...b}
console.log(d) // { x: 1, y: 3, z: 4}
```
</details>

<details>
    <summary>★ Object.assign() 메서드 정리</summary>

```javascript
/**
 * 1) 기본 형태
 *   - Object.assign(target, ...sources)
 *   - sources의 속성을 target에 복사해서 병합한다.
 *
 * 2) 덮어쓰기 규칙
 *   - 같은 키가 있으면 뒤에 오는 source 값이 앞의 값을 덮어쓴다.
 *     예) Object.assign({}, { y: 2 }, { y: 3 })  // 결과: { y: 3 }
 *
 * 3) target에 따른 차이(중요)
 *   - Object.assign(a, b)     : a 자체가 변경된다(원본 변경)
 *   - Object.assign({}, a, b) : 새 객체에 결과를 담는다(원본 유지)
 *
 * 4) 반환값
 *   - 반환값은 항상 target이다.
 *     예) const c = Object.assign(a, b);  // c와 a는 같은 객체
 */
```
</details>

<details>
    <summary>★ Object.assign() 메서드 예시</summary>

```javascript
// 1) 기본 형태: Object.assign(target, ...sources)
const t1 = {};
const s1 = { x: 1 };
const s2 = { y: 2 };

const r1 = Object.assign(t1, s1, s2);
console.log(r1);        // { x: 1, y: 2 }
console.log(t1);        // { x: 1, y: 2 }  (target에 복사되어 병합됨)
console.log(r1 === t1); // true (반환값은 target)

// 2) 덮어쓰기 규칙: 같은 키는 뒤의 source가 덮어씀
const r2 = Object.assign({}, { y: 2 }, { y: 3 });
console.log(r2); // { y: 3 }

// 3) target에 따른 차이(중요)
// 3-1) Object.assign(a, b) : a 원본이 변경됨
const a = { x: 1, y: 2 };
const b = { y: 3, z: 4 };

const r3 = Object.assign(a, b);
console.log(r3);       // { x: 1, y: 3, z: 4 }
console.log(a);        // { x: 1, y: 3, z: 4 }  (a가 바뀜)
console.log(a === r3); // true (r3는 a와 같은 객체)

// 3-2) Object.assign({}, a, b) : 새 객체에 결과를 담음(원본 유지)
const a2 = { x: 1, y: 2 };
const b2 = { y: 3, z: 4 };

const r4 = Object.assign({}, a2, b2);
console.log(r4);        // { x: 1, y: 3, z: 4 }
console.log(a2);        // { x: 1, y: 2 } (원본 유지)
console.log(b2);        // { y: 3, z: 4 } (원본 유지)
console.log(a2 === r4); // false (새 객체)

// 4) 반환값: 항상 target
const target = { base: true };
const r5 = Object.assign(target, { add: 1 });
console.log(r5);            // { base: true, add: 1 }
console.log(target);        // { base: true, add: 1 }
console.log(r5 === target); // true
```
</details>

<details>
    <summary>전개 연산자(...)로 배열의 각 값을 함수 인자로 전달</summary>

```javascript
function fn(x,y,z) {
    console.log(x, y, z)
}

const a = [1, 2, 3]

fn(...a) // console.log(1,2,3)
```
</details>

<br />

### 3-6. 구조 분해 할당(Destructuring assignment)
<details>
    <summary>배열 - 구조 분해 할당</summary>

```javascript
/**
 *  배열 - 구조 분해 할당에서 [,] 안의 각 위치는 배열의 인덱스(0, 1, 2 …)와 1:1로 대응한다
 *    특정 인덱스 값을 쓰지 않을 때는 그 자리를 “비워서 건너뛴다”
 *    이때 “자리를 비웠다”는 표시가 바로 ,(빈 슬롯)이다
 *    즉 [, b, c]는 “0번은 건너뛰고, 1번은 b, 2번은 c에 담겠다”는 의미다
*/

const arr = [1, 2, 3];

[,b,c] = arr;
console.log(b, c); // 2 3

/**
 *  관련 추가 설명
 *    자바스크립트는 보통 문장 끝의 ;를 생략할 수 있다(자동으로 세미콜론이 들어가는 것처럼 동작하는 경우가 많음).
 *    그런데 다음 줄이 [ 또는 ( 또는 ` 로 시작하면, 바로 앞 줄과 “한 문장으로 이어서 해석”될 수 있는 경우가 있다.
 *    배열 구조분해 할당처럼 [, b, c] = arr는 [로 시작하므로, 앞 줄이 세미콜론 없이 끝났을 때 의도치 않게 합쳐져 오류가 날 수 있다.
 *    그래서 안전하게 “이 줄은 새로운 문장이다”를 확실히 선언하려고 앞에 ;를 붙여서 시작한다.
 *    가장 깔끔한 방법은 애초에 이전 줄을 ;로 정상 종료시키는 것이고, 
 *    그게 어렵거나(예: 여러 코드 조각을 이어 붙일 때) 확실히 방어하고 싶으면 ;를 줄 앞에 붙이는 패턴을 쓴다.
*/
const arr = [1, 2, 3]
;[a,b,c] = arr
console.log(a, b, c) // 1 2 3
```
</details>

<details>
    <summary>구조 분해 할당에서의 나머지 연산자(Rest)</summary>

```javascript
/**
 *  구조 분해 할당, 남은 값들을 모아 하나의 변수로.
 *    ...rest는 구조 분해 할당에서 사용하지 않은 나머지 값들을 모아 하나의 변수로 만드는 문법이다
 *    배열 구조 분해에서는 남은 요소들을 배열로 모은다
 *    객체 구조 분해에서는 남은 속성들을 객체로 모은다
 *    rest는 예약어가 아니며 변수 이름일 뿐이라서, 의미에 맞게 다른 이름을 써도 된다
 */
const arr = [1, 2, 3];
const [a, ...rest] = arr;

console.log(a, rest); // 1, [2, 3]

// 객체일 때
const obj = { 
    a: 1,
    b: 2,
    c: 3,
    x: 7,
    y: 100 
}
const { c, ...rest } = obj; 
console.log(c, rest); // 3, { a: 1, b: 2, x: 7, y: 100 }
```
</details>

<details>
    <summary>★ 배열도 특정 요소를 제외한 나머지를 모으고 싶을 때</summary>

```javascript
/** 
 *  filter 콜백의 기본 형태
 *    arr.filter((value, index, array) => {
        // value : 요소 값
        // index : 인덱스
        // array : 원본 배열
      });
*/

// 방법 1. 값 기준으로 2 제외
const arr = [1, 2, 3];
const rest = arr.filter(v => v !== 2);
console.log(rest); // [1, 3]

// 방법 2. 인덱스 기준으로 두 번째 요소만 제외(= arr[1] 제외)
const arr = [1, 2, 3];
const rest = arr.filter((_, i) => i !== 1);
console.log(rest); // [1, 3]

// 방법 3. 분해 + 재조합(현재 예시처럼 3개일 때)
const arr = [1, 2, 3];
const [a, , c] = arr;      // 2는 자리만 비워서 건너뜀
const rest = [a, c];
console.log(rest); // [1, 3]
```
</details>

<details>
    <summary>객체 구조 분해: 기본값(디폴트 값) 지정</summary>

```javascript
/**
 *  객체 구조 분해에서 기본값(디폴트 값)을 지정하는 방식.
 *    { x = 4 }는 obj에서 x라는 속성(property)을 꺼내서 변수 x에 담겠다는 뜻이다.
 *    그런데 obj에는 x 속성이 없다.
 *    그래서 구조 분해 문법에 적어둔 기본값 4가 사용된다.
 *    결과적으로 x는 4가 된다.
 */
const obj = { a: 1, b: 2 }
const { x = 4 } = obj

console.log(x); // 4

/**
 *  obj에 x 속성이 없으면 obj.x는 undefined로 평가된다(콘솔에서 console.log(obj.x)를 찍으면 undefined가 나온다).
 *  구조 분해에서 기본값(x = 4)은 해당 값이 undefined일 때만 적용된다.
 *  즉, 속성이 아예 없거나 값이 undefined이면 기본값이 적용되지만, 
 *  null/0/""/false는 undefined가 아니므로 기본값이 적용되지 않는다.
 */
```
</details>

<details>
    <summary>객체 구조 분해에서 변수 이름 바꾸기</summary>

```javascript
const obj = { a: 1, b: 2 }
const { x = 4, a: heropy, y: ten = 10 } = obj

/**
 *  위 코드에서 객체 구조 분해에서는
 *    왼쪽 : 오른쪽 형태로 작성한다
 *      - 왼쪽은 객체의 키 이름
 *      - 오른쪽은 새로 만들 변수 이름
 *    따라서
 *      a: heropy는
 *      → obj.a 값을 꺼내서 heropy라는 변수에 담는다
 *    이때 a라는 변수는 생성되지 않는다
 *    만들어지는 변수는 heropy 하나뿐이다
 */

console.log(heropy); // 1
console.log(a);      // 에러 (a는 선언되지 않음)
console.log(ten);    // 10
console.log(y);      // 에러 (y는 선언되지 않음)
```
</details>

<br />

### 3-7. 선택적 체이닝
<details>
    <summary>?.는 null/undefined면 undefined를 반환한다.</summary>

```javascript
const userA = {
    name: 'heropy',
    age: 85,
    address: {
        country: 'korea',
        city: 'seoul'
    }
}

const userB = {
    name: 'Neo',
    age: 22
}

function getCity(user) {
    return user.address?.city
}

/**
 *  선택적 체이닝(?.)은 앞의 값이 null 또는 undefined일 때,
 *  다음 속성 접근을 멈추고 에러 대신 undefined를 반환한다.
 * 
 *  즉 user.address?.city는
 *  user.address가 있으면                   → user.address.city를 반환
 *  user.address가 없으면(undefined / null) → 여기서 멈추고 undefined 반환
 * 
 *  그래서 address가 없는 객체(userB)에서도
 *  “Cannot read properties of undefined” 같은 런타임 에러를 피할 수 있다.
 * 
 *  이 코드에 적용하면:
 *  userA는 address.city가 있으므로 "seoul" 출력
 *  userB는 address가 없으므로 undefined 출력 (에러 아님)
 */

console.log(getCity(userA)); // seoul
console.log(getCity(userB)); // undefined

/**
 *  즉 결론은
 *    에러로 프로그램이 깨지는 대신
 *    undefined라는 결과로 “없음”을 안전하게 전달해 주는 수단이라고 보면 됩니다.
 */
```
</details>

<details>
    <summary>선택적 체이닝 + 기본값 처리 (?. 와 ||)</summary>

```javascript
const userA = {
    name: 'heropy',
    age: 85,
    address: {
        country: 'korea',
        city: 'seoul'
    }
}

const userB = {
    name: 'Neo',
    age: 22
}

function getCity(user) {
    return user.address?.city || '주소 없음'
}

/**
 *  user.address?.city는 address가 없을 경우 에러 대신 undefined를 반환한다.
 *  || 연산자는 왼쪽 값이 거짓으로 판정되면 오른쪽 값을 반환한다.
 *  따라서 city 값이 존재하면 그대로 반환되고,
 *  address가 없어서 undefined가 나오면 '주소 없음'이 반환된다.
 */
console.log(getCity(userA)) // 'seoul'
console.log(getCity(userB)) // '주소 없음'

/**
 *  정리하면 이 코드는
 *  선택적 체이닝으로 런타임 에러를 피하고, ||로 사람이 읽을 수 있는 기본값을 제공하는 패턴이다.
*/
```
</details>

<br />

### 3-8. if, Switch 조건문
<details>
    <summary>if 조건문</summary>

```javascript
if (조건) {
    // 실행
}

if (조건) {
    // 실행
} else {
    // 실행    
}

if (조건1) {
    // 실행
} else if (조건2) {
    // 실행    
} else if (조건3) {
    // 실행    
} else {
    // 실행
}

// 예시
function isPositive(number) {
    if (number > 0) {
        return '양수';
    } else if (number < 0) {
        return '음수';
    } else {
        return '0'
    }
};

console.log(isPositive(1));  // 양수
console.log(isPositive(10)); // 양수 
console.log(isPositive(-2)); // 음수
console.log(isPositive(0));  // 0
```
</details>

<details>
    <summary>switch 조건문</summary>

```javascript
switch(조건) {
    case 값1 :
        // 조건이 값1 일 때 실행
    break;

    case 값2 :
        // 조건이 값2 일 때 실행
    break;

    default:
        // 조건이 '값1'도 '값2'도 아닐 때 실행
}

// 예시
function price(fruit) {
    let p;

    swith (fruit) {
        case 'Apple':
            p = 1000;
            break;
        case 'Banana':
            p = 1500;
            break;
        case 'Cherry':
            p = 2000;
            break;
        default:
            p = 0;
            break;
    }
}

console.log('Apple');  // 1000
console.log('Banana'); // 1500
console.log('Cherry'); // 2000
console.log('orange'); // 0

// breack 대신 return 을 사용하여 종료 할 수도 있다.
function price(fruit) {
    swith (fruit) {
        case 'Apple':
            return 1000;
        case 'Banana':
            return 1500;
        case 'Cherry':
            return 2000;
        default:
            return 0;
    }
}

console.log('Apple');  // 1000
console.log('Banana'); // 1500
console.log('Cherry'); // 2000
console.log('orange'); // 0
```
</details>

<br />

### 3-9. For, For of, For in 반복문
<details>
    <summary>For 반복문</summary>

```javascript
/**
 *  for(초기화; 조건; 증감) {
 *    // 반복 실행할 코드
 *  }
 */
for(let i = 9; i > -1; i -= 1) {
    console.log(i); // 9 > 8 > 7 > 6 > 5 > 4 > 3 > 2 > 1 > 0
}

/**
 *  for문에서 break, continue 차이.
 *    - break : 
 *        현재 반복문을 즉시 종료하고, 반복문 다음 코드로 넘어간다.
 *    - continue :
 *        현재 반복을 즉시 끝내고, 반복문을 종료하지는 않은 채 다음 반복(다음 회차)으로 넘어간다.
 */
// breack 예시
for(let i = 9; i > -1; i -= 1) {
    if (i < 4) {
        break;
    }

    console.log(i); // 9 > 8 > 7 > 6 > 5 > 4
}

// continue 예시
for(let i = 9; i > -1; i -= 1) {
    if (i % 2 === 0) {
        continue;
    }

    console.log(i); // 9 > 7 > 5 > 3 > 1
}
```
</details>

<details>
    <summary>For of 반복문</summary>

```javascript
/**
 *  배열 순회에 적합한 for...of
 *    배열처럼 “순서가 있는 값들”을 처음부터 끝까지 순회하면서 값 자체가 필요할 때 for...of를 쓰면 가장 간결합니다.
 *    인덱스가 필요 없고, length/i++ 같은 반복 제어가 불필요할 때 적합합니다.
 */
const fruits = ['apple', 'banana', 'cherry']

// for 문을 사용했을 때
// for (let i = 0; i < fruits.length; i++) {
//     console.log(fruits[i]); // apple > banana > cherry
// }

// for ...of 문
for (const fruit of fruits) {
    console.log(fruit); // apple > banana > cherry
}

/**
 *  값만 필요하면 for...of,
 *  인덱스/횟수/범위/방향을 컨트롤해야 하면 for.
*/

// 객체 일 때
const users = [
    {
        name: 'heropy',
        age: 85,
    },
    {
        name: 'neo',
        age: 22,
    }
]

// for 문을 사용했을 때
// for (let i = 0; i < users.length; i += 1) {
//     console.log(users[i].name, users[i].age); // heropy 85 > neo 22
// }

for (const {name, age} of users) {
    console.log(name, age) // heropy 85 > neo 22
}

```
</details>

<details>
    <summary>For in 반복문</summary>

```javascript
/**
 *  for...in 문 사용 시점 (객체 순회)
 *    for...in은 객체의 키(프로퍼티 이름)를 순회한다.
 *    반복 변수 key에는 값이 아니라 키 문자열이 들어온다.
 *    값이 필요하면 user[key]처럼 대괄호 표기법으로 접근한다.
 *    객체의 각 속성을 하나씩 검사·처리할 때 적합하다
 *    - 예: 필드 유효성 검사, 특정 키 필터링, 로그/변환 작업 등
 */
const user = {
    name: 'Heropy',
    age: 85,
    isValid: true,
    email: 'thesecon@gmai.com',
}

for (const key in user) {
    console.log(key)       // 키
    console.log(user[key]) // 값
}

// 안전하게 쓰는 패턴
for (const key in user) {
    if (Object.hasOwn(user, key)) {
        console.log(key)
        console.log(user[key])
    }
}
```
</details>

<details>
    <summary>for...in 사용 시 주의점 - 상속 속성</summary>

```javascript
/**
 *  for...in은 상속된 열거 속성이 섞일 수 있으므로,
 *  필요하면 Object.hasOwn()로 자기 속성만 처리한다.
*/

// 1) obj는 "처음엔 빈 객체"인데, parent를 부모로 연결한 상태
const parent = { inherited: 1 };
const obj = Object.create(parent); // obj = {} (own 속성 없음)
obj.own = 2;                       // obj = { own: 2 }

// 2) obj에 직접 있는지(own) 확인
console.log('obj own 키:', Object.keys(obj));                     // ['own']
console.log('own은 내꺼?', Object.hasOwn(obj, 'own'));             // true
console.log('inherited는 내꺼?', Object.hasOwn(obj, 'inherited')); // false

// 3) 그런데 for...in은 상속(inherited)까지 돌 수 있음
for (const k in obj) console.log('for...in:', k);
// 출력 예: for...in: own
//        for...in: inherited

// 4) 그래서 for...in 쓸 때는 "내꺼만" 처리하려면 이렇게 걸러야 함
for (const k in obj) {
  if (Object.hasOwn(obj, k)) console.log('내꺼만:', k);
}
// 출력 예: 내꺼만: own

```
</details>

<details>
    <summary>for / for...in / for...of 사용 기준</summary>

```javascript
/**
 *  for 반복문
 *    - 인덱스/범위/방향 제어가 필요
 * 
 *  for...of
 *    - 배열의 값을 순회
 * 
 *  for...in
 *    - 객체의 키/값을 순회해야 할 때
*/
```
</details>

<br />

### 3-10. While, Do while 반복문
<details>
    <summary>While 반복문</summary>

```javascript
let n = 0;

/**
 *  while은 조건이 true인 동안 반복하고, 조건이 false가 되는 순간 반복을 종료한다.
 *  반복 횟수가 “정해져 있는지”보다, 어떤 조건이 만족될 때까지 계속 반복해야 하는 상황에 적합하다.
 * 
 *  주의할 점은 반복문 안에서 조건을 변화시키는 코드(예: n += 1)가 없으면 
 *  조건이 계속 true로 남아 무한 루프가 될 수 있다.
*/

while (n < 4) {
    console.log(n); // 0 > 1 > 2 > 3
    n += 1;
}
```
</details>

<details>
    <summary>Do while 반복문</summary>

```javascript
let n = 0;

/**
 *  do { ... } while (조건);
 * 
 *    do…while의 기본 형태다.
 *    본문을 먼저 1회 실행한 뒤, 조건을 검사해서 반복 여부를 결정한다.
*/
do {
    // 실행
} while (조건);

do {
    console.log(n);
    n += 1; // 0 > 1 > 2 > 3 > 4
} while(n < 4);

/**
 *  while (n)은 조건을 먼저 검사한다.
 *  n이 0이면 거짓이므로, console.log(n)은 한 번도 실행되지 않는다.
*/
while (n) {
    console.log(n);
}

/**
 *  do { console.log(n); } while (n);
 *  
 *    위 규칙 때문에
 *    n이 0이어도 console.log(n)이 최소 1번 실행되고,
 *    이후 while (n) 조건이 거짓이라 반복이 종료된다.
 * 
 *  do while 반복문을 사용할 때?
 *    사용자 입력을 한 번은 무조건 받아야 하는 경우
 *    초기 화면 표시, 초기 로그 출력처럼 첫 실행이 전제인 로직.
 *    반복 여부를 사후 조건으로 판단해야 할 때
 *    “한 번은 무조건 실행돼야 하면 do…while”
*/
do {
    console.log(n); // 0
} while (n);
```
</details>

<br />
