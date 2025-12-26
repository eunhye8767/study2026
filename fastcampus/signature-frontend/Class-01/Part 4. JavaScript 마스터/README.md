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

**Homebrew는 macOS의 패키지 관리 툴 :**
> 맥북 사용자라면 필수적으로 설치해야할 프로그램 중 하나이다.
> 간단한 명령어로 다양한 소프트웨어를 설치, 관리, 제거할 수 있어 매우 유용하다!
> (macOS, Homebrew로 `npm`, `nvm`, `git` 설치)
> [Homebrew 공홈 바로가기](https://brew.sh/ko/)

<br />
<hr />
<br />

## 목차
- [Ch 01. Node JS](#ch-01-node-js)
  - [Node.js 다운로드](#nodejs-다운로드)
  - [npm](#npm)
  - [npm을 사용하는 이유](#npm을-사용하는-이유)
  - [CDN vs npm](#cdn-vs-npm)
    - [CDN 방식](#cdn-방식)
    - [npm 방식](#npm-방식)
  - [npm 설치](#npm-설치)
  - [Parcel, 개발 서버 실행과 빌드](#parcel-개발-서버-실행과-빌드)
  - [유의적 버전(Semver)](#유의적-버전semver)
- [Ch 02. JS 데이터](#ch-02-js-데이터)

<br />
<hr />
<br />

## Ch 01. Node JS
`Node.js`는 Chrome V8 JavaScript 엔진으로 빌드된 **JavaScript 런타임**(프로그래밍 언어가 동작하는 환경).  

### ♦︎ Node.js 다운로드
- [node 공식 - 다운로드](https://nodejs.org/ko/download)
- 환경에 맞게 다운로드 설치.

### ♦︎ npm
- `npm`(node package manager)은 전 세계의 개발자들이 만든 다양한 기능(패키지, 모듈)들을 관리.
- `npm install ???` 으로 설치하여 사용 할 수 있다.

### ♦︎ npm을 사용하는 이유
`node.js` 환경에서는 `npm`을 통해 필요한 패키지를 직접 설치하고 버전을 관리하며 사용한다.<br />
이 방식은 초기에는 설정과 개념을 이해해야 해서 다소 복잡하지만,<br />
의존성 관리와 확장성이 뛰어나 프로젝트를 체계적으로 관리할 수 있다.<br />
그 결과, 비교적 적은 시간으로도 복잡한 기능을 안정적으로 추가하고 고도화할 수 있다.<br />
이처럼 초기 복잡함을 감수하고 장기적인 효율을 얻는 선택을 트레이드 오프라고 한다.<br />

### ♦︎ CDN vs npm
#### CDN 방식
- 라이브러리를 외부 서버에서 바로 불러와 사용
- 설정이 거의 없고 빠르게 시작 가능
- 프로젝트가 커지면 파일 여러 곳에 링크가 흩어져 버전/의존성 관리가 어려움

#### npm 방식
- 라이브러리를 프로젝트 안에 설치해서 사용
- 설정과 빌드 과정이 필요함
- `package.json`(및 lock 파일)에 사용 패키지/버전이 기록되어 **버전 고정·의존성 관리·환경 재현에 유리**함
- 팀/대규모 프로젝트에 특히 적합
- [npm 공홈 바로가기](https://www.npmjs.com/)

### ♦︎ npm 설치
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

### ♦︎ Parcel, 개발 서버 실행과 빌드
- [Parcel(파셀) 공홈, 바로가기](https://parceljs.org/)
- `index.html` 파일 생성
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

- `main.js` 파일 생성
    ```javascript
    import _ from "lodash"

    console.log(_.upperCase("hello-world"))
    ```

- `package.json` : dev, build 명령어 만들기
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

-  `main.ts` 파일 생성 : `Parcel(파슬)`은 ts(타입스크립트)도 지원해 준다.
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

### ♦︎ 유의적 버전(Semver)
```html
Major.Minor.Patch
4.17.21 

Major : 기존 버전과 호환되지 않는 새로운 버전.
Minor : 기존 버전과 호환되는 기능이 추가된 버전.
Patch : 기존 버전과 호환되는 버그 및 오타 등이 수정된 버전.

---

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

---

~Major.Minor.Patch
~4.17.21 

Minor 버전 안에서 가장 최신 버전으로 업데이트 가능
즉, Patch 버전만 가장 최신 버전으로 업데이트 된다는 개념.
```

<br />
<hr />
<br />

## Ch 02. JS 데이터