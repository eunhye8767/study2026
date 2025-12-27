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

<details open>
    <summary>목차</summary>

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